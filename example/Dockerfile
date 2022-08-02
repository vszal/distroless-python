## Example Dockerfile for Python Flask app running in gunicorn
## Set your builder repo location
FROM repo/python-distroless-builder:latest as build

# create venv
RUN python -m venv /home/nonroot/venv
# install deps
COPY requirements.txt .
RUN /home/nonroot/venv/bin/pip install -r requirements.txt

## Set your run image location
FROM repo/python-distroless:latest
ENV PYTHONUNBUFFERED=1

# nonroot
ENV HOME=/home/nonroot

WORKDIR $HOME
# copy files and python deps
COPY --from=build $HOME/venv $HOME/venv
COPY --chown=nonroot:nonroot . .

# change user context to nonroot
USER 65532
# needed for gunicorn
ENV PYTHONPATH=${HOME}/venv/lib/python3.10/site-packages
ENV GUNICORN_CMD_ARGS="--workers 2 --threads 2 -b 0.0.0.0:8080 --chdir $HOME"
# Run the web service on container startup.
CMD ["gunicorn",  "app:app"]
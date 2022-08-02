## Example Dockerfile for Python Flask app running in gunicorn
## Set your builder repo location here
FROM path/to/python-distroless-builder:latest as build

# create venv
RUN python -m venv /home/nonroot/venv
# install deps
COPY requirements.txt .
RUN /home/nonroot/venv/bin/pip install -r requirements.txt

FROM /path/to/python-distroless:latest
ENV PYTHONUNBUFFERED=1

# nonroot
ARG USER=nonroot
ENV HOME=/home/$USER

WORKDIR $HOME
# copy files and python deps
COPY --from=build /home/nonroot/venv $HOME/venv
COPY --chown=nonroot:nonroot . .

# change user context to nonroot
USER 65532
# needed for gunicorn
ENV PYTHONPATH=/home/nonroot/venv/lib/python3.10/site-packages
ENV GUNICORN_CMD_ARGS="--workers 2 --threads 2 -b 0.0.0.0:8080 --chdir $HOME"
# Run the web service on container startup.
CMD ["gunicorn",  "app:app"]
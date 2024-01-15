FROM mcr.microsoft.com/devcontainers/python:3.10-bullseye
ENV PYTHONUNBUFFERED=1
# RUN pip install ipykernel
COPY pyproject.toml ./

RUN pip install poetry
RUN poetry config virtualenvs.create false \
    && poetry install --no-root
RUN poetry run python -m ipykernel install --user --name docker-pystan
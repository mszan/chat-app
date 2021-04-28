# pull official base image
FROM python:3

# prevents Python from buffering stdout and stderr (equivalent to python -u option)
ENV PYTHONUNBUFFERED 1

WORKDIR /app/backend/requirements

# install dependencies
COPY requirements/base.txt .
COPY requirements/dev.txt .

RUN pip install -r dev.txt

WORKDIR /app/backend

COPY . .
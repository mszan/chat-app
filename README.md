**Important note:** Please keep in mind that this app is in its very early stage, be forgiving.
# Chat App
![](https://i.imgur.com/dIe8CVP.png)

## General info
This is a dockerized real-time web chat app built with ReactJS, Django Rest Framework, WebSockets and Postgres.

In details, [backend](https://github.com/mszan/chat-backend) is built on top of [Django](https://www.djangoproject.com/) and uses: [Django Rest Framework](https://www.django-rest-framework.org/), [Django Channels](https://channels.readthedocs.io/en/stable/), [Postgres](https://www.postgresql.org/). [Frontend](https://github.com/mszan/chat-frontend) is built on top of [ReactJS](https://reactjs.org/) and uses [Ant Design](https://ant.design/) as its main component library.

## Usage
### Development
This setup is ready for developing inside [VSCode Dev Container](https://code.visualstudio.com/docs/remote/containers).

1. Start [docker-sync](http://docker-sync.io/) and wait for it to get up:
    ```
    docker-sync start
    ```
2. Run the Docker Compose:
   - If you're using VSCode, use ``devcontainer.json`` file in ``frontend`` or ``backend`` directory as described [here](https://code.visualstudio.com/docs/remote/containers#_quick-start-try-a-development-container).
   - If you're **not** using VSCode, start the development Docker Compose directly by running:
   ```
   docker-compose -f docker-compose.development.yml up
   ```
3. Make Django migrations from the inside of the container.
    ```
    docker exec -it chat-app_backend /bin/bash
    ```
    ```
    root@foo:/app# python3 manage.py makemigrations
    ```
    ```
    root@foo:/app# python3 manage.py migrate
    ```
4.  **Done!** To verify it's working, check the exposed ports inside [docker-compose.development.yml](docker-compose.development.yml) file.


### Production
Although this repository has been equipped with CI/CD to handle production deployment, the following section describes a manual setup.

This setup requires you to pass the following args:

- **SERVICE_POSTGRES_PASSWORD** (e.g.: *postgres*) - [database password](https://hub.docker.com/_/postgres)

- **SERVICE_POSTGRES_DATA_LOC** (e.g.: */p)ath/to/postgres/data* - [database external volume location](https://hub.docker.com/_/postgres)

- **SERVICE_POSTGRES_PORT** (e.g.: *6030*) - exposed to host database port

- **SERVICE_REDIS_PORT** (e.g.: *6020*) - exposed to host redis port

- **SERVICE_BACKEND_PORT** (e.g.: *6010*) - exposed to host backend port

- **SERVICE_BACKEND_DJANGO_SETTINGS_MODULE** (e.g.: *src.)settings.production* - [django settings module env variable](https://docs.djangoproject.com/en/3.2/topics/settings/#envvar-DJANGO_SETTINGS_MODULE)

- **SERVICE_BACKEND_DJANGO_SECRET_KEY** (e.g.: *FOO123*) - [django settings secret key](https://docs.djangoproject.com/en/3.2/ref/settings/#secret-key)

- **SERVICE_FRONTEND_ARG_REACT_APP_BACKEND_WEBSOCKET_URL** (e.g.: *wss://example. - pl/ws/*) - backend WebSocket endpoint

    **Note:** This needs a [web server WebSocket reverse proxy](https://www.nginx.com/blog/websocket-nginx/) to be set.

- **SERVICE_FRONTEND_ARG_REACT_APP_BACKEND_API_URL** (e.g.: *https://example.com/api*) - backend base API endpoint

- **SERVICE_FRONTEND_PORT** (e.g.: *6000*) - exposed to host frontend port

Besides that, you should do the following:

1. Setup a Web Server with a reverse proxy to exposed ports for both backend and frontend.
2. Run the Docker Compose in deteached mode.

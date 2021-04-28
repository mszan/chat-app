production# Stage 1
## Install and build React app.

FROM node:lts-alpine as build-stage

WORKDIR /app

ENV PATH /app/node_modules/.bin:$PATH

COPY package.json /app/

RUN npm install

COPY ./ /app/

RUN npm run build


# Stage 2
## Serve built React app with nginx.

FROM nginx:1.20.0-alpine

COPY --from=build-stage /app/build /usr/share/nginx/html

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.production.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

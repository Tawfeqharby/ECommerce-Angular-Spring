# base image
FROM node:14.15.5-alpine3.13 as node

# set working directory
WORKDIR /usr/local/app
COPY ./frontend /usr/local/app/

# install and cache app dependencies
RUN npm install --legacy-peer-deps
RUN npm install -g @angular/cli
RUN npm i @angular-devkit/build-angular
RUN npm run-script build

# Run in NGINX #
FROM nginx:latest
COPY --from=node /usr/local/app/dist/frontend /usr/share/nginx/html
COPY --from=node /usr/local/app/ngnix.conf /etc/nginx/nginx.conf
EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]

FROM node:18 AS build
WORKDIR /app
COPY task-manager-frontend/ .
RUN npm install
RUN npm run build

FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
COPY ../default.conf /etc/nginx/conf.d/default.conf

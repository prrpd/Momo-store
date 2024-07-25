FROM mirror.gcr.io/node:16.20.0-alpine3.18 as builder
WORKDIR /app
COPY ./public ./public
COPY ./src ./src
COPY *.json *.js ./
RUN npm install \ 
    bash -c "NODE_ENV=production VUE_APP_API_URL=http://localhost:8081 npm run serve" \
    && npm run build

# шаг релиза
FROM mirror.gcr.io/nginx:stable-alpine-slim
WORKDIR /app
COPY --from=builder /app/dist /usr/share/nginx/html/
EXPOSE 80
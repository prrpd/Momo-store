FROM mirror.gcr.io/node:16.20.0-alpine3.18 as builder
# ARG NODE_ENV=production
# ARG VUE_APP_API_URL=http://localhost:8081
WORKDIR /app
COPY . .
RUN npm install 
RUN NODE_ENV=production VUE_APP_API_URL=/ npm run build

#шаг релиза
FROM mirror.gcr.io/nginx:stable-alpine-slim
COPY --from=builder /app/dist /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
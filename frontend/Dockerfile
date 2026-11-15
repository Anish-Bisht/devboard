# Stage 1 — build the app into static files
FROM node:20-alpine3.21 AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --legacy-peer-deps
COPY . .
RUN npm run build

# Stage 2 — Minimal runtime
FROM alpine:3.21
RUN apk update && apk add --no-cache nginx=1.26.3-r3 zlib=1.3.2-r1
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/http.d/custom.conf
EXPOSE 4173
CMD ["nginx", "-g", "daemon off;"]

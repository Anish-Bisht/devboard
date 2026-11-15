# Stage 1 — build the app into static files
FROM node:20-alpine3.21 AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci --legacy-peer-deps
COPY . .
RUN npm run build

# Stage 2 — Minimal runtime with updated zlib
FROM alpine:3.21
# Update packages to get fixed zlib 1.4-r0
RUN apk update && apk upgrade && apk add --no-cache nginx zlib>=1.4-r0
COPY --from=build /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/http.d/custom.conf
EXPOSE 4173
CMD ["nginx", "-g", "daemon off;"]

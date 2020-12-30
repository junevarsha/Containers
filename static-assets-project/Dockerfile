FROM node:latest AS builder
WORKDIR /app
COPY . .
RUN npm ci && npm run build

# you could totally use nginx:alpine here too
FROM nginx:latest
COPY --from=builder /app/build /usr/share/nginx/html
# stage1 as builder
FROM node:20.7.0-alpine3.18 as builder

WORKDIR /app

# Copy the package.json and install dependencies
COPY package*.json ./
RUN npm install -g pnpm

RUN pnpm install

# Copy rest of the files
COPY . .

# COPY ./.production/.env.production ./.env

ENV NODE_ENV production

# Build the project
RUN pnpm run build


FROM fholzer/nginx-brotli as production-build
COPY ./.production/.nginx/nginx.conf /etc/nginx/nginx.conf

## Remove default nginx index page
RUN rm -rf /usr/share/nginx/html/*

# Copy from the stahg 1
COPY --from=builder /app/.output /usr/share/nginx/html

EXPOSE 80
ENTRYPOINT ["nginx", "-g", "daemon off;"]

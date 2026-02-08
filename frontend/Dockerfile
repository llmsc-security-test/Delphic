# Build container
FROM node:16-alpine as builder

WORKDIR /app
COPY package.json .
COPY yarn.lock .
RUN yarn cache clean&& yarn install
COPY . .
RUN yarn build

# Run container
FROM nginx:1.22.0-alpine

LABEL version="1.0"
LABEL description="Base docker image for Delphic - a simple framework to harness LLMs for knowledge transformation"
LABEL maintainer = ["scrudato@umich.edu"]

# Run and copy server config
RUN rm -rf /etc/nginx/conf.d
COPY conf /etc/nginx

# Copy static build files
COPY --from=builder /app/build /usr/share/nginx/html/

# Expose port 3000
EXPOSE 3000

#Make a copy of env file and shell scripts in container
WORKDIR /usr/share/nginx/html
COPY ./env.sh .
COPY ./.env .

# Make shell scripts to load .env variables executable
RUN chmod +x env.sh

# Let's start our server

# --- Auto-Generated Entrypoint Configuration ---
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
RUN mkdir -p /var/log && touch /var/log/app.log
EXPOSE 3000
ENTRYPOINT ["/entrypoint.sh"]

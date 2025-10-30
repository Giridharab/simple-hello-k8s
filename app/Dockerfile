# Simple NGINX-based app
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
RUN groupadd -for docker && usermod -aG docker jenkins

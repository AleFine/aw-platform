FROM nginx:1.28.0-alpine-slim

# Metadata
LABEL maintainer="kevin@example.com"
LABEL version="1.0.0"
LABEL description="Hello World application for AWS EKS deployment"

# nginx user directories for running as non-root
RUN mkdir -p /var/cache/nginx/client_temp \
    && mkdir -p /var/cache/nginx/proxy_temp \
    && mkdir -p /var/cache/nginx/fastcgi_temp \
    && mkdir -p /var/cache/nginx/uwsgi_temp \
    && mkdir -p /var/cache/nginx/scgi_temp \
    && chown -R nginx:nginx /var/cache/nginx \
    && chown -R nginx:nginx /var/log/nginx \
    && chown -R nginx:nginx /etc/nginx/conf.d \
    && touch /var/run/nginx.pid \
    && chown -R nginx:nginx /var/run/nginx.pid

COPY nginx.conf /etc/nginx/nginx.conf

COPY html/ /usr/share/nginx/html/

# permissions html directory
RUN chown -R nginx:nginx /usr/share/nginx/html

# non-root user
USER nginx

EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost/health || exit 1

# nginx in foreground
CMD ["nginx", "-g", "daemon off;"]

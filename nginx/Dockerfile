FROM nginx:stable-alpine

ARG CERTBOT_EMAIL=info@domain.com
ARG DOMAIN_LIST=yourdomain

RUN apk update \
    && apk upgrade \
    && apk add apk-cron certbot certbot-nginx \
    && certbot certonly --nginx --dry-run --agree-tos -m "${CERTBOT_EMAIL}" -n -d ${DOMAIN_LIST} \
    && echo -e '#!/bin/sh \n' \
               'certbot renew --dry-run --nginx >> /var/log/cron.log 2>&1' > /etc/periodic/monthly/certbot-renew \
    && chmod +x /etc/periodic/monthly/certbot-renew

VOLUME /etc/letsencrypt

CMD sh -c crond && nginx -g 'daemon off;'
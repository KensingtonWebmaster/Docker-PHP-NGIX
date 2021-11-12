FROM nginx:latest

COPY ./nginx-modified.conf /etc/nginx/nginx.conf

COPY ./site.conf /etc/nginx/conf.d/default.conf

COPY ./domain.crt /etc/ssl/domain.crt
COPY ./newkey.pem /etc/ssl/domain.pem

COPY ./certs/midwest/MIDWESTTORONTOOHT.crt /etc/ssl/domain2.crt
COPY ./certs/midwest/midwesttorontooht_ca.key /etc/ssl/domain2.key

commands:-\

docker-compose up -d nginx\
docker-compose run --rm certbot certonly --webroot --webroot-path=/var/lib/letsencrypt  -d api.vecna.online --email vikasisgen@gmail.com --agree-tos --no-eff-email\

create reverse-ssh-tunnel, prevents termination\
autossh -M 0 -p 2599 -o ServerAliveInterval=60 -o ServerAliveCountMax=3 -N -R 0.0.0.0:80:localhost:80 -R 0.0.0.0:443:localhost:443 root@103.235.106.167\
# docker-ssl-tunnel\

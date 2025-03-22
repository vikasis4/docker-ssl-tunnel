Folder structure
    app/    
        docker-compose.yaml
        nginx/  
            nginx.conf
        certbot/
            www/
            conf/


Docker-compse.yaml file

version: '3'

services:
  nginx:
    image: nginx:latest
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/lib/letsencrypt
    ports:
      - "80:80"
      - "443:443"

  certbot:
    image: certbot/certbot
    volumes:
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/lib/letsencrypt

volumes:
  certbot-etc:
  certbot-var:


nginx/nginx.conf file

events {}
http {
   server {
        listen 80;
        server_name vecna.online api.vecna.online;

        location /.well-known/acme-challenge/ {
            root /var/lib/letsencrypt;
            try_files $uri = 404;
        }

        location / {
            return 301 https://$host$request_uri;
        }
    }

}


commands:-

docker-compose up -d nginx
docker-compose run --rm certbot certonly --webroot --webroot-path=/var/lib/letsencrypt  -d api.vecna.online --email vikasisgen@gmail.com --agree-tos --no-eff-email

create reverse-ssh-tunnel, prevents termination 
autossh -M 0 -p 2599 -o ServerAliveInterval=60 -o ServerAliveCountMax=3 -N -R 0.0.0.0:80:localhost:80 -R 0.0.0.0:443:localhost:443 root@103.235.106.167
# docker-ssl-tunnel

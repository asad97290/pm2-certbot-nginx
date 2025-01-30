## pm2 setup
- pm2 init
```javascript
module.exports = {
  apps : [{
          name:"explorer-frontend",
          script:"yarn dev"
  }]
};

```
- pm2 start ecosystem.config.js
- pm2 log

## delete pm2 log files
pm2 flush

## enable log rotation
- pm2 install pm2-logrotate

- pm2 set pm2-logrotate:max_size 50M      
- pm2 set pm2-logrotate:retain 1          
- pm2 set pm2-logrotate:compress false    
- pm2 set pm2-logrotate:rotateInterval '0 * * * *' 

- pm2 restart pm2-logrotate

- pm2 show pm2-logrotate



## nginx and certbot setup

- sudo apt install nginx
- sudo apt install certbot python3-certbot-nginx
- sudo nginx -t
- vim /etc/nginx/conf.d/default.conf
```shell
server {
    listen 80;
    server_name rpc.agensensus.ai;

    location / {
        proxy_pass http://127.0.0.1:3001; 
        proxy_redirect off;
    }
}

```
- sudo systemctl restart nginx
- sudo systemctl status nginx

- sudo certbot --nginx -d explorer-api.agensensus.ai


## setup cronjob for certbot renewal 
- sudo crontab -l
- sudo crontab -e
- 0 3 1 * * certbot renew --quiet --post-hook "systemctl reload nginx"
- sudo systemctl status cron
- sudo systemctl start cron
## fow renewal without cronjob 
- sudo certbot renew --dry-run
- sudo systemctl reload nginx
- sudo systemctl status nginx




## view ec2 disk space
df -h

## grow ec2 space

- lsblk
- sudo apt update
- sudo apt install -y cloud-guest-utils
- sudo growpart /dev/xvda 1
- sudo resize2fs /dev/xvda1
- sudo xfs_growfs /



## docker commands

- docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Ports}}"



## postgres

- su - postgres

- psql
- DROP DATABASE blockscout;
- for exisiting: \q
- exit
- sudo systemctl restart postgresql

- This will list the process IDs (pid) of active connections.

```javascript
SELECT pid, usename, application_name, client_addr 
FROM pg_stat_activity 
WHERE datname = 'blockscout';
```
- Now, forcefully disconnect all sessions using:
```javascript
SELECT pg_terminate_backend(pid) 
FROM pg_stat_activity 
WHERE datname = 'blockscout';
```
https://docs.blockscout.com/setup/deployment/manual-deployment-guide/ubuntu-setup



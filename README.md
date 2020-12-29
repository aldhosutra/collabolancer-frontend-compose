# collabolancer-frontend-compose

Docker Compose Structure to Easily Deploy Collabolancer Client, integrated with Lisk Blockchain Explorer, and Lisk Faucet

## Setup Certbot

To setup https for your domain, change the <yourdomain.com> to your respective domain, and run this command:

```
sudo docker-compose up -d
sudo docker-compose exec collabolancer-client sh
certbot --nginx -d <yourdomain.com> -d <www.yourdomain.com>
```

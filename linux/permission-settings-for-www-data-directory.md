# Permission settings for www-data directory

매번 헷갈리고 매번 찾아보기도 귀찮다

```
sudo adduser ubuntu www-data
sudo chown -R www-data:www-data /var/www/static
sudo chmod -R g+rwX /var/www/static
```

# Prefetch

If you want to support multi path in your server, we have a better idea: just make a symbolic link

```sh
ln -s /var/www/li/pic /var/www/hexo/public/pic
```

And make necessary changes in each of your nginx's config:
```
proxy_pass http://127.0.0.1:3333/pic1;
```
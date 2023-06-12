# MultiPath

If you want to support multi path in your server, currently we have idea of just make a symbolic link, for example if your `IMG_PATH` is `/path/to/pics` and you'd like to serve some images under `/another-path/to/another-domain`, you can symbol link that path to `/path/to/pics`, command example as below

```sh
ln -s /another-path/to/another-domain /path/to/pics
```

Now, all the requests to `http://127.0.0.1:3333/another-domain/some-image.jpg` will serve `some-image.jpg` under `/another-path/to/another-domain`

And make necessary changes in each of your nginx's config when you'd like to serve images under that path:

```
location ~* \.(?:jpg|jpeg|gif|png|bmp)$ {
    rewrite ^/(.*)$ /another-domain.com/$1 break;
    proxy_pass http://127.0.0.1:3333;
}
```

There is an going discussion of a new configuration format that will support multipath in configuration rather than manually creaing symbolic link, related GitHub Issue at: [Draft for new configuration format · Issue #217 · webp-sh/webp_server_go](https://github.com/webp-sh/webp_server_go/issues/217), you can follow the progress there.
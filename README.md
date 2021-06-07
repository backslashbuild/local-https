# How to set up locally

1. install and set up `mkcert`
2. generate the certificates into your certs directory:

```
mkcert -cert-file certs/local-cert.pem -key-file certs/local-key.pem "dev.localhost" "*.dev.localhost"
```

3. use docker to run `backslashbuild/local-https`, ensuring to pass in:

   - an APP_URL to point at your app
   - a volume that maps your certificates folder to `/etc/certs`

   You can use this docker compose as an example:

   ```
   version: "3.7"

   services:
     proxy:
       image: backslashbuild/local-https
       environment:
         - APP_URL=http://hello-world
       ports:
         - 80:80
         - 443:443
       volumes:
         - ./certs:/etc/certs:ro

     hello-world:
       image: tutum/hello-world
   ```

   Open `https://dev.localhost` in your browser!

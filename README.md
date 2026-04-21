Part:1-

I have create a folder by:

    mkdir -p ~/secure-app
then:
    ~/secure-app/index.html

    Inside the html file: 
        <h1>Secure Server Running via Nginx</h1>

    then setup nginx configuration.

    then restart nginx by:
        brew services restart nginx
if I browse: http://localhost:8080
    then the index page showing via nginx.

Part:2
    Generate self signed ssl(365)
    using: 
            openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
            -keyout ~/nginx-ssl/secure-app.key \
            -out ~/nginx-ssl/secure-app.crt
     and stored in /etc/nginx/ssl/

Part:3
    create custom config in nginx.config
        server {
    listen 80;
    server_name localhost;

    return 301 https://$host$request_uri;
}

    server {
        listen 443 ssl;
        server_name localhost;
    }

    with 80 and 443 port redirect.

Part 4:
    Run backend on port 3000 using:
        python3 -m http.server 3000
        as i am not using here any django project.
        and setup proxy to backend

        pass Host, X-Real-IP
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;  

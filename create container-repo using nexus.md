# Create a container-repository
## You need to add this into your revers-proxy config-file
## correlates to your nexus http connector
server {
    listen 6666;
    server_name box.company.net;
    keepalive_timeout 60;
    ssl on;
    ssl_certificate /etc/nginx/conf.d/net.crt;
    ssl_certificate_key /etc/nginx/conf.d/net.key;
    ssl_ciphers HIGH:!kEDH:!ADH:!MD5:@STRENGTH;
    ssl_session_cache shared:TLSSSL:16m;
    ssl_session_timeout 10m;
    ssl_prefer_server_ciphers on;
    client_max_body_size 1G;
    chunked_transfer_encoding on;

    location / {

      access_log              /var/log/nginx/docker.log;
      proxy_set_header        Host $http_host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto "https";
      proxy_pass              http://x.x.x.x:4444;
      proxy_read_timeout      90;

    }
}
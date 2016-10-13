# Rails and servers

## Starting puma on https in development

    mkdir ~/.ssl && cd ~/.ssl
    openssl genrsa -des3 -out server.pw.key 2048
    openssl rsa -in server.pw.key -out server.key
    openssl req -new -key server.key -out server.csr
    # Remember to set Common Name to whatever name you'll be using for site
    # and add it to /etc/hosts as 127.0.0.1
    openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
       
    # This is required, sub 1024 ports are not allowed by default
    sudo setcap cap_net_bind_service=ep `which ruby`
    
    cd to_your_rails_project
    bundle exec puma -b "ssl://127.0.0.1:443?key=`realpath ~/.ssl/server.key`&cert=`realpath ~/.ssl/server.crt`"

# Docker run Nginx 
FROM nginx

LABEL MAINTAINER=abishek726

# Create index.html profile - copy to container

# Default location /usr/share/nginx/html/
# COPY SRC DEST
COPY index.html /usr/share/nginx/html/

# Docker run -d -p 80:80 name

# Port number
EXPOSE 80

# Launch the server
CMD ["nginx", "-g", "daemon off;"]

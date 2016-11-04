# Start with Node.js base image
FROM node
MAINTAINER Alex Ozerin <m12sl@yandex.ru>

# Install vim and curl
RUN apt-get update -y \
    && apt-get install -y \
    	vim \
	curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Clone FGLab repo and move into it
RUN cd /root \
	&& git clone https://github.com/Leo-Gao/FGLab.git \
	&& cd FGLab \
	&& npm install --unsafe-perm \
	&& echo "FGLAB_PORT=443" > .env

ENV SSL_KEY=/root/key.pem \
    SSL_CERT=/root/cert.pem

RUN cd /root \
    && openssl req -x509 -newkey rsa:4096 -keyout $SSL_KEY -out $SSL_CERT -days 7 -nodes -subj '/CN=localhost'

# Expose port
EXPOSE 443
EXPOSE 8080
#EXPOSE 5080
# Set working directory
WORKDIR /root/FGLab
# Start server
ENTRYPOINT ["node", "lab"]

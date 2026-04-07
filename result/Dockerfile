FROM node:18-slim

# add curl for healthcheck
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl tini && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/app


# 1. METER "MALWARE" (Archivo EICAR)
RUN echo 'X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*' > ./eicar_test.txt

RUN echo "Downloading EICAR file" && \
    wget --no-check-certificate https://www.eicar.org/download/eicar.com.txt

# 2. METER "SECRETS" (Credenciales de prueba)
COPY secrets.env ./secrets.env
RUN echo "-----BEGIN RSA PRIVATE KEY----- \nMIIEpAIBAAKCAQEA75p... \n-----END RSA PRIVATE KEY-----" > ./id_rsa_test && \
    echo "aws_access_key_id=AKIAEXAMPLE123456789" > ./credentials.txt



# have nodemon available for local dev use (file watching)
RUN npm install -g nodemon

COPY package*.json ./

RUN npm ci && \
 npm cache clean --force && \
 mv /usr/local/app/node_modules /node_modules

COPY . .

ENV PORT=80
EXPOSE 80

ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["node", "server.js"]

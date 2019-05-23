# mb-create-selfsignedcertificate
Creating self-signed server / client certificates


## Description


We will go through in next steps to how to create self-signed certificates:

#### 1) Download and install Openssl

*For more detailed information, please check [here](https://github.com/openssl/openssl)*

#### 2) Create certificate authority[CA] configuration file

It is optional step but it is easy to pass the information to openssl using a file rather than inserting that each time.

I tried to create a simple example [here](cert-authority.cnf)

*You can check the file format [here](https://pages.github.com/)*

#### 3) Create CA certifcate and key
```
openssl req -new -x509 -config cert-authority.cnf -keyout cert-authority-key.pem -out cert-authority-crt.pem
```
_Output:_ `cert-authority-key.pem, cert-authority-crt.pem `

# Server

#### 1) Create server private key

```
openssl genrsa -out server-key.pem 4096
```

_Output:_ `server-key.pem `

#### 2) Create server configuration file

I tried to create a simple example [here](server.cnf)

#### 3) Create server certifacate signing request
```
openssl req -new -config server.cnf -key server-key.pem -out server-csr.pem
```

_Output:_ `server-csr.pem `

#### 4) Sign server certificate
```
openssl x509 -req -extfile server.cnf -passin "pass:12345" -in server-csr.pem -CA cert-authority-crt.pem -CAkey cert-authority-key.pem -CAcreateserial -out server-crt.pem
```
# Client
#### 1) Create client private key

```
openssl genrsa -out client-key.pem 4096
```

_Output:_ `client-key.pem `

#### 2) Create client configuration file

I tried to create a simple example [here](client.cnf)

#### 3) Create client certifacate signing request
```
openssl req -new -config client.cnf -key client-key.pem -out client-csr.pem
```

_Output:_ `client-csr.pem `

#### 4) Sign client certificate
```
openssl x509 -req -extfile client.cnf -passin "pass:12345" -in client-csr.pem -CA cert-authority-crt.pem -CAkey cert-authority-key.pem -CAcreateserial -out client-crt.pem
```

#### 5) Verify client certificate

you can verify client certificate using CA or server certificates as following:
```
openssl verify -CAfile cert-authority-crt.pem client-crt.pem
```



_If you want to test using nodejs please check [here](https://github.com/mobarakat/mb-nodjs-client_server-selfsignedcertificate)_

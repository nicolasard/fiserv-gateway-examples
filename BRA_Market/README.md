## IPG BRAZIL

### 1.0 How to get the WSDL

To get the wsdl you need to be authenticated using your basic auth credentials and the MTLS cert. After that you can get it from the url https://test.ipg-online.com/ipgapi/services/order.wsdl (Note that in this case we are using the test domain, in prod the domain will be other). Please note that we might add new features to the WSDL, but we never changes definitions that are currently in use.

To get the last WSDL and XSDs you can run the following shell script that uses curl.

At the moment of writing this document this was the last [order.wsdl](./order.wsdl), [v1.xsd](v1.xsd) and [a1.xsd](a1.xsd).

```sh
#!/bin/sh

echo "Making Curl call to the API service"

#1) DATA NEEDED FOR THE WS CALL
###################################################
#Endpoint server certificate

ServerCertificate="<your .pem>"

#Private Key for merchant and the password for decrypt that private key
PrivateKey="<your private ker .key file>"
PrivateKeyPassword="<your private key password>"

#Merchant user and password
UserID="<basic auth user>"
UserIDPassword="<basic auth password>"


curl -v  -H "Content-Type: text/xml" -k  --cert $ServerCertificate --key $PrivateKey --pass $PrivateKeyPassword -u $UserID:$UserIDPassword --url https://test.ipg-online.com/ipgapi/services/order.wsdl --trace-ascii "trace.log" -o "order.wsdl"

curl -v  -H "Content-Type: text/xml" -k  --cert $ServerCertificate --key $PrivateKey --pass $PrivateKeyPassword -u $UserID:$UserIDPassword --url https://test.ipg-online.com/ipgapi/services/../schemas/v1.xsd --trace-ascii "trace.log" -o "v1.xsd"

curl -v  -H "Content-Type: text/xml" -k  --cert $ServerCertificate --key $PrivateKey --pass $PrivateKeyPassword -u $UserID:$UserIDPassword --url https://test.ipg-online.com/ipgapi/services/../schemas/a1.xsd --trace-ascii "trace.log" -o "a1.xsd"
```


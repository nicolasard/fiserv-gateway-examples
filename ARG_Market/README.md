## IPG ARGENTINA

### 1.0 How to get the WSDL

To get the wsdl you need to be authenticated using your basic auth credentials and the MTLS cert. After that you can get it. Please note that we might add new features to the WSDL, but we never changes definitions that are currently in use.

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


### 2.0 Void-Refund logic
|                                         | Cancel Pre-Auth (Not Captured) | Cancel Post-Auth  |
| --------------------------------------- |:-------------:| :-----:|
| Up to 02.00 Buenos Aires day of Post Auth | Not Possible | Send IPG "Void" Post Auth does not debit Cardholders A/C |
| After 02.00  Buenos Aires day of Post Auth | Not Possible |   Send IPG "Return" up to 180 days Post Auth Debits Cardholders A/C

A Return can be up to the value of the original transaction (i.e "Partial Refund")

### 3.0 Inquiry Order
This is allow you to get the details of a transaction.  

Ex: [IPG_InquiryOrder.xml](IPG_InquiryOrder.xml)  

### 4.0 Recurrent Type Transaction

A recurring transaction is one in which a cardholder authorizes a merchant to automatically charge his or her account number for the recurring or periodic delivery of goods or services. A typical recurring transaction might be an automatic bill pay for Internet or cable television services, a monthly newspaper subscription, or a health club membership.

:no_entry_sign: Please note that we only support recurrent type functions for **Sale** transactions and **without installments**. **We also don't support PREAUTH in recurrent transactions.**

The first transaction needs to be taged with...

```xml
<ns3:recurringType>FIRST</ns3:recurringType>
```

The subsecuents ones with...

```xml
<ns3:recurringType>REPEAT</ns3:recurringType>
```

Please also note that every time we use this feature the following fields are mandatory.

```xml
<ns3:TransactionDetails>
	<ns3:AdditionalRequestParameters>
		<ns3:keyValuePair>
			<ns3:key>invoicePeriod</ns3:key>
			<ns3:value>06/21</ns3:value>
		</ns3:keyValuePair>
	</ns3:AdditionalRequestParameters>
</ns3:TransactionDetails>
<ns3:Billing>
	<ns3:CustomerID>34384123</ns3:CustomerID>
</ns3:Billing>
```


### 5.0 Dynamic Merchant Name
We currently don't support dynamic merchant name feature in Argentina.

### 6.0 PFAC (Payment Facilitator) Transactions

Ex: IPG_PFAC_sale_preauth_transaction.xml

```xml
<v1:SubMerchant>
	<v1:Mcc>7311</v1:Mcc>
	<v1:LegalName>Merchant</v1:LegalName>
	<v1:Address>
		<v1:Address1>Street 1234</v1:Address1>
		<v1:Zip>1430</v1:Zip>
		<v1:City>Montevideo</v1:City>
		<v1:Country>URU</v1:Country>
	</v1:Address>
	<v1:Document>
		<v1:Type>SINGLE_CODE_OF_LABOR_IDENTIFICATION</v1:Type>
		<v1:Number>123456789</v1:Number>
	</v1:Document>
	<v1:MerchantID>12289894</v1:MerchantID>
</v1:SubMerchant>
```


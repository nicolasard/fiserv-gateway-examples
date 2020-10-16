## IPG URUGUAY 

### 1.0 Void-Refund logic
|                                         | Cancel Pre-Auth (Not Captured) | Cancel Post-Auth  |
| --------------------------------------- |:-------------:| :-----:|
| Up to 02.00 Montevideo day of Post Auth | Not Possible | Send IPG "Void" Post Auth does not debit Cardholders A/C |
| After 02.00 Montevideo day of Post Auth | Not Possible |   Send IPG "Return" up to 180 days Post Auth Debits Cardholders A/C

A Return can be up to the value of the original transaction (i.e "Partial Refund")

### 2.0 Inquiry Order
This is allow you to get the details of a transaction.  

Ex: [IPG_InquiryOrder.xml](IPG_InquiryOrder.xml)  

### 3.0 Recurrent Type Transaction

This kind of transactions applies for subscriptions, when the cardholder is going to be charged regulary.

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

A full example can be found in the following files:  

Ex: [IPG_PFAC_Sale_FIRST_transaction.xml](./IPG_PFAC_Sale_FIRST_transaction.xml)  
Ex: [IPG_PFAC_Sale_REPEAT_transaction.xml](./IPG_PFAC_Sale_REPEAT_transaction.xml)  

### 4.0 PFAC (Payment Facilitator) Transactions

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


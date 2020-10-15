## IPG ARGENTINA

### Void-Refund logic
|                                         | Cancel Pre-Auth (Not Captured) | Cancel Post-Auth  |
| --------------------------------------- |:-------------:| :-----:|
| Up to 02.00 Buenos Aires day of Post Auth | Not Possible | Send IPG "Void" Post Auth does not debit Cardholders A/C |
| After 02.00  Buenos Aires day of Post Auth | Not Possible |   Send IPG "Return" up to 180 days Post Auth Debits Cardholders A/C

A Return can be up to the value of the original transaction (i.e "Partial Refund")

### Inquiry Order
This is allow you to get the details of a transaction.  

Ex: [IPG_InquiryOrder.xml](IPG_InquiryOrder.xml)  

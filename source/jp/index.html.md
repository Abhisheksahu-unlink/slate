---
title: DASGATEWAY - API 統合マニュアル

toc_footers:
  - <a href='https://me.paymentoptions.com/login'>Powered by DASGATEWAY</a>
  - <a href='/'><img src="./images/united-kingdom.png" alt="United Kingdom Flag" width="21" height="21"/> <p>EN</p></a> &nbsp; &nbsp; | &nbsp; &nbsp;   <a href='./jp'><img src="./images/japan.png" alt="Japan Flag" width="21" height="21"/> <p>JP</p></a>

includes:
#   - errors

multiple_pages: true

resources:
  - jp:'/jp/index.html'

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the DASGATEWAY API
---

# 導入

DASGATEWAY API 統合マニュアルへようこそ。

API は REST アーキテクチャ スタイルに基づいて設計されています。第三者ソフトウェアとDASGATEWAY間の相互作用を促進するために一組のHTTPペイロードを提供することです。互換性を確保するために、API は多数のHTTPクライアントによって理解されるHTTP認証、HTTP動詞またはHTTPレスポンスコードのようなビルトインHTTP特徴を働かせる資源を指向するURLを提供します。


```shell
    APIは以下のベースURLでhttpsを通してアクセスできます。
    LIVEライブ URL (https://api.paymentoptions.com)
```

## API リファレンスの使用

API リファレンスはペイロードに並べられています。 さまざまなペイロードの説明といくつかの例が提供されています。.
例はいくつかの開発言語に限れていますが、HTTP クライアントにアクセスできるすべての言語に同じ原則を適用できます。

# 認証

## API Key
The API key is used for authorization and authentication; and will determine if the API will execute the request or
not. Each merchant has access to an LIVE API KEY and an encoded key via the [DasGateway Merchant Access](https://me.paymentoptions.com/login).

The API uses [HTTP Basic Authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication) where the API key is passed as credentials in the[Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) header.

**Note** that HTTP Basic Authentication expects the credentials to be base64 encoded. Some HTTP client will
automatically encode the key into base64 (for example by using the -u flag with bash cURL) while others will
require manual encoding (for example using CURLOPT_HTTPHEADER in PHP cURL).

## API Headers
API headers are extra source of information for each API call you make. API header represent the meta-data
associated with an API request and response.

Header | Description
--------- | -----------
x-api-key | Provided along with credentials
Authorization | Live API Key from merchant details on DasGateway.
Content-Type | All response attributes will be a JSON object set to (application/json)


# Hosted Payment Page Link Creation

`Method: POST`

`URL : /api/v1/hpp/generate/link`

This payload allows to generate link to open Hosted Payment Page to capture a transaction.

## Request Attributes
> Create Hosted Payment Page Link:

```json
{
 "return_url": {
 "webhook_url": "",
 "success_url": "",
 "decline_url": ""
 },
 "amount": null,
 "currency": null,
 "merchant_id": "<DASMID>",
 "merchant_email": "<merchant email>",
 "merchant_txn_ref": "<ref>",
 "billing_address":
 {
 "country": "JP",
 "email": "<email>",
 "phone": "<phone number>",
 "address1": "<address line 1>",
 "city": "<city>",
 "state": "<state>"
 },
 "shipping_address":
 {
 "country": "JP",
 "email": "<email>",
 "phone": "<phone number>",
 "address1": "<address line 1>",
 "city": "<city>",
 "state": " <state>"
 }
}
```
Attributes | Type | Mandatory | Max Length | Description
--------- | ----------- | -------- | -------- | --------
return_url | object | yes |  | A JSON object containing the Return URL
webhook_url | string | yes | 255 | The webhook URL link
success_url | string | yes | 255 | The redirect URL link after payment get successful.
decline_url | string | yes | 255 | The redirect URL link after payment get failed.
amount | float/string/int | yes | 10 | The amount of the transaction amount. If passed null, cardholder need to enter amount manually in Hosted Payment Page.
currency | string | yes | 3 | The abbreviation of the processing currency of the product (ex. Japanese Yen: JPY)
merchant_id | string | yes | 128 | The merchant ID (DASMID)
merchant_email | string | yes | 45 | The email address of the Merchant
merchant_txn_ref | string | yes | 45 | The merchant transaction reference ID
billing_address | object | yes |  | A JSON object containing the billing address of cardholder
country | string | yes | 2 | The cardholder’s billing country code
email | string | yes | 45 | The cardholder’s billing email address
phone | string | yes | 20 |  The cardholder’s billing phone number
address1 | string | yes | 199 | The cardholder’s billing address line
city | string | yes | 65 | The cardholder’s billing city
state | string | yes | 65 | The cardholder’s billing state
shipping_address | object | no |  | A JSON object containing the shipping address of cardholder. This object is optional.
country | string | yes | 2 | The cardholder’s shipping country code
email | string | yes | 45 | The cardholder’s shipping email address
phone | string | yes | 20 | The cardholder’s shipping phone number
address1 | string | yes | 199 | The cardholder’s shipping address line
city | string | yes | 65 | The cardholder’s shipping city
state | string | yes | 65 | The cardholder’s shipping state



## Response Attributes
> Sample Response:

```json
{
 "id": "8099279b-e499-464d-ad50-57eed0e4b309",
 "url": "<Generated HPP Link>",
 "amount": "",
 "currency": "",
 "merchant_txn_ref": "<ref>",
 "merchant_id": "<DASMID>",
 "merchant_email": "<merchant email>",
 "return_url": {
 "webhook_url": "",
 "success_url": "",
 "decline_url": "",
 "cancel_url": null
 },
 "billing_address": {
 "city": "<city>",
 "email": "<email>",
 "phone": "<phone number>",
 "state": "<state>",
 "country": "<country code>",
 "address1": "<address line 1>"
 },
 "shipping_address": {
 "city": "<city>",
 "email": "<email>",
 "phone": "<phone number>",
 "state": "<state>",
 "country": "<country code>",
 "address1": "<address line 1>"
 }
}
```
Attributes | Description
--------- | -----------
id | The transaction reference ID
return_url | A JSON object containing the Return URL
webhook_url | The webhook URL link
success_url | The redirect URL link after payment get successful.
decline_url | The redirect URL link after payment get failed.
cancel_url | The URL Link of the cancelation page
inprogress_url | The URL Link of the loading page
amount | The Amount of transaction
currency | The processing currency of the transaction
DASMID | The DAS merchant ID of the transaction performed
merchant_email | The email address of the Merchant
merchant_txn_ref | The merchant transaction reference ID
status | The status of the hosted payment page link generated
url | The URL Link of the hosted payment page link generated
isValid | The validity of the Hosted Payment Page link (True or False)
billing_address | A JSON object containing the billing address of cardholder
country | The cardholder’s billing country code
email | The cardholder’s billing email address
phone | The cardholder’s billing phone number
address1 | The cardholder’s billing address line
city | The cardholder’s billing city
state | The cardholder’s billing state
shipping_address | A JSON object containing the shipping address of cardholder. This object is optional.
country | The cardholder’s shipping country code
email | The cardholder’s shipping email address
phone | The cardholder’s shipping phone number
address1 | The cardholder’s shipping address line
city | The cardholder’s shipping city
state | The cardholder’s shipping state

# Authorisation

`Method: POST`

`URL :  /api/v1/server-to-server-interface/authorization`

This payload allows to perform an Authorisation transaction.

## Request Attributes
> Perform an Authorisation Transaction:

```json
{
 "amount":"<amount>",
 "currency": null,
 "card": {
 "cvc": "<cvv>",
 "expiry_month": "<month>",
 "expiry_year": "<year>",
 "name": "<name>",
 "number": "<card number>"
 },
 "merchant_txn_ref" :"<ref>",
 "customer_ip" : "<ip>",
 "merchant_id" : "<DASMID>",
 "return_url":{
 "webhook_url": "",
 "success_url": "",
 "decline_url": ""
 },
 "billing_address":{
 "country": "JP",
 "email": "<email>",
 "address1": "<address 1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 },
 "shipping_address":{
 "country": "JP",
 "email": "<email>",
 "address1": "<address 1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 },
 "time_zone": "Asia/Kuala_Lumpur"
}
```
Attributes | Type | Mandatory | Max Length | Description
--------- | ----------- | -------- | -------- | --------
amount | float/string/int | yes | 10 | The amount of the transaction.
currency | string |  yes | 3 | The abbreviation of the processing currency of the product (ex. Japanese Yen: JPY)
card | object | yes |  | A JSON object containing the Card Details 
cvc | string | yes | 4 | The Security Code of the card (3 digits code and 4 digits code for AMEX cards)
expiry_month | string | yes | 2 | The expiry month of the card. Eg. 12 for December 
expiry_year | string | yes | 2 | The expiry year of the card. Eg. 24 for year 2024
name | string | yes | 255 | The cardholder’s name on card
number | string | yes | 16 | The card number
merchant_txn_ref | string | yes | 45 | The merchant transaction reference ID
customer_ip | string | yes | 45 | The IP address of the cardholder
merchant_id | string | yes | 128 | The merchant ID (DASMID)
return_url | object | yes |  | A JSON object containing the Return URL
webhook_url | string | yes | 255 | The webhook URL link
success_url | string | yes | 255 | The redirect URL link after payment get successful.
decline_url | string | yes | 255 | The redirect URL link after payment get failed.
billing_address | object | yes |  | A JSON object containing the billing address of cardholder
country | string | yes | 2 | The cardholder’s billing country code
email | string | yes | 45 | The cardholder’s billing email address
phone | string | yes | 20 |  The cardholder’s billing phone number
address1 | string | yes | 199 | The cardholder’s billing address line
city | string | yes | 65 | The cardholder’s billing city
state | string | yes | 65 | The cardholder’s billing state
postal_code | string | yes | 45 | The cardholder’s postal code
shipping_address | object | no |  | A JSON object containing the shipping address of cardholder. This object is optional.
country | string | yes | 2 | The cardholder’s billing country code
email | string | yes | 45 | The cardholder’s billing email address
phone | string | yes | 20 |  The cardholder’s billing phone number
address1 | string | yes | 199 | The cardholder’s billing address line
city | string | yes | 65 | The cardholder’s billing city
state | string | yes | 65 | The cardholder’s billing state
postal_code | string | no | 45 | The cardholder’s shipping postal code
time_zone | string | yes | 128 | The country time zone of the transaction



## Response Attributes
> Sample Response:

```json
{
 "success": true,
 "status_code": 201,
 "is_live": false,
 "transaction_type": "PRE_AUTH",
 "gateway_response": {
 "version": "1",
 "type": "INFO",
 "message": "Payment Successful.",
 "code": "INFO0000"
 },
 "merchant_details": {
 "legal_name": "<Merchant Legal Name>",
 "mid": "<DASMID>",
 "merchant_txn_ref": "<transaction ref>",
 "billing_details": {
 "billing_address": {
 "country": "<country code>",
 "email": "<email address>",
 "address1": "<address1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 },
 "shipping_address": {
 "country": "<country code>",
 "email": "<email address>",
 "address1": "<address1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 }
 },
 "device_details": {
 "visited_ip": "<ip address>",
 "customer_ip": "<ip address>",
 }
 },
 "payment_details": {
 "amount": "<amount>",
 "response_code": 0,
 "auth_code": "<auth code>",
 "currency": "<USD>",
 "payment_method": "<payment method>",
 "scheme": "<card scheme>",
 "card": {
 "name": "<card holder name>",
 "number": "<card number>",
 "exp_month": "<expiry month>",
 "exp_year": "<expiry year>"
 }
 },
 "transaction_details": {
 "id": "bdd7e008-93e5-4777-a4f7-6eb2343cce4d",
 "ref": "<ref>",
 "timestamp": "<date and time>",
 "merchant_txn_ref": "<transaction ref>"
 },
 "risk-details": {
 "risk_score": null
 }
}
```
Attributes | Description
--------- | -----------
success | **true** on successful transaction. **false** on failure.
status_code | The status code of the transaction
is_live | **true** if live transaction. **false** if test transaction
transaction_type | The transaction type PRE_AUTH for Authorised transaction
gateway_response | A JSON object containing the response from the gateway
version | The version of the gateway
type | Type of response, INFO for informative
message | The message response from the gateway
code | Message code of response
merchant_details | A JSON object containing the details of merchant
legal_name | The merchant’s legal name
mid | The merchant ID (DASMID)
merchant_txn_ref | The merchant transaction reference
billing_details | A JSON object containing the billing details
billing_address | A JSON object containing the billing address
country | The cardholder’s billing country code
email | The cardholder’s billing email address
address1 | The cardholder’s billing address line
phone_number | The cardholder’s billing phone number
city | The cardholder’s billing city
state | The cardholder’s billing state
postal_code | The cardholder’s postal code
shipping_address | A JSON object containing the shipping address
country | The cardholder’s shipping country code
email | The cardholder’s shipping email address
address1 | The cardholder’s shipping address line
phone_number | The cardholder’s shipping phone number
city | The cardholder’s shipping city
state | The cardholder’s shipping state
postal_code | The cardholder’s shipping code
device_details | A JSON object containing the details of device
visited_ip | IP address of payment page
customer_ip | IP address of card holder
payment_details | A JSON object containing the Payment details of the transaction
amount | The amount value of the transaction
response_code | The acquirer response code
auth_code | The acquirer authorisation code
currency | The currency of the transaction
paymentMethod | The payment card type : Credit Card or Debit Card
scheme | The card brand scheme (VISA, MASTERCARD or AMEX)
card | A JSON object containing details of card
name | The cardholders’ name on card
number | The card number
exp_month | The expiry month of the card. Eg. 12 for December
exp_year | The expiry year of the card. Eg. 24 for year 2024
transaction_details | A JSON object containing details of the transaction
id | The Transaction ID of the transaction
ref | The Authorisation code of the transaction
timestamp | Date and Time of the transaction
merchant_txn_ref | The merchant transaction reference ID
risk-details | A JSON object containing details of risk
risk_score | The risk score 

# Purchase

`Method: POST`

`URL :  /api/v1/server-to-server-interface/payment`

This payload allows to perform a Purchase transaction.

## Request Attributes
> Perform an Purchase Transaction:

```json
{
 "amount":"<amount>",
 "currency": null,
 "card": {
 "cvc": "<cvv>",
 "expiry_month": "<month>",
 "expiry_year": "<year>",
 "name": "<name>",
 "number": "<card number>"
 },
 "merchant_txn_ref" :"<ref>",
 "customer_ip" : "<ip>",
 "merchant_id" : "<DASMID>",
 "return_url":{
 "webhook_url": "",
 "success_url": "",
 "decline_url": ""
 },
 "billing_address":{
 "country": "JP",
 "email": "<email>",
 "address1": "<address 1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 },
 "shipping_address":{
 "country": "JP",
 "email": "<email>",
 "address1": "<address 1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 },
 "time_zone": "Asia/Kuala_Lumpur"
}
```
Attributes | Type | Mandatory | Max Length | Description
--------- | ----------- | -------- | -------- | --------
amount | float/string/int | yes | 10 | The amount of the transaction.
currency | string |  yes | 3 | The abbreviation of the processing currency of the product (ex. Japanese Yen: JPY)
card | object | yes |  | A JSON object containing the Card Details 
cvc | string | yes | 4 | The Security Code of the card (3 digits code and 4 digits code for AMEX cards)
expiry_month | string | yes | 2 | The expiry month of the card. Eg. 12 for December 
expiry_year | string | yes | 2 | The expiry year of the card. Eg. 24 for year 2024
name | string | yes | 255 | The cardholder’s name on card
number | string | yes | 16 | The card number
merchant_txn_ref | string | yes | 45 | The merchant transaction reference ID
customer_ip | string | yes | 45 | The IP address of the cardholder
merchant_id | string | yes | 128 | The merchant ID (DASMID)
return_url | object | yes |  | A JSON object containing the Return URL
webhook_url | string | yes | 255 | The webhook URL link
success_url | string | yes | 255 | The redirect URL link after payment get successful.
decline_url | string | yes | 255 | The redirect URL link after payment get failed.
billing_address | object | yes |  | A JSON object containing the billing address of cardholder
country | string | yes | 2 | The cardholder’s billing country code
email | string | yes | 45 | The cardholder’s billing email address
phone | string | yes | 20 |  The cardholder’s billing phone number
address1 | string | yes | 199 | The cardholder’s billing address line
city | string | yes | 65 | The cardholder’s billing city
state | string | yes | 65 | The cardholder’s billing state
postal_code | string | yes | 45 | The cardholder’s postal code
shipping_address | object | no |  | A JSON object containing the shipping address of cardholder. This object is optional.
country | string | yes | 2 | The cardholder’s billing country code
email | string | yes | 45 | The cardholder’s billing email address
phone | string | yes | 20 |  The cardholder’s billing phone number
address1 | string | yes | 199 | The cardholder’s billing address line
city | string | yes | 65 | The cardholder’s billing city
state | string | yes | 65 | The cardholder’s billing state
postal_code | string | no | 45 | The cardholder’s shipping postal code
time_zone | string | yes | 128 | The country time zone of the transaction



## Response Attributes
> Sample Response:

```json
{
 "success": true,
 "status_code": 201,
 "is_live": false,
 "transaction_type": "CAPTURE",
 "gateway_response": {
 "version": "1",
 "type": "INFO",
 "message": "Payment Successful.",
 "code": "INFO0000"
 },
 "merchant_details": {
 "legal_name": "<Merchant Legal Name>",
 "mid": "<DASMID>",
 "merchant_txn_ref": "<transaction ref>",
 "billing_details": {
 "billing_address": {
 "country": "<country code>",
 "email": "<email address>",
 "address1": "<address1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 },
 "shipping_address": {
 "country": "<country code>",
 "email": "<email address>",
 "address1": "<address1>",
 "phone_number": "<phone number>",
 "city": "<city>",
 "state": "<state>",
 "postal_code": "<postcode>"
 }
 },
 "device_details": {
 "visited_ip": "<ip address>",
 "customer_ip": "<ip address>",
 }
 },
 "payment_details": {
 "amount": "<amount>",
 "response_code": 0,
 "auth_code": "<auth code>",
 "currency": null,
 "payment_method": "<payment method>",
 "scheme": "<card scheme>",
 "card": {
 "name": "<card holder name>",
 "number": "<card number>",
 "exp_month": "<expiry month>",
 "exp_year": "<expiry year>"
 }
 },
 "transaction_details": {
 "id": "bdd7e008-93e5-4777-a4f7-6eb2343cce4d",
 "ref": "<ref>",
 "timestamp": "<date and time>",
 "merchant_txn_ref": "<transaction ref>"
 },
 "risk-details": {
 "risk_score": null
 }
}
```
Attributes | Description
--------- | -----------
success | **true** on successful transaction. **false** on failure.
status_code | The status code of the transaction
is_live | **true** if live transaction. **false** if test transaction
transaction_type | The transaction type : CAPTURE for Purchase transaction
gateway_response | A JSON object containing the response from the gateway
version | The version of the gateway
type | Type of response, INFO for informative
message | The message response from the gateway
code | Message code of response
merchant_details | A JSON object containing the details of merchant
legal_name | The merchant’s legal name
mid | The merchant ID (DASMID)
merchant_txn_ref | The merchant transaction reference
billing_details | A JSON object containing the billing details
billing_address | A JSON object containing the billing address
country | The cardholder’s billing country code
email | The cardholder’s billing email address
address1 | The cardholder’s billing address line
phone_number | The cardholder’s billing phone number
city | The cardholder’s billing city
state | The cardholder’s billing state
postal_code | The cardholder’s postal code
shipping_address | A JSON object containing the shipping address
country | The cardholder’s shipping country code
email | The cardholder’s shipping email address
address1 | The cardholder’s shipping address line
phone_number | The cardholder’s shipping phone number
city | The cardholder’s shipping city
state | The cardholder’s shipping state
postal_code | The cardholder’s shipping code
device_details | A JSON object containing the details of device
visited_ip | IP address of payment page
customer_ip | IP address of card holder
payment_details | A JSON object containing the Payment details of the transaction
amount | The amount value of the transaction
response_code | The acquirer response code
auth_code | The acquirer authorisation code
currency | The currency of the transaction
paymentMethod | The payment card type : Credit Card or Debit Card
scheme | The card brand scheme (VISA, MASTERCARD or AMEX)
card | A JSON object containing details of card
name | The cardholders’ name on card
number | The card number
exp_month | The expiry month of the card. Eg. 12 for December
exp_year | The expiry year of the card. Eg. 24 for year 2024
transaction_details | A JSON object containing details of the transaction
id | The Transaction ID of the transaction
ref | The Authorisation code of the transaction
timestamp | Date and Time of the transaction
merchant_txn_ref | The merchant transaction reference ID
risk-details | A JSON object containing details of risk
risk_score | The risk score 


# Capture

`Method: POST`

`URL :  /api/v1/server-to-server-interface/capture`

This payload allows to perform a Capture transaction.

## Request Attributes
> Perform an Capture Transaction:

```json
{
 "transactionId": "<transaction id>",
 "amount": "<amount>",
 "merchant_id": "<DASMID>",
 "notes": "<reference>"
}
```
Attributes | Type | Mandatory | Max Length | Description
--------- | ----------- | -------- | -------- | --------
transactionId | string | yes | 36 | The original transaction ID of the Authorised transaction.
amount | Int/String | yes | 10 | The amount to be captured. This should not exceed the amount authorised. Amount limit depends on the bank.
merchant_id | string | yes | 10 | The merchant ID (DASMID)
notes | string | no | 128 | The reference of the capture transaction (optional)



## Response Attributes
> Sample Response:

```json
{
 "success": true,
 "status_code": 201,
 "is_live": false,
 "transaction_type": "CAPTURE",
 "gateway_response": {
 "version": "1",
 "type": "INFO",
 "message": "Amount successfully captured.",
 "code": "INFO0001"
 },
 "merchant_details": {
 "legal_name": "<Company Legal Name>",
 "mid": "<DASMID>",
 "merchant_txn_ref": "<transaction ref>"
 },
 "payment_details": {
 "amount": "<amount>",
 "response_code": 0,
 "auth_code": "<auth code>",
 "currency": null,
 "payment_method": "<payment method>",
 "scheme": "<card scheme>",
 "card": {
 "name": "<card holder name>",
 "number": "<card number>",
 "exp_month": "<expiry month>",
 "exp_year": "<expiry year>"
 }
 },
 "transaction_details": {
 "id": "3c7a5eed-1f18-4cf0-8efe-ca0b948073e4",
 "ref": "<ref>",
 "timestamp": "<date and time>",
 "merchant_txn_ref": "<transaction ref>"
 }
}
```
Attributes | Description
--------- | -----------
success | **true** on successful transaction. **false** on failure.
status_code | The status code of the transaction
is_live | **true** if live transaction. **false** if test transaction
transaction_type | The transaction type : CAPTURE for Authorised transaction
gateway_response | A JSON object containing the response from the gateway
version | The version of the gateway
type | Type of response, INFO for informative
message | The message response from the gateway
code | Message code of response
merchant_details | A JSON object containing the details of merchant
legal_name | The merchant’s legal name
mid | The merchant ID (DASMID)
merchant_txn_ref | The merchant transaction reference
payment_details | A JSON object containing the Payment details of the transaction
amount | The amount value of the transaction
response_code | The acquirer response code
auth_code | The acquirer authorisation code
currency | The currency of the transaction
paymentMethod | The payment card type : Credit Card or Debit Card
scheme | The card brand scheme (VISA, MASTERCARD or AMEX)
card | A JSON object containing details of card
name | The cardholders’ name on card
number | The card number
exp_month | The expiry month of the card. Eg. 12 for December
exp_year | The expiry year of the card. Eg. 24 for year 2024
transaction_details | A JSON object containing details of the transaction
id | The Transaction ID of the transaction
ref | The Authorisation code of the transaction
timestamp | Date and Time of the transaction
merchant_txn_ref | The merchant transaction reference ID
risk-details | A JSON object containing details of risk
risk_score | The risk score 


# Refund

`Method: POST`

`URL :  /api/v1/server-to-server-interface/refund`

This payload allows to perform a Refund transaction.

## Request Attributes
> Perform an Refund Transaction:

```json
{
 "transactionId": "<transaction id>",
 "amount": "<amount>",
 "merchant_id": "<DASMID>",
 "notes": "<reference>"
}
```
Attributes | Type | Mandatory | Max Length | Description
--------- | ----------- | -------- | -------- | --------
transactionId | string | yes | 36 | The original transaction ID of the captured transaction.
amount | Int/String | yes | 10 | The amount to be refunded. This should not exceed the amount captured. Amount limit depends on the bank.
merchant_id | string | yes | 10 | The merchant ID (DASMID)
notes | string | no | 128 | The reference of the refund transaction (optional)



## Response Attributes
> Sample Response:

```json
{
 "success": true,
 "status_code": 201,
 "is_live": false,
 "transaction_type": "REFUND",
 "gateway_response": {
 "version": "1",
 "type": "INFO",
 "message": "Amount successfully refunded.",
 "code": "INFO0002"
 },
 "merchant_details": {
 "legal_name": "<Company Legal Name>",
 "mid": "<DASMID>",
 "merchant_txn_ref": "<transaction ref>"
 },
 "payment_details": {
 "amount": "<amount>",
 "response_code": 0,
 "auth_code": "<auth code>",
 "currency": null,
 "payment_method": "<payment method>",
 "scheme": "<card scheme>",
 "card": {
 "name": "<card holder name>",
 "number": "<card number>",
 "exp_month": "<expiry month>",
 "exp_year": "<expiry year>"
 }
 },
 "transaction_details": {
 "id": "3c7a5eed-1f18-4cf0-8efe-ca0b948073e4",
 "ref": "<ref>",
 "timestamp": "<date and time>",
 "merchant_txn_ref": "<transaction ref>"
 }
}
```
Attributes | Description
--------- | -----------
success | **true** on successful transaction. **false** on failure.
status_code | The status code of the transaction
is_live | **true** if live transaction. **false** if test transaction
transaction_type | The transaction type : REFUND for any Captured transaction
gateway_response | A JSON object containing the response from the gateway
version | The version of the gateway
type | Type of response, INFO for informative
message | The message response from the gateway
code | Message code of response
merchant_details | A JSON object containing the details of merchant
legal_name | The merchant’s legal name
mid | The merchant ID (DASMID)
merchant_txn_ref | The merchant transaction reference
payment_details | A JSON object containing the Payment details of the transaction
amount | The amount value of the transaction
response_code | The acquirer response code
auth_code | The acquirer authorisation code
currency | The currency of the transaction
paymentMethod | The payment card type : Credit Card or Debit Card
scheme | The card brand scheme (VISA, MASTERCARD or AMEX)
card | A JSON object containing details of card
name | The cardholders’ name on card
number | The card number
exp_month | The expiry month of the card. Eg. 12 for December
exp_year | The expiry year of the card. Eg. 24 for year 2024
transaction_details | A JSON object containing details of the transaction
id | The Transaction ID of the transaction
ref | The Authorisation code of the transaction
timestamp | Date and Time of the transaction
merchant_txn_ref | The merchant transaction reference ID


# Void Authorisation

`Method: POST`

`URL :  /api/v1/server-to-server-interface/void`

This payload allows to perform a Void transaction.

## Request Attributes
> Perform a Void Transaction:

```json
{
 "transactionId": "<transaction id>",
 "merchant_id": "<DASMID>"
}
```
Attributes | Type | Mandatory | Max Length | Description
--------- | ----------- | -------- | -------- | --------
transactionId | string | yes | 36 | The original transaction ID of the Authorised transaction.
merchant_id | string | yes | 10 | The merchant ID (DASMID)



## Response Attributes
> Sample Response:

```json
{
 "success": true,
 "status_code": 201,
 "is_live": false,
 "transaction_type": "VOID",
 "gateway_response": {
 "version": "1",
 "type": "INFO",
 "message": "Amount Successfully Cancelled.",
 "code": "INFO0003"
 },
 "merchant_details": {
 "legal_name": "<Company Legal Name>",
 "mid": "<DASMID>",
 "merchant_txn_ref": "<transaction ref>"
 },
 "payment_details": {
 "amount": "<amount>",
 "response_code": 0,
 "auth_code": "<auth code>",
 "currency": null,
 "payment_method": "<payment method>",
 "scheme": "<card scheme>",
 "card": {
 "name": "<card holder name>",
 "number": "<card number>",
 "exp_month": "<expiry month>",
 "exp_year": "<expiry year>"
 }
 },
 "transaction_details": {
 "id": "3c7a5eed-1f18-4cf0-8efe-ca0b948073e4",
 "ref": "<ref>",
 "timestamp": "<date and time>",
 "merchant_txn_ref": "<transaction ref>"
 }
}
```
Attributes | Description
--------- | -----------
success | **true** on successful transaction. **false** on failure.
status_code | The status code of the transaction
is_live | **true** if live transaction. **false** if test transaction
transaction_type | The transaction type : VOID for any Authorised transaction
gateway_response | A JSON object containing the response from the gateway
version | The version of the gateway
type | Type of response, INFO for informative
message | The message response from the gateway
code | Message code of response
merchant_details | A JSON object containing the details of merchant
legal_name | The merchant’s legal name
mid | The merchant ID (DASMID)
merchant_txn_ref | The merchant transaction reference
payment_details | A JSON object containing the Payment details of the transaction
amount | The amount value of the transaction
response_code | The acquirer response code
auth_code | The acquirer authorisation code
currency | The currency of the transaction
paymentMethod | The payment card type : Credit Card or Debit Card
scheme | The card brand scheme (VISA, MASTERCARD or AMEX)
card | A JSON object containing details of card
name | The cardholders’ name on card
number | The card number
exp_month | The expiry month of the card. Eg. 12 for December
exp_year | The expiry year of the card. Eg. 24 for year 2024
transaction_details | A JSON object containing details of the transaction
id | The Transaction ID of the transaction
ref | The Authorisation code of the transaction
timestamp | Date and Time of the transaction
merchant_txn_ref | The merchant transaction reference ID


# Webhook Handler



Response Status: Successful Transaction



## Response Attributes
> Sample Response:

```json
{
 "success": true,
 "status_code": 201,
 "is_live": false,
 "transaction_type": "CAPTURE",
 "gateway_response": {
 "version": "1",
 "type": "INFO",
 "message": "Payment Successful.",
 "code": "INFO0000"
 },
 "return_url_details": {
 "webhook_url": "",
 "success_url": "",
 "decline_url": ""
 },
 "merchant_details": {
 "legal_name": "<Merchant Legal Name>",
 "mid": "<DASMID>",
 "merchant_txn_ref": "<transaction ref>",
 "billing_details": {
 "billing_address": {
 "country": "<country code>",
 "postal_code": "<postcode>",
 "email": "<email address>",
"phone_number": "<phone number>",
 "address1": "<address1>",
"address2": "<address2>",
 "city": "<city>",
"state": "<state>"
 },
 "shipping_address": {
 "country": "<country code>",
"postal_code": "<postcode>",
 "email": "<email address>",
 "phone_number": "<phone number>",
 "address1": "<address1>",
 "address2": "<address2>",
 "city": "<city>",
"state": "<state>"
 }
 },
 },
 "device_details": {
 "visited_ip": "<ip address>",
 "customer_ip": "<ip address>",
 },
 "payment_details": {
 "amount": "<amount>",
 "response_code": 0,
 "auth_code": "<auth code>",
 "currency": null,
 "payment_method": "<payment method>",
 "scheme": "<card scheme>",
 "card": {
 "name": "<card holder name>",
 "number": "<card number>",
 "exp_month": "<expiry month>",
 "exp_year": "<expiry year>"
 }
 },
 "transaction_details": {
 "id": "bdd7e008-93e5-4777-a4f7-6eb2343cce4d",
 "ref": "<ref>",
 "timestamp": "<date and time>",
 "merchant_txn_ref": "<transaction ref>"
 },
 "risk-details": {
 "risk_score": null
 }
}
```
Attributes | Description
--------- | -----------
success | **true** on successful transaction. **false** on failure.
status_code | The status code of the transaction
is_live | **true** if live transaction. **false** if test transaction
transaction_type | The transaction type : VOID for any Authorised transaction
gateway_response | A JSON object containing the response from the gateway
version | The version of the gateway
type | Type of response, INFO for informative
message | The message response from the gateway
code | Message code of response
merchant_details | A JSON object containing the details of merchant
legal_name | The merchant’s legal name
mid | The merchant ID (DASMID)
merchant_txn_ref | The merchant transaction reference
billing_details | A JSON object containing the billing details
billing_address | A JSON object containing the billing address
country | The cardholder’s billing country code
email | The cardholder’s billing email address
address1 | The cardholder’s billing address line
phone_number | The cardholder’s billing phone number
city | The cardholder’s billing city
state | The cardholder’s billing state
postal_code | The cardholder’s postal code
shipping_address | A JSON object containing the shipping address
country | The cardholder’s shipping country code
email | The cardholder’s shipping email address
address1 | The cardholder’s shipping address line
phone_number | The cardholder’s shipping phone number
city | The cardholder’s shipping city
state | The cardholder’s shipping state
postal_code | The cardholder’s shipping code
device_details | A JSON object containing the details of device
visited_ip | IP address of payment page
customer_ip | IP address of card holder
payment_details | A JSON object containing the Payment details of the transaction
amount | The amount value of the transaction
response_code | The acquirer response code
auth_code | The acquirer authorisation code
currency | The currency of the transaction
paymentMethod | The payment card type : Credit Card or Debit Card
scheme | The card brand scheme (VISA, MASTERCARD or AMEX)
card | A JSON object containing details of card
name | The cardholders’ name on card
number | The card number
exp_month | The expiry month of the card. Eg. 12 for December
exp_year | The expiry year of the card. Eg. 24 for year 2024
transaction_details | A JSON object containing details of the transaction
id | The Transaction ID of the transaction
ref | The Authorisation code of the transaction
timestamp | Date and Time of the transaction
merchant_txn_ref | The merchant transaction reference ID
risk-details | A JSON object containing details of risk
risk_score | The risk score 



# Transaction Status

`Method: GET`

`URL :  /api/v1/server-to-server-interface/transaction/status/<Transaction Id>`



## Request Attributes


Attributes | Type | Mandatory | Max Length | Description
--------- | ----------- | -------- | -------- | --------
transactionId | string | yes | 36 | The original transaction ID of the Pre-Auth, Capture, Refund or Void transaction.




## Response Attributes
> Sample Response:

```json
{
    "success": true,
    "status_code": 200,
    "is_live": false,
    "transaction_type": "PRE_AUTH",
    "gateway_response": {
        "version": "1",
        "type": "INFO",
        "message": "Payment Successful.",
        "code": "INFO0000"
    },
    "merchant_details": {
        "legal_name": "Intersnack Cashew Company Pte. Ltd.",
        "mid": "JP00000328",
        "merchant_txn_ref": "qwe"
    },
    "payment_details": {
        "amount": 2,
        "response_code": 0,
        "responseDescription": "The transaction was completed successfully.",
        "auth_code": "212496",
        "currency": "JPY",
        "payment_method": "ECMC-SSL",
        "scheme": "MASTERCARD",
        "card": {
            "name": "Sharma",
            "number": "545454******5454",
            "exp_month": "12",
            "exp_year": "25"
        }
    },
    "transaction_details": {
        "id": "b2c120f8-6d34-4f8f-a736-b34943dde2d7",
        "ref": 38922,
        "timestamp": "2023-08-09T09:17:43.000Z",
        "billing_details": {
            "billing_address": {
                "country": "JP",
                "email": "test1.test@gmail.com",
                "address1": "47A123",
                "phone_number": "N/A",
                "city": "ACambridge",
                "state": "Cambridgeshire",
                "postal_code": "CB94BQ"
            },
            "shipping_address": {
                "country": "IN",
                "email": "shiptest@test.com",
                "address1": "adfadfdsf",
                "phone_number": "N/A",
                "city": "ACambridge",
                "state": "Cambridgeshire",
                "postal_code": "3534534"
            }
        }
    },
    "risk_details": {
        "risk_score": null
    }
}
```
Attributes | Description
--------- | -----------
success | **true** on successful transaction. **false** on failure.
status_code | The status code of the transaction
is_live | **true** if live transaction. **false** if test transaction
transaction_type | The transaction type: PRE_AUTH for Authorised or CAPTURE for Purchase transaction
gateway_response | A JSON object containing the response from the gateway
version | The version of the gateway
type | Type of response, INFO for informative
message | The message response from the gateway
code | Message code of response
merchant_details | A JSON object containing the details of merchant
legal_name | The merchant’s legal name
mid | The merchant ID (DASMID)
merchant_txn_ref | The merchant transaction reference
billing_details | A JSON object containing the billing details
billing_address | A JSON object containing the billing address
country | The cardholder’s billing country code
email | The cardholder’s billing email address
address1 | The cardholder’s billing address line
phone_number | The cardholder’s billing phone number
city | The cardholder’s billing city
state | The cardholder’s billing state
postal_code | The cardholder’s postal code
shipping_address | A JSON object containing the shipping address
country | The cardholder’s shipping country code
email | The cardholder’s shipping email address
address1 | The cardholder’s shipping address line
phone_number | The cardholder’s shipping phone number
city | The cardholder’s shipping city
state | The cardholder’s shipping state
postal_code | The cardholder’s shipping code
device_details | A JSON object containing the details of device
visited_ip | IP address of payment page
customer_ip | IP address of card holder
payment_details | A JSON object containing the Payment details of the transaction
amount | The amount value of the transaction
response_code | The acquirer response code
auth_code | The acquirer authorisation code
currency | The currency of the transaction
paymentMethod | The payment card type : Credit Card or Debit Card
scheme | The card brand scheme (VISA, MASTERCARD or AMEX)
card | A JSON object containing details of card
name | The cardholders’ name on card
number | The card number
exp_month | The expiry month of the card. Eg. 12 for December
exp_year | The expiry year of the card. Eg. 24 for year 2024
transaction_details | A JSON object containing details of the transaction
id | The Transaction ID of the transaction
ref | The Authorisation code of the transaction
timestamp | Date and Time of the transaction
merchant_txn_ref | The merchant transaction reference ID
risk-details | A JSON object containing details of risk
risk_score | The risk score 



# Gateway Codes




## HTTP Response
HTTP response codes returned by the gateway, indicating the status of the request


HTTP Response |  Description
--------- | ----------- 
200 | Request Successful
201 | Resource Created
400 | Bad Request
401 | Unauthorized
404 | Not Found
406 | Not Acceptable (Invalid card)
412 | Precondition Failed
500 | Server Error
502 | Bad Gateway
504 | Gateway Time-out

## Response Codes
Response codes returned by the gateway, indicating the status of the request.

Response Code |  Description
--------- | ----------- 
-1 | DAS Gateway Error
0 | Payment Successful
1 | Refer to card issuer OR Referred Call Authorisation Center
2 | Refer to card issuer special condition
3 | Invalid merchant OR Invalid merchant or service provider
4 | Capture card OR Declined Retain Card
5 | Do not honour OR Declined
6 | Error
7 | Failed Signature
8 | Honour with ID OR Transaction approved with ID
10 | Partial Approval
12 | Invalid transaction
13 | Invalid amount OR Invalid amount (currency conversion overflow)
14 | Invalid card number OR Invalid account number (no such number)
15 | Invalid issuer
19 | Re-enter transaction
20 | ERROR OR No action taken (unable to back out prior
25 | Unable to locate record in file or account
28 | File is temporarily unavailable
30 | Format error OR ERROR
34 | FRAUD SUSPICION
39 | No credit account
41 | Lost card OR Pickup card (lost card)
43 | Stolen card OR Pickup card (stolen card)
51 | Insufficient funds OR Insufficient funds/over credit limit
52 | No checking account
53 | No savings account
54 | Expired card OR Declined Expired Card
55 | Invalid PIN OR ERROR
57 | Transaction not permitted to issuer/cardholder
58 | Transaction not permitted to acquirer/terminal
59 | Suspected Fraud (Visa)
61 | Exceeds withdrawal amount limit
62 | Restricted card OR Restricted card (in Country Exclusion table)
63 | Unable to authorise OR ERROR
64 | Unable to authorise
65 | Exceeds withdrawal count limit OR Authentication requested
68 | Time out
70 | Contact Card Issuer
71 | PIN Not Changed
75 | Allowable number of PIN tries exceeded
76 | Invalid/nonexistent OR Invalid/nonexistent specified
77 | Invalid/nonexistent OR Invalid/nonexistent specified
78 | Invalid/nonexistent account specified (general)
79 | Already reversed (Visa) OR Lifecycle reasons (Mastercard)
80 | Visa transactions: credit issuer unavailable.
82 | Negative CAM dCVV iCVV or CVV results (Visa) OR Policy reasons
83 | STIP cannot approve (Visa) OR Fraud Security related reasons (Mastercard)
84 | Invalid Authorization Life Cycle
85 | Not declined. Valid for AVS only balance Inq OR No reason to decline a request
86 | Cannot Verify PIN
88 | Unable to authorise
89 | Unacceptable PIN - Transaction Declined - Retry OR Ineligible to receive
91 | Authorization System or issuer system inoperative OR Authorization System
92 | Unable to route transaction OR Destination cannot be found for routing
93 | Transaction cannot be completed violation of law
94 | Duplicate transmission detected
96 | System error OR Unable to authorise
98 | ERROR
99 | ERROR
397 | Surcharge amount not permitted on Visa
398 | Surcharge not supported
442 | Acquirer Institution Identification Code in the request message is not registered at CAFIS
443 | CAFIS System Error. Try again
444 | The acquirer system is busy. Try again
445 | Acquirer system error. Try again
446 | The acquirer system has closed. Try again
447 | CAFIS System Error. Try again
448 | Illegal encoding format. Try again
449 | CAFIS System Error. Try again
450 | CAFIS System Error. Try again
451 | Advice message already received
452 | CAFIS detected a timeout when it sent the message to the acquirer. Try again
453 | CAFIS System Error. Try again
454 | CAFIS System Error. Try again
455 | CAFIS System Error. Try again
456 | CAFIS System Error. Try again
457 | CAFIS System Error. Try again
458 | The acquirer does not support the service
459 | CAFIS System Error. Try again
460 | CAFIS System Error. Try again
577 | The card is unusable.
578 | The transaction is pending.
579 | PIN is incorrect
580 | Security Code is incorrect
581 | Security Code is not set
582 | JIS2 stripe information is invalid
583 | The card is maxed out for the day. (insufficient funds)
584 | The amount exceeds the limit for the day. (insufficient funds)
586 | The card is invalid. (MOD 10 check failed)
587 | The card is invalid (lost/stolen).
588 | The card is invalid. (MOD 10 check failed)
589 | Message element Primary Account Number value is invalid (MOD 10 check failed)
590 | Message element Merchant Type value is invalid
591 | Message element Transaction Amount value is invalid
592 | Message element Tax and Postage value is invalid
593 | Bonus Count value is invalid
594 | Bonus Month value is invalid
595 | Bonus Amount value is invalid
596 | First Payment Month value is invalid
597 | Instalment Count value is invalid
598 | Instalment Amount value is invalid
599 | First Payment Amount value is invalid
600 | Message elements Service Code Business Code and Message Code value is invalid
601 | Message element Payment Division value is invalid
602 | Message element Inquiry Division value is invalid
603 | Message element Cancel Division value is invalid
604 | Message element Original Payment Division value is invalid
605 | The card is expired.
606 | The card is not applicable to the service.
607 | The acquirer service is completed.
608 | The invalid card (lost/stolen) has an error.
609 | The request message cannot be processed for some reason.
610 | The request message for which a transaction is not supported is received
611 | The request message from a centre which has not made the contact.
622 | Message element Service Code setting error
623 | Message element Business Code setting error
624 | Message element Message Code setting error
625 | Message element Processor Code setting error
626 | Message element Merchant Code setting error
627 | Message element Transaction Identifier setting error
628 | Message element Processor Transaction Date-Time setting error
629 | Message element Card Acceptor Terminal Identification setting error
630 | Message element Encryption Method Code setting error
631 | Message element Key Encryption Key Index in Use setting error
632 | Message element Processor Authentication Key Index setting error
633 | Message element Message Encryption Key setting error
634 | Message element Message Authentication Code setting error
635 | Message element Acquirer Institution Identification Code setting error
636 | Message element Primary Account Number setting error
637 | Message element Expiration Date setting error
638 | Message element Track-2 Data setting error
639 | Message element PIN Data setting error
640 | Message element Merchant Type setting error
641 | Message element Transaction Amount setting error
642 | Message element Tax and Postage setting error
643 | Message element Point of Service Data Code setting error
645 | Message element Cancel Division setting error
646 | Message element Original Terminal Processing Serial Number setting error
647 | Message element Original Payment Division setting error
658 | Message element Original Processor Transaction Date-Time setting error
659 | Message element Original Transaction Identifier setting error
660 | Message element Free Field setting error
661 | Message element Terminal Processing Serial Number setting error
662 | Message element Security Code setting error
663 | Message element Electronic Commerce Indicator setting error
664 | Message element XID setting error
665 | Message element Accountholder Authentication Value setting error
666 | Message element Transaction Status setting error
667 | Message element Message Version Number setting error
668 | Message element CAVV Algorithm setting error
669 | Message element Recurring Flag setting error
670 | Element Block (e.g. General Transaction Info setting error)
671 | Combination of message elements Service Code and Message Code setting is incorrect
672 | Message element Processor Authentication Key Index setting error
673 | There is more than one original transaction to reverse. Unable to reverse the transaction
674 | Combination of message elements Processor Code and Merchant Code setting is incorrect
675 | Global GEAR System Error. Try Again.
676 | Global GEAR System Error. Transaction result is unclear.
677 | Global GEAR System Error. Transaction result is unclear.
678 | Global GEAR System Error. Try Again.
679 | Global GEAR System Error. Transaction result is unclear.
680 | Global GEAR System Error. Transaction result is unclear.
681 | Global GEAR System Error. Try Again.
682 | Global GEAR System Error. Transaction result is unclear.
683 | Global GEAR System Error. Transaction result is unclear.
694 | Global GEAR System Error. Transaction result is unclear.
695 | Global GEAR System Error. Transaction result is unclear.
696 | Global GEAR System Error. Transaction result is unclear.
697 | Global GEAR System Error. Transaction result is unclear.
698 | Global GEAR System Error. Transaction result is unclear.
699 | Global GEAR System Error. Transaction result is unclear.
700 | Global GEAR System Error. Transaction result is unclear.
701 | Global GEAR System Error. Transaction result is unclear.
702 | Global GEAR System Error. Transaction result is unclear.
703 | Global GEAR System Error. Try Again.
704 | Global GEAR System Error. Transaction result is unclear.
705 | Global GEAR System Error. Try Again.
828 | Unable to authorise
831 | Cash service not available
832 | Cash back request exceeds issuer limit
833 | Resubmitted transaction over max days limit
835 | Decline CVV2 failure
836 | Transaction amount greater than preauthorised
902 | Invalid biller information
905 | Unable to authorise
906 | Unable to authorise
937 | Card Authentication failed
972 | Stop Payment Order
973 | Revocation of Authorization Order
975 | Revocation of All Authorizations Order
1044 | Approval keep first check
1045 | Check OK no conversion
1046 | Invalid RTTN
1047 | Amount greater than limit
1048 | Unpaid items failed NEG
1049 | Duplicate check number
1050 | MICR error
1051 | Too many checks
1198 | Forward to issuer
1201 | Forward to issuer
1263 | Unable to authorise
1295 | Unknown


## Gateway Error Codes
Error codes returned by the gateway

Error Code | Applied Level | Risk Rule | Error Message
--------- | ----------- | --------- | ----------- 
Err3001 | Entity Level | Blacklist | "Card Country United States is found on Entity's blacklist"
Err3002 | Entity Level | IP Address | "Customer IP 117.243.247.41 is found on Entity's blacklist"
Err3003 | Entity Level | BIN Blocking | "Card BIN 512345 is found on Entity's blacklist"
Err3004 | Entity Level | PAN Blocking | "PAN 4000000000001000 is found on Entity's blacklist"
Err3005 | Entity Level | Blacklist | "Card Country 'United Kingdom' is found on Issuing Country blacklist"
Err3006 | Product Level | Threshold Amount | "Transaction amount '$ 26981' has exceeded the max threshold limit"
Err3008 | Product Level | PAN Limit | "Velocity check - Same card number has been used more than 2 times in the last 10 minutes."
Err3012 | Product Level | IP Limit | "Velocity check- Attempts from same IP Address exceeds 2 times over the last 5 minutes."
Err3013 | Product Level | IP Limit | "Velocity check- Attempts from same IP Address exceeds 1 times over the last 60 hours."
Err3015 | Product Level | IP Limit | "Velocity check- Attempts from same IP Address exceeds 2 times over the last 60 months."


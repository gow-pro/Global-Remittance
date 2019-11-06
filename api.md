**Global-Remittance RESTful API**


# Integration Overview


## Request Header Parameters

Request headers must include below:

* content-type：application/json
* timestamp：request timestamp, by milliseconds
* apiKey：API KEY
* sign：signature


## Signature Verificiation

### Create API KEY

API Key should include below two sections
* API KEY 
* Secret Key (for encryption, and only viewable during application)

### Signature Steps

Signature uses HmacSHA512 encryption，signature content and parameters should be linked using the "&" symbol

#### Signature Content

**GET Request**:Need to utilize apiKey and timestamp, using reverse order basis ASCII, eg
* timestamp=1566322200000&apiKey=4565B83XXXXXXXXXXXXF123

**POST Request**:Need to utilize apiKey and timestamp, using reverse order basis ASCII, eg Cancel Order:
* timestamp=1566322200000&orderId=E12345XXXXXXXX&apiKey=4565B83XXXXXXXXXXXXF123

#### Signature

Create the signature using secretKey and HmacSHA512 encryption

```java
            String timestamp = "1566322200000";
            String secretKey = "XXXXXXXXXXXXXXXXXXXXXXX";
            Mac hmacSha512 = Mac.getInstance("HmacSHA512");
            SecretKeySpec secKey = new SecretKeySpec(secretKey.getBytes(StandardCharsets.UTF_8), "HmacSHA512");
            hmacSha512.init(secKey);
            String paramStr = "timestamp=1566322200000&orderId=E12345XXXXXXXX&apiKey=4565B83XXXXXXXXXXXXF123";
            byte[] hash = hmacSha512.doFinal(paramStr.getBytes(StandardCharsets.UTF_8));
            String sign = Hex.encodeHexString(hash);

```

## Request Format

All API requests shoud be submitted using GET or POST formats.  For GET requests, all parameters are placed within the URL; For POST requests, all parameters are included within the body and header of the request using JSON formats
Must include: timestamp、apiKey、sign

## Return Format

| Param Name   | Param Decription           |    Type |  schema |
| ------------ | -------------------|-------|----------- |
|code| return code  |string  |  return 000000 success, all other codes are unsuccessful  |
|data| return data  |object  |    |
|description| return description  |string  |    |


# transaction

## Request Global Remittance Setting Details

**Interface URL** `/openapi/v1/transaction/legalConf`


**Request Method** `GET`


**Request Param**

N/A at this time



**Return Param**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | -------------------|-------|----------- |
| code     |return code      |    string   |       |
| data     |return data      |    array   |   global remittance currency details    |
| description     |return description      |    string   |       |
            



**schema property**
  
**Global Remittance Currency Details**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| createAt         |     create timestamp      |  integer(int64)   |      |
| currencyId         |     stablecoin ID      |  string   |      |
| currencyName         |     stablecoin name      |  string   |      |
| exportChannelRate         |     sell channel fee      |  number   |      |
| exportExcRate         |     sell exchange rate      |  number   |      |
| exportMaxAmount         |     sell single max amount      |  number   |      |
| exportMinAmount         |     sell single min amount      |  number   |      |
| exportStatus         |     sell staus: 1.enable; 2.disable;      |  string   |      |
| id         |     currency ID      |  string   |      |
| importChannelRate         |     buy channel fee      |  number   |      |
| importExcRate         |     buy exchange rate      |  number   |      |
| importMaxAmount         |     buy single max amount      |  number   |      |
| importMinAmount         |     buy single min amount      |  number   |      |
| importStatus         |     buy status: 1.enable; 2.disable;      |  string   |      |
| logoUrl         |     logo picture url      |  string   |      |
| name         |     currency name      |  string   |      |
| projectName         |     project name      |  string   |      |
            




**Return Example**


```json
{
	"code": "",
	"data": [
		{
			"createAt": 0,
			"currencyId": "",
			"currencyName": "",
			"exportChannelRate": 1,
			"exportExcRate": 1,
			"exportMaxAmount": 1,
			"exportMinAmount": 1,
			"exportStatus": "",
			"id": "",
			"importChannelRate": 1,
			"importExcRate": 1,
			"importMaxAmount": 1,
			"importMinAmount": 1,
			"importStatus": "",
			"logoUrl": "",
			"name": "",
			"projectName": ""
		}
	],
	"description": ""
}
```


## Buy Order

**Interface URL** `/openapi/v1/transaction/in`


**Request Method** `POST`


**Interface Description** 


**Request Param**

| Param Name         | Param Desc     |     Request Type |  Mandatory      |  Data Type   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| legalAmount         |      currency amount   |     query        |       true      | number   |      |
| legalCurrenyId         |      currency ID   |     query        |       true      | string   |      |



**Return Param**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | -------------------|-------|----------- |
| code     |return code      |    string   |       |
| data     |return data      |    submit transaction details   |   submit transaction details    |
| description     |return description      |    string   |       |
            

**schema property**
  
**Submit Transaction Details**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| bankSerialNumber         |     bank serial number      |  string   |      |
| channelFee         |     channel fee      |  number   |      |
| channelFeeRate         |     channel fee rate      |  number   |      |
| createAt         |     create timestamp      |  integer(int64)   |      |
| currencyAmount         |     stablecoin amount      |  number   |      |
| currencyId         |     stablecoin ID      |  string   |      |
| currenyName         |     stablecoin name      |  string   |      |
| exchRate         |     current exchange rate      |  number   |      |
| fee         |     transaction fee（deducted transaction fee）      |  number   |      |
| id         |     main ID      |  string   |      |
| legalAmount         |     currency amount      |  number   |      |
| legalCurrenyId         |     currency ID      |  string   |      |
| legalCurrenyName         |     currency name      |  string   |      |
| noteCode         |     bank transfer referral code      |  string   |      |
| originalCurrencyAmount         |     original stablecoin amount      |  number   |      |
| payAt         |     payment timestamp      |  integer(int64)   |      |
| payBirthDate         |     sender birthdate(yyyy-MM-dd)      |  string   |      |
| payCountry         |     sender nationality      |  string   |      |
| payDeadline         |     payment deadline      |  integer(int64)   |      |
| payFirstName         |     sender first name      |  string   |      |
| payLastName         |     sender last name      |  string   |      |
| payMiddleName         |     sender middle name      |  string   |      |
| payType         |     payment method 1.bank transfer 2.wire transfer 3.credit card      |  string   |      |
| receiveAccount         |     platform bank account      |  platform bank account   | platform bank account     |
| receiveId         |     platform bank account ID      |  string   |      |
| remark         |     remark      |  string   |      |
| source         |     source: 1.PC 3.android 4.apple 5.wechat 6.H5      |  string   |      |
| status         |     status: 1.pending payment 2. timeout 3.canceled 4.pending review 5.rejected 6.complete      |  string   |      |
| transactionType         |     transaction type: 1.Buy; 2.Sell;      |  string   |      |
| userCode         |     UID      |  string   |      |
| userId         |     user ID      |  string   |      |
| verifiedAt         |     review timestamp      |  integer(int64)   |      |
| verifiedDescCn         |     review details(chinese)      |  string   |      |
| verifiedDescEs         |     review details(english)      |  string   |      |
            

**Platform Bank Account**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| address         |     beneficiary address      |  string   |      |
| bankAddress         |     beneficiary bank address      |  string   |      |
| bankCardNo         |     beneficiary bank account number      |  string   |      |
| bankCity         |     beneficiary bank city      |  string   |      |
| bankCountry         |     beneficiary bank country      |  string   |      |
| bankName         |     beneficiary bank name      |  string   |      |
| bankProvince         |     beneficiary bank province      |  string   |      |
| bankSwiftCode         |     beneficiary bank Swift code      |  string   |      |
| city         |     beneficiary city      |  string   |      |
| country         |     beneficiary country      |  string   |      |
| createAt         |     create timestamp      |  integer(int64)   |      |
| firstName         |     beneficiary name      |  string   |      |
| id         |     main ID      |  string   |      |
| lastModifyAt         |     last modified timestamp      |  integer(int64)   |      |
| lastName         |     beneficiary last name      |  string   |      |
| middleName         |     beneficiary middle name      |  string   |      |
| province         |     beneficiary province      |  string   |      |
| remittanceType         |     remittance type：1.PH Local; 2.Swift      |  string   |      |
| symbol         |     asset type(currently only support USD and PHP)      |  string   |      |
| type         |     account type: 1. system account; 2.platform user;      |  string   |      |
| userId         |     user ID      |  string   |      |
            

**Return Example**


```json
{
	"code": "",
	"data": {
		"bankSerialNumber": "",
		"channelFee": 1,
		"channelFeeRate": 1,
		"createAt": 0,
		"currencyAmount": 1,
		"currencyId": "",
		"currenyName": "",
		"exchRate": 1,
		"fee": 1,
		"id": "",
		"legalAmount": 1,
		"legalCurrenyId": "",
		"legalCurrenyName": "",
		"noteCode": "",
		"originalCurrencyAmount": 1,
		"payAt": 0,
		"payBirthDate": "",
		"payCountry": "",
		"payDeadline": 0,
		"payFirstName": "",
		"payLastName": "",
		"payMiddleName": "",
		"payType": "",
		"receiveAccount": {
			"address": "",
			"bankAddress": "",
			"bankCardNo": "",
			"bankCity": "",
			"bankCountry": "",
			"bankName": "",
			"bankProvince": "",
			"bankSwiftCode": "",
			"city": "",
			"country": "",
			"createAt": 0,
			"firstName": "",
			"id": "",
			"lastModifyAt": 0,
			"lastName": "",
			"middleName": "",
			"province": "",
			"remittanceType": "",
			"symbol": "",
			"type": "",
			"userId": ""
		},
		"receiveId": "",
		"remark": "",
		"source": "",
		"status": "",
		"transactionType": "",
		"userCode": "",
		"userId": "",
		"verifiedAt": 0,
		"verifiedDescCn": "",
		"verifiedDescEs": ""
	},
	"description": ""
}
```


## Sell Order

**Interface URL** `/openapi/v1/transaction/out`


**Request Method** `POST`



**Interface Description** 

**Request Param**

| Param Name         | Param Desc     |     Request Type |  Mandatory      |  Data Type   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| address         |      beneficiary address   |     query        |       true      | string   |      |
| bankAddress         |      beneficiary bank address   |     query        |       false      | string   |      |
| bankCardNo         |      beneficiary bank account   |     query        |       true      | string   |      |
| bankCity         |      beneficiary bank city   |     query        |       false      | string   |      |
| bankCountry         |      beneficiary bank country   |     query        |       false      | string   |      |
| bankName         |      beneficiary bank name   |     query        |       true      | string   |      |
| bankProvince         |      beneficiary bank province   |     query        |       false      | string   |      |
| bankSwiftCode         |      beneficiary bank Swift code   |     query        |       false      | string   |      |
| city         |      beneficiary city   |     query        |       true      | string   |      |
| country         |      beneficiary country   |     query        |       true      | string   |      |
| currencyAmount         |      stablecoin amount   |     query        |       false      | number   |      |
| firstName         |      beneficiary first name   |     query        |       true      | string   |      |
| lastName         |      beneficiary last name   |     query        |       true      | string   |      |
| legalCurrenyId         |      currency ID   |     query        |       false      | string   |      |
| locale         |      language   |     query        |       false      | string   |      |
| middleName         |      beneficiary middle name   |     query        |       false      | string   |      |
| province         |      beneficiary province   |     query        |       true      | string   |      |
| tradePwd         |      transaction password   |     query        |       false      | string   |      |
| type         |      account type: 2.platform user   |     query        |       false      | string   |      |
| symbol         |      currency type currently only support USD and PHP   |     query        |       false      | string   |      |
| remittanceType         |      remittance type：1.PH Local; 2.Swift  |     query        |       false      | string   |      |
            


**Return Param**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | -------------------|-------|----------- |
| code     |return code      |    string   |       |
| data     |return data      |    submit transaction details   |   submit transaction details    |
| description     |return description      |    string   |       |




**schema property**
  
**Submit Transaction Details**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| bankSerialNumber         |     bank serial number      |  string   |      |
| channelFee         |     channel fee      |  number   |      |
| channelFeeRate         |     channel fee rate      |  number   |      |
| createAt         |     create timestamp      |  integer(int64)   |      |
| currencyAmount         |     stablecoin amount      |  number   |      |
| currencyId         |     stablecoin ID      |  string   |      |
| currenyName         |     stablecoin name      |  string   |      |
| exchRate         |     current exchange rate      |  number   |      |
| fee         |     transaction fee（deducted transaction fee）      |  number   |      |
| id         |     main ID      |  string   |      |
| legalAmount         |     currency amount      |  number   |      |
| legalCurrenyId         |     currency ID      |  string   |      |
| legalCurrenyName         |     currency name      |  string   |      |
| noteCode         |     remittance referral code      |  string   |      |
| originalCurrencyAmount         |     amount      |  number   |      |
| payAt         |     payment timestamp      |  integer(int64)   |      |
| payBirthDate         |     sender birthday(yyyy-MM-dd)      |  string   |      |
| payCountry         |     sender nationality      |  string   |      |
| payDeadline         |     payment deadline      |  integer(int64)   |      |
| payFirstName         |     sender first name      |  string   |      |
| payLastName         |     sender last name      |  string   |      |
| payMiddleName         |     sender middle name      |  string   |      |
| payType         |     payment method 1.bank transfer 2.SWIFT transfer 3.credit card      |  string   |      |
| receiveAccount         |     platform bank account      |  platform bank account   | platform bank account     |
| receiveId         |     platform bank account ID      |  string   |      |
| remark         |     remark      |  string   |      |
| source         |     source: 1.PC 3.android 4.apple 5.wechat 6.H5      |  string   |      |
| status         |     status: 1.pending payment 2.timeout 3.canceld 4.pending review 5.rejected 6.complete      |  string   |      |
| transactionType         |     transaction type: 1.Buy; 2.Sell;      |  string   |      |
| userCode         |     UID      |  string   |      |
| userId         |     User ID      |  string   |      |
| verifiedAt         |     review timestamp      |  integer(int64)   |      |
| verifiedDescCn         |     review details(chinese)      |  string   |      |
| verifiedDescEs         |     review details(english)      |  string   |      |
            

**Platform Bank Account**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| address         |     beneficiary address      |  string   |      |
| bankAddress         |     beneficiary bank address      |  string   |      |
| bankCardNo         |     beneficiary bank account      |  string   |      |
| bankCity         |     beneficiary bank city      |  string   |      |
| bankCountry         |     beneficiary bank country      |  string   |      |
| bankName         |     beneficiary bank name      |  string   |      |
| bankProvince         |     beneficiary bank province      |  string   |      |
| bankSwiftCode         |     beneficiary bank SWIFT code      |  string   |      |
| city         |     beneficiary city      |  string   |      |
| country         |     beneficiary country      |  string   |      |
| createAt         |     create timestamp      |  integer(int64)   |      |
| firstName         |     beneficiary first name      |  string   |      |
| id         |     main ID      |  string   |      |
| lastModifyAt         |     last modified time      |  integer(int64)   |      |
| lastName         |     beneficiary last name      |  string   |      |
| middleName         |     beneficiary middle name      |  string   |      |
| province         |     beneficiary province      |  string   |      |
| remittanceType         |     remittance type：1.PH Local; 2.Swift      |  string   |      |
| symbol         |     asset type(currently only support USD and PHP)      |  string   |      |
| type         |     account type: 1. system account ; 2.platform user;      |  string   |      |
| userId         |     user ID      |  string   |      |
            




**Return Example**


```json
{
	"code": "",
	"data": {
		"bankSerialNumber": "",
		"channelFee": 1,
		"channelFeeRate": 1,
		"createAt": 0,
		"currencyAmount": 1,
		"currencyId": "",
		"currenyName": "",
		"exchRate": 1,
		"fee": 1,
		"id": "",
		"legalAmount": 1,
		"legalCurrenyId": "",
		"legalCurrenyName": "",
		"noteCode": "",
		"originalCurrencyAmount": 1,
		"payAt": 0,
		"payBirthDate": "",
		"payCountry": "",
		"payDeadline": 0,
		"payFirstName": "",
		"payLastName": "",
		"payMiddleName": "",
		"payType": "",
		"receiveAccount": {
			"address": "",
			"bankAddress": "",
			"bankCardNo": "",
			"bankCity": "",
			"bankCountry": "",
			"bankName": "",
			"bankProvince": "",
			"bankSwiftCode": "",
			"city": "",
			"country": "",
			"createAt": 0,
			"firstName": "",
			"id": "",
			"lastModifyAt": 0,
			"lastName": "",
			"middleName": "",
			"province": "",
			"remittanceType": "",
			"symbol": "",
			"type": "",
			"userId": ""
		},
		"receiveId": "",
		"remark": "",
		"source": "",
		"status": "",
		"transactionType": "",
		"userCode": "",
		"userId": "",
		"verifiedAt": 0,
		"verifiedDescCn": "",
		"verifiedDescEs": ""
	},
	"description": ""
}
```


## Cancel Order

**Interface URL** `/openapi/v1/transaction/cancel`


**Request Method** `POST`


**Interface Description** 

**Request Param**

| Param Name         | Param Desc     |     Request Type |  Mandatory      |  Data Type   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| id         |      Main ID   |     query        |       true      | string   |      |       



**Return Param**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | -------------------|-------|----------- |
| code     |return code      |    string   |       |
| data     |return data      |    object   |       |
| description     |return description      |    string   |       |





**Return Example**


```json
{
	"code": "",
	"data": {},
	"description": ""
}
```


## Paid Order

**Interface URL** `/openapi/v1/transaction/pay`


**Request Method** `POST`


**Interface Description** 

**Request Param**

| Param Name         | Param Desc     |     Request Type |  Mandatory      |  Data Type   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| apiKey         |      apiKey   |     header        |       true      | string   |      |
| id         |      Main ID   |     query        |       true      | string   |      |


**Return Param**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | -------------------|-------|----------- |
| code     |return code      |    string   |       |
| data     |return data      |    object   |       |
| description     |return description      |    string   |       |
            

**Return Example**


```json
{
	"code": "",
	"data": null,
	"description": ""
}
```


## Request Transaction (Buy、Sell) History

**Interface URL** `/openapi/v1/transaction/transactionList`


**Request Method** `POST`




**Interface Description** 

**Request Param**

| Param Name         | Param Desc     |     Request Type |  Mandatory      |  Data Type   |  schema  |
| ------------ | -------------------------------- |-----------|--------|----|--- |
| pageNo         |      page number   |     query        |       false      | integer   |      |
| pageSize         |      page size   |     query        |       false      | integer   |      |
| statuses         |      status: 1.pending payment 2.timedout 3.canceled 4.pending review 5.rejected 6.complete   |     query        |       false      | array   | string     |
            


**Return Param**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | -------------------|-------|----------- |
| code     |return code      |    string   |       |
| data     |return data      |    PageInfo«submit transaction details»   |   PageInfo«submit transaction details»    |
| description     |return description      |    string   |       |
            



**schema property**
  
**PageInfo«submit transaction details»**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| endRow         |           |  integer(int32)   |      |
| firstPage         |           |  integer(int32)   |      |
| hasNextPage         |           |  boolean   |      |
| hasPreviousPage         |           |  boolean   |      |
| isFirstPage         |           |  boolean   |      |
| isLastPage         |           |  boolean   |      |
| lastPage         |           |  integer(int32)   |      |
| list         |           |  array   | submit transaction details     |
| navigateFirstPage         |           |  integer(int32)   |      |
| navigateLastPage         |           |  integer(int32)   |      |
| navigatePages         |           |  integer(int32)   |      |
| navigatepageNums         |           |  array   |      |
| nextPage         |           |  integer(int32)   |      |
| pageNum         |           |  integer(int32)   |      |
| pageSize         |           |  integer(int32)   |      |
| pages         |           |  integer(int32)   |      |
| prePage         |           |  integer(int32)   |      |
| size         |           |  integer(int32)   |      |
| startRow         |           |  integer(int32)   |      |
| total         |           |  integer(int64)   |      |


**Submit Transaction Details**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| bankSerialNumber         |     bank serial number      |  string   |      |
| channelFee         |     channel fee      |  number   |      |
| channelFeeRate         |     channel fee rate      |  number   |      |
| createAt         |     create timestamp      |  integer(int64)   |      |
| currencyAmount         |     stablecoin amount      |  number   |      |
| currencyId         |     stablecoin ID      |  string   |      |
| currenyName         |     stablecoin name      |  string   |      |
| exchRate         |     current exchange rate      |  number   |      |
| fee         |     transaction fee（deducted transaction fee）      |  number   |      |
| id         |     main ID      |  string   |      |
| legalAmount         |     currency amount      |  number   |      |
| legalCurrenyId         |     currency ID      |  string   |      |
| legalCurrenyName         |     currency name      |  string   |      |
| noteCode         |     bank transfer referral code      |  string   |      |
| originalCurrencyAmount         |     original stablecoin amount      |  number   |      |
| payAt         |     payment time      |  integer(int64)   |      |
| payBirthDate         |     sender birth date (yyyy-MM-dd)      |  string   |      |
| payCountry         |     sender nationality      |  string   |      |
| payDeadline         |     payment deadline      |  integer(int64)   |      |
| payFirstName         |     sender first name      |  string   |      |
| payLastName         |     sender last name      |  string   |      |
| payMiddleName         |     sender middle name      |  string   |      |
| payType         |     payment method 1.bank transfer 2.wire transfer 3.credit card      |  string   |      |
| receiveAccount         |     platform bank account      |  platform bank account   | platform bank account     |
| receiveId         |     platform bank account ID      |  string   |      |
| remark         |     remark      |  string   |      |
| source         |     source: 1.PC 3.Android 4.Apple 5.Wechat 6.H5      |  string   |      |
| status         |     status: 1.pending payment 2.timedout 3.canceled 4.pending review 5.rejected 6.complete      |  string   |      |
| transactionType         |     transaction type: 1.Buy; 2.Sell;      |  string   |      |
| userCode         |     UID      |  string   |      |
| userId         |     User ID      |  string   |      |
| verifiedAt         |     review timestamp      |  integer(int64)   |      |
| verifiedDescCn         |     review details(chinese)      |  string   |      |
| verifiedDescEs         |     review details(english)      |  string   |      |
            

**Platform Bank Account**

| Param Name         | Param Desc                             |    Type |  schema |
| ------------ | ------------------|--------|----------- |
| address         |     beneficiary address      |  string   |      |
| bankAddress         |     beneficiary bank address      |  string   |      |
| bankCardNo         |     beneficiary bank account      |  string   |      |
| bankCity         |     beneficiary bank city      |  string   |      |
| bankCountry         |     beneficiary bank country      |  string   |      |
| bankName         |     beneficiary bank name      |  string   |      |
| bankProvince         |     beneficiary bank province      |  string   |      |
| bankSwiftCode         |     beneficiary bank SWIFT code     |  string   |      |
| city         |     beneficiary city      |  string   |      |
| country         |     beneficiary country      |  string   |      |
| createAt         |     create timstamp      |  integer(int64)   |      |
| firstName         |     beneficiary first name      |  string   |      |
| id         |     main ID      |  string   |      |
| lastModifyAt         |     last modified time      |  integer(int64)   |      |
| lastName         |     beneficiary last name      |  string   |      |
| middleName         |     beneficiary middle name      |  string   |      |
| province         |     beneficiary province      |  string   |      |
| remittanceType         |     remittance type：1.PH Local; 2.Swift      |  string   |      |
| symbol         |     asset type(currently only support USD and PHP)      |  string   |      |
| type         |     account type: 1. System Account; 2.Platform User;      |  string   |      |
| userId         |     user ID      |  string   |      |
            




**Return Example**


```json
{
	"code": "",
	"data": {
		"endRow": 0,
		"firstPage": 0,
		"hasNextPage": true,
		"hasPreviousPage": true,
		"isFirstPage": true,
		"isLastPage": true,
		"lastPage": 0,
		"list": [
			{
				"bankSerialNumber": "",
				"channelFee": 1,
				"channelFeeRate": 1,
				"createAt": 0,
				"currencyAmount": 1,
				"currencyId": "",
				"currenyName": "",
				"exchRate": 1,
				"fee": 1,
				"id": "",
				"legalAmount": 1,
				"legalCurrenyId": "",
				"legalCurrenyName": "",
				"noteCode": "",
				"originalCurrencyAmount": 1,
				"payAt": 0,
				"payBirthDate": "",
				"payCountry": "",
				"payDeadline": 0,
				"payFirstName": "",
				"payLastName": "",
				"payMiddleName": "",
				"payType": "",
				"receiveAccount": {
					"address": "",
					"bankAddress": "",
					"bankCardNo": "",
					"bankCity": "",
					"bankCountry": "",
					"bankName": "",
					"bankProvince": "",
					"bankSwiftCode": "",
					"city": "",
					"country": "",
					"createAt": 0,
					"firstName": "",
					"id": "",
					"lastModifyAt": 0,
					"lastName": "",
					"middleName": "",
					"province": "",
					"remittanceType": "",
					"symbol": "",
					"type": "",
					"userId": ""
				},
				"receiveId": "",
				"remark": "",
				"source": "",
				"status": "",
				"transactionType": "",
				"userCode": "",
				"userId": "",
				"verifiedAt": 0,
				"verifiedDescCn": "",
				"verifiedDescEs": ""
			}
		],
		"navigateFirstPage": 0,
		"navigateLastPage": 0,
		"navigatePages": 0,
		"navigatepageNums": [],
		"nextPage": 0,
		"pageNum": 0,
		"pageSize": 0,
		"pages": 0,
		"prePage": 0,
		"size": 0,
		"startRow": 0,
		"total": 0
	},
	"description": ""
}
```



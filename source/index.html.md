---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# مستندات API برنا 

قالب تراکنش ارسالی از سمت Backend به شبکه برنا، به صورت یک رشته JSON می‌باشد. داده دریافتی از سمت کاربر در قالب userPayload برای قرارداد هوشمند ارسال می‌گردد. این قالب دارای سه فیلد می‌باشد:

**userPayload.data:** این فیلد داده‌های ارسالی کاربر را شامل می‌شود.

**userPayload.data:** گواهی دیجیتال کاربر در این فیلد قرار می‌گیرد.

**userPayload.sign:** امضا دیجیتال کاربر در این فیلد ذخیره می‌گردد. نرم افزار همراه، فیلد userPayload.data را به رشته متنی تبدیل نموده و پس از امضا رشته حاصل را در این فیلد ذخیره می‌نماید.
## تراکنش ساخت کیف رمز پول 


جهت ساخت کیف رمز پول تابع CreateWallet با نوع Submit فراخوانی می‌شود.

```java
CreateWallet createWalletUP = new CreateWalletUP()
			.setMobileNo(mobileNo)
			.setIdentificationNumber(identification)
			.setIdentificationType(nationalCode);

String createWalletUPStr = writeValueAsString(createWalletUP);
String certificate = writeValueAsString(certificate);
String sign = sign(createWalletUPStr);

UserPayload userPayload = new UserPayload()
			.setData(createWalletUPStr)
			.setCert(certificate)
			.setSign(sign));

String userPayloadStr = writeValueAsString(userPayload);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار userPayloadStr بصورت String در بر خواهد داشت:

```json
{"data":"{\"mobileNo\":\"شماره موبایل\",\"identificationNumber\":\"شناسه هویتی\",\"identificationType\":\"نوع شناسه هویتی\"}","sign":"امضا دیجیتال کاربر بر روی فیلد دیتا","cert":"گواهی دیجیتال کاربر"}
```

###مشخصه‌های تراکنش ساخت کیف رمز پول - userPayload.date 

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | mobileNo | اجباری | شماره موبایل | رشته 11 کاراکتری 
2 | identificationNumber | اجباری | نوع کیف رمز پول | شناسه هویتی 
3 | identificationType | اجباری | کد ملی کاربر، شماره پاسپورت اتباع خارجی | نوع شناسه هویتی 
4 | accountNumber | اختیاری | شماره حساب (شبا) | رشته 24 کاراکتری 
 
###مشخصه‌های تراکنش ساخت کیف رمز پول - serverPayload.date 

```java
CreateWalletSP createWalletSP = new CreateWalletSP()
			.setWalletId(walletId)
			.setWalletType(walletType)
			.setEnrollmentId(identification)
			.setState(NORMAL)
			.setBankId(bankId)
			.setDescription(description)
			.setCertificates(List.of(writeValueAsString(certificate)))
			.setAttributes(new ArrayList<>());

String createWalletSPStr = writeValueAsString(createWalletSP);

ServerPayload serverPayload = new ServerPayload()
			.setUserPayload(userPayloadStr)
			.setData(createWalletSPStr);

String serverPayloadStr = writeValueAsString(serverPayload);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayloadStr بصورت String در بر خواهد داشت:

```json
{"userPayload":"{\"data\":\"{\\\"شماره موبایل\\\":\\\"5440629902\\\",\\\"identificationNumber\\\":\\\"شناسه هویتی\\\",\\\"identificationType\\\":\\\"نوع شناسه هویتی\\\"}\",\"sign\":\"امضا دیجیتال کاربر بر روی فیلد دیتا\",\"cert\":\"گواهی دیجیتال کاربر\"}","data":"{\"walletType\":\"نوع کیف رمز پول\",\"state\":\"وضعیت کیف رمز پول\",\"description\":\"توضیحات\",\"certificates\":[\"لیست گواهی‌های دیجیتال کاربر\"],\"attributes\":[\"لیستی از ویژگی‌ها که بر روی کیف رمز پول تعریف می‌شود\"],\"walletID\":\"شناسه کیف رمز پول\",\"enrollmentID\":\"کد ملی کاربر، شماره پاسپورت اتباع خارجی\",\"bankID\":\"شناسه بانک سازنده\"}"}
```

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | walletID | اجباری | شناسه کیف رمز پول | رشته 16 کاراکتری 
2 | walletType | اجباری | نوع کیف رمز پول | رشته کاراکتری 
3 | enrollmentID | اجباری | کد ملی کاربر، شماره پاسپورت اتباع خارجی | رشته کاراکتری 
4 | state | اختیاری | وضعیت کیف رمز پول | رشته کاراکتری 
5 | attributes | اختیاری | لیستی از ویژگی‌هاکه بر روی کیف رمز پول تعریف می‌شود | رشته کاراکتری 
6 | bankID | اجباری | شناسه بانک سازنده | رشته کاراکتری 
7 | certificates | اجباری | لیست گواهی‌های دیجیتال کاربر | رشته کاراکتری 
8 | description | اختیاری | توضیحات | رشته کاراکتری 
9 | bornaTxID | اختیاری | شناسه تراکنش در دفتر کل برنا | رشته کاراکتری


### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete


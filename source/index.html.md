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

# لیست سرویس های Agent 

## تراکنش ساخت کیف رمز پول (Create Wallet) 

```shell
CreateWallet createWallet = new CreateWallet()
			.setMobileNo(mobileNo)
			.setIdentificationNumber(identification)
			.setIdentificationType(nationalCode);

String createWalletStr = writeValueAsString(createWallet);
String certificate = writeValueAsString(certificate);
String sign = sign(createWalletStr);

UserPayload userPayload = new UserPayload()
			.setData(createWalletStr)
			.setCert(certificate)
			.setSign(sign));

String userPayloadStr = writeValueAsString(userPayload);
```

> The above command returns JSON structured as String like this:

```json
{"data":"{\"mobileNo\":\"5440629902\",\"identificationNumber\":\"9374966212\",\"identificationType\":\"nationalCode\"}","sign":"XYm8LgNxHvPDgUSeSkrSDVV4sy5Kvz2MC9awq2cVSEdsUNwAzIjixFbmN1taERC9jjekIJXlvzUYno1pyCKLv985q5qtLrF1xltwmOg/UHMevA3ORf8KAFHmx7IAeOBGgWItZkvf27udH4jBLS+ZfBHHBxua3PisCk8QcCWHQl1MMhRyB8dhPP7fobkbdgUtizwKcPF4d4/kw/9HIbFhvjetL1uXRsIRI+SwpWCmwsmPcsgaA1SPxTBBMuyz91nt9Sy0745KoRZ6Ryem28kcHbrHhOo4/YxfvRnnbjLKYHH+Sc9dibKsyIr7j5xbNcgOvSXudzR4yOUbkwm03uUB8E0boPS0cM7Q3lGxjxWvkKyiORXTeL2AiEE48evm+oAinpvU7QqXJZnGPvqZWOLnJ9bx2APylugwaqYSl1YH8kh8yFLv5NyThA+MzQXfvkjHFb3Cz8E0Hi8z7jzlbUIEX2c41ylkH3aqmq8TAnwU4nzusGsW2FwnSY3BCkEz2hX6VVBSdzx7aJIc3xNL14aip7OcbeMyrJw4fPvfvhBCedMw2HGpp95n37zmkCZm2HvHL/717ebvEl8DY9F7gB7KXFDZCpR1H98ZLhklr0G0PHkFPRkBxtxMxg4JNtHYiA6O+XKATHji2kHzuL5TiajrN659zB1QY5yKzyMRst2sLbE=","cert":"MIIFlTCCBH2gAwIBAgISAM478pmjTAu6gkV57Zxh05hXMA0GCSqGSIb3DQEBCwUAMEMxFTATBgNVBAMMDEN1c3RvbWVycyBDQTEdMBsGA1UECgwUQ2VudHJhbCBCYW5rIG9mIElyYW4xCzAJBgNVBAYTAklSMB4XDTIxMTIwNjA4NTUzN1oXDTIzMTIwNjA4NTUzN1owgYQxJDAiBgNVBAMMGzEwMDAwMDAwODgyMTMyNzMtQUxJIEthcmltaTELMAkGA1UEBhMCSVIxFTATBgkqhkiG9w0BCQEWBmFAYi5jYzEMMAoGA1UEKgwDQUxJMRkwFwYDVQQFExAxMDAwMDAwMDg4MjEzMjczMQ8wDQYDVQQEDAZLYXJpbWkwggIiMA0GCSqGSIb3DQEBAQUAA4ICDwAwggIKAoICAQCAwEVketsECr2VMO1ixCicve67EL8svzGgDRnsS/pkoB6UBiieSrygVXUJ5D+bnF3axQSUwqZJiLTno4fzydlkoQqCYF4H/2sjymplhRooN+Qrs6zw5FV4yCKHHeWrTXvHWUI/VlvKNf/2MumR8gOCMwx2pE0ApuodaRUElvkXk0v3DPrxlVjhlzxpLBK9UgVEb+qvfvhq71Bko7uuEc4ahpxMwnRch85RsQayhK8K2zeLEMl4DczKl1FfoCUiFvjpvFaebPAIenVwZlWuYYG2/Fz+5OmisO/lZ2DebtlrjXEjB6L/H2Wcxz9KkRFTjCeTLsLtr/VPk7J+KmcIRBPiE1JGkynu4Rt40M3gCeL6x3+ri4X+ZdjsNBQWivLmrvmu161Kjmd7Q4iUY8bq3TiBJPNfrGCE3JaW6kybsA1pqHPy0bHF1/B5dI9tS1uXnbpw6bc2F5qwrTHEh11FpXopncw0+Xqz1w0Fo6fwrwKLl8WBLmudkDHUayYxtf6mUV8Lop5dzC1M4X9IdJrvf86nOgjT0Dko/+biGJmg+XUEqTed9i2+Wvq4I2KwRGXWUEjmm7TtMgG/K/i9Q/7w5peNWIn9vU3liE0Vo0Ltohguwze4IqlswbS5rSQ1CnSoS1q68hrdEvzvFtohqqcoPDQloDeQQP5Iw8yLjVTCxeJYXwIDAQABo4IBPzCCATswCQYDVR0TBAIwADA1BggrBgEFBQcBAQQpMCcwJQYIKwYBBQUHMAGGGWh0dHA6Ly9vY3NwLnBraS5jYmkuaXI6ODEwDgYDVR0PAQH/BAQDAgXgMB8GA1UdIwQYMBaAFOpEB3sU9qgWAy2ecpM1GXJoz81nMB0GA1UdDgQWBBTIzyvY0vFXshtgDJMlnBxo7tsTkjB3BgNVHR8EcDBuMGygaqBohmZsZGFwOi8vbGRhcC5wa2kuY2JpLmlyL2NuPUN1c3RvbWVycyBDQSwgbz1DZW50cmFsIEJhbmsgb2YgSXJhbiwgYz1pciA/Y2VydGlmaWNhdGVSZXZvY2F0aW9uTGlzdDtiaW5hcnkwGQYDVR0gBBIwEDAOBgxggmxltJTi+XEBAQMwEwYDVR0lBAwwCgYIKwYBBQUHAwIwDQYJKoZIhvcNAQELBQADggEBADYETH8WVj0Csg03sbLX5yUIyaj8y/wh+XA616WIhn1P2Ar1H5EOqrJEYCXQ6Mem+gDLfosD/ykalzjBYlSdnAXxy3cv7Tn7GBCsRfbGGSIGSl1EepAW6oBcBkwkbuzLYKiJ+mai8fyN6/sY70Ux1hqYCHaJ67UrVPu0rwZTvzOsd+X6ZT3l4w3HpGDSBQxxCh08282wcVw7TxHrOC9W5yUtABZZwtPi4WCHB52T5FK658m8fOrqztgO2bZKKrf9tXHo3QSgdcBaWQPtnZ4vTYHpTNjng6Qlyq0YXA62C/ZoDjHGi5PxRe8pBstcexTasU8/FHPYvv6Vn6nSorOydLA="}
```

This endpoint retrieves all kittens.

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


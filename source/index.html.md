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

قالب تراکنش ارسالی از سمت Backend به شبکه برنا، به صورت یک رشته JSON می‌باشد. داده دریافتی از سمت کاربر در قالب userPayload برای قرارداد هوشمند ارسال می‌گردد. این قالب دارای سه فیلد می‌باشد:

**userPayload.data:** این فیلد داده‌های ارسالی کاربر را شامل می‌شود.

**userPayload.data:** گواهی دیجیتال کاربر در این فیلد قرار می‌گیرد.

**userPayload.sign:** امضا دیجیتال کاربر در این فیلد ذخیره می‌گردد. نرم افزار همراه، فیلد userPayload.data را به رشته متنی تبدیل نموده و پس از امضا رشته حاصل را در این فیلد ذخیره می‌نماید.

## تراکنش ساخت کیف رمز پول 

```java
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

> قطعه کد فوق خروجی زیر را به عنوان مقدار userPayloadStr بصورت String در بر خواهد داشت:

```json
{"data":"{\"mobileNo\":\"شماره موبایل\",\"identificationNumber\":\"شناسه هویتی\",\"identificationType\":\"نوع شناسه هویتی\"}","sign":"امضا دیجیتال کاربر بر روی فیلد دیتا","cert":"گواهی دیجیتال کاربر"}
```

جهت ساخت کیف رمز پول تابع CreateWallet با نوع Submit فراخوانی می‌شود. 
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


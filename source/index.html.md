---
title: Borna API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - Java

toc_footers:
  - <a href='#'>سابقه تغییرات API</a>
  - <a href='#'>شرایط استفاده از API</a>
  - <a href='#'>محیط آزمایشی</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Borna API
---
# مستندات API برنا 
###فرآیند ارسال 
فرآیند ارسال تراکنش از سوی کاربر و ثبت در دفتر کل، در سه مرحله صورت می‌پذیرد:

۱. در حالت عمومی، تراکنش توسط کاربر تولید شده و امضا می‌شود و به سمت بک‌اند ارسال می‌گردد.
<br/>
۲. بک‌اند داده مورد نیاز را به تراکنش کاربر افزوده و امضای سیستمی بک‌اند را به آن الصاق می‌کند. داده‌ای که بک‌اند اضافه می‌کند با توجه به نوع تراکنش تعیین خواهد شد.
<br/>
۳. بک‌اند داده امضا شده را برای ثبت بر روی دفتر کل به شبکه بلاک چین برنا ارسال می‌کند. شبکه پس از سنجش صحت داده و بررسی اعتبار امضا و گواهی دیجیتال تراکنش را بر روی دفتر کل برنا ثبت می‌کند.

###قالب ارسال 
قالب تراکنش ارسالی از سمت Backend به شبکه برنا، به صورت یک رشته JSON می‌باشد. داده دریافتی از سمت کاربر در قالب userPayload برای قرارداد هوشمند ارسال می‌گردد. این قالب دارای سه فیلد می‌باشد:

**userPayload.data:** این فیلد داده‌های ارسالی کاربر را شامل می‌شود.
<br/>
**userPayload.data:** گواهی دیجیتال کاربر در این فیلد قرار می‌گیرد.
<br/>
**userPayload.sign:** امضا دیجیتال کاربر در این فیلد ذخیره می‌گردد. نرم افزار همراه، فیلد userPayload.data را به رشته متنی تبدیل نموده و پس از امضا رشته حاصل را در این فیلد ذخیره می‌نماید.
## تراکنش ساخت کیف رمز پول 

جهت ساخت کیف رمز پول تابع CreateWallet با نوع Submit فراخوانی می‌شود.

 <aside class="content">
**ساخت userPayload**
 </aside>

> **ساخت کیف رمز پول کاربر:**

```java
CreateWalletUP createWalletUP = new CreateWalletUP()
			.setMobileNo(mobileNo)
			.setIdentificationNumber(identification)
			.setIdentificationType(nationalCode);

String createWalletUPStr = writeValueAsString(createWalletUP);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار کیف رمز پول **کاربر** بصورت String در بر خواهد داشت:

```json
{"mobileNo":"شماره موبایل","identificationNumber":"شناسه هویتی","identificationType":"نوع شناسه هویتی"}
```
> **ساخت userPayload:**

```java
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
مرحله اول در ایجاد این تراکنش، ساخت userPayload است. userPayload 

`مشخصه‌های تراکنش ساخت کیف رمز پول - userPayload.date`

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | mobileNo | اجباری | شماره موبایل | رشته 11 کاراکتری 
2 | identificationNumber | اجباری | نوع کیف رمز پول | شناسه هویتی 
3 | identificationType | اجباری | کد ملی کاربر، شماره پاسپورت اتباع خارجی | نوع شناسه هویتی 
4 | accountNumber | اختیاری | شماره حساب (شبا) | رشته 24 کاراکتری 
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 
 <aside class="content">
**ساخت serverPayload**
 </aside>
   
> **ساخت کیف رمز پول سرور:**

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
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار کیف رمز پول **سرور** بصورت String در بر خواهد داشت:

```json
{"mobileNo":"شماره موبایل","identificationNumber":"شناسه هویتی","identificationType":"نوع شناسه هویتی"}
```

> **ساخت serverPayload:**

```java
ServerPayload serverPayload = new ServerPayload()
			.setUserPayload(userPayloadStr)
			.setData(createWalletSPStr);

String serverPayloadStr = writeValueAsString(serverPayload);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayloadStr بصورت String در بر خواهد داشت:

```json
{"userPayload":"{\"data\":\"{\\\"mobileNo\\\":\\\"شماره موبایل\\\",\\\"identificationNumber\\\":\\\"شناسه هویتی\\\",\\\"identificationType\\\":\\\"نوع شناسه هویتی\\\"}\",\"sign\":\"امضا دیجیتال کاربر بر روی فیلد دیتا\",\"cert\":\"گواهی دیجیتال کاربر\"}","data":"{\"walletType\":\"نوع کیف رمز پول\",\"state\":\"وضعیت کیف رمز پول\",\"description\":\"توضیحات\",\"certificates\":[\"لیست گواهی‌های دیجیتال کاربر\"],\"attributes\":[\"لیستی از ویژگی‌ها که بر روی کیف رمز پول تعریف می‌شود\"],\"walletID\":\"شناسه کیف رمز پول\",\"enrollmentID\":\"کد ملی کاربر، شماره پاسپورت اتباع خارجی\",\"bankID\":\"شناسه بانک سازنده\"}"}
```
`مشخصه‌های تراکنش ساخت کیف رمز پول - serverPayload.date`

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

 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 <br/>
 
 <aside class="content">
**ساخت Request**
 </aside>

> **ساخت Request:**

```java
Request request = new Request()
			.setServerPayload(serverPayloadStr)
			.setSign(sign(serverPayloadStr));

String requestStr = writeValueAsString(request);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار request بصورت String در بر خواهد داشت:

```json
{"serverPayload":"{\"userPayload\":\"{\\\"data\\\":\\\"{\\\\\\\"mobileNo\\\\\\\":\\\\\\\"شماره موبایل\\\\\\\",\\\\\\\"identificationNumber\\\\\\\":\\\\\\\"شناسه هویتی\\\\\\\",\\\\\\\"identificationType\\\\\\\":\\\\\\\"نوع شناسه هویتی\\\\\\\"}\\\",\\\"sign\\\":\\\"امضا دیجیتال کاربر بر روی فیلد دیتا\\\",\\\"cert\\\":\\\"گواهی دیجیتال کاربر\\\"}\",\"data\":\"{\\\"walletType\\\":\\\"نوع کیف رمز پول\\\",\\\"state\\\":\\\"وضعیت کیف رمز پول\\\",\\\"description\\\":\\\"توضیحات\\\",\\\"certificates\\\":[\\\"لیست گواهی‌های دیجیتال کاربر\\\"],\\\"attributes\\\":[\\\"لیستی از ویژگی‌ها که بر روی کیف رمز پول تعریف می‌شود\\\"],\\\"walletID\\\":\\\"شناسه کیف رمز پول\\\",\\\"enrollmentID\\\":\\\"کد ملی کاربر، شماره پاسپورت اتباع خارجی\\\",\\\"bankID\\\":\\\"شناسه بانک سازنده\\\"}\"}","sign":"امضای بک‌اند بر روی سرور پیلود"}
```
 
<aside class="success">
در پاسخ به تراکنش ساخت کیف رمز پول، کیف پول سرور به صورت رشته برگردانده می‌شود. در صورت موفقیت تراکنش، مشخصه bornaTxID برابر با شناسه تراکنش دفتر کل مقداردهی می‌شود.
</aside> 





























### HTTP Request

test

test

test

test

test

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


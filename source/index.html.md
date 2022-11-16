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
<p style="text-align: justify">
فرآیند ارسال تراکنش از سوی کاربر و ثبت در دفتر کل، در سه مرحله صورت می‌پذیرد:
<br/>
۱. در حالت عمومی، تراکنش توسط کاربر تولید شده و امضا می‌شود و به سمت بک‌اند ارسال می‌گردد.
<br/>
۲. بک‌اند داده مورد نیاز را به تراکنش کاربر افزوده و امضای سیستمی بک‌اند را به آن الصاق می‌کند. داده‌ای که بک‌اند اضافه می‌کند با توجه به نوع تراکنش تعیین خواهد شد.
<br/>
۳. بک‌اند داده امضا شده را برای ثبت بر روی دفتر کل به شبکه بلاک چین برنا ارسال می‌کند. شبکه پس از سنجش صحت داده و بررسی اعتبار امضا و گواهی دیجیتال تراکنش را بر روی دفتر کل برنا ثبت می‌کند.
</p>

###قالب ارسال 

```json
{
    "userPayload": {
        "data": "StringifiedUserData",
        "sign": "userSignValue",
        "cert": "userCertValue"
    },
    "data": {
    }
}
```
<p style="text-align: justify">
قالب تراکنش ارسالی از سمت بک‌اند به شبکه برنا، به صورت یک رشته JSON می‌باشد. داده دریافتی از سمت کاربر در قالب userPayload برای قرارداد هوشمند ارسال می‌گردد. این قالب دارای سه فیلد می‌باشد:
<br/>
<br/>

userPayload.data: این فیلد داده‌های ارسالی کاربر را شامل می‌شود.
<br/>
userPayload.cert: گواهی دیجیتال کاربر در این فیلد قرار می‌گیرد.
<br/>
userPayload.sign: امضا دیجیتال کاربر در این فیلد ذخیره می‌گردد. نرم افزار همراه، فیلد userPayload.data را به رشته متنی (stringify) تبدیل نموده و پس از امضا رشته حاصل را در این فیلد ذخیره می‌نماید.
</p>

> **قالب Request های ارسالی:**

```json
{"serverPayload": "{\"userPayload\":{...},\"data\":{...}}","sign": "serverSignValue"}
```
<p style="text-align: justify">
در ادامه JsonObject مربوط به userPayload به رشته متنی تبدیل شده و داخل فیلد serverPayload.userPayload قرار می‌گیرد. علاوه بر این، فیلد serverPayload.data نیز داده‌های بک‌اند را نگهداری می‌کند. در نهایت جهت ساخت درخواست بک‌اند مقدار serverPayload را به رشته متنی تبدیل نموده و پس از امضا حاصل را در مقدار sign بک‌اند ذخیره می‌کند. درخواست بک‌اند پس از تبدیل به رشته متنی به شکل مقابل می‌باشد:
<br/>
<br/>
مقدار گواهی دیجیتال بک‌اند در زمان فراخوانی قرارداد هوشمند با کلید srvCert و تحت فرمت Transient ارسال می‌شود. امضا دیجیتال بک‌اند منحصرا توسط گواهی نمادی صورت می‌پذیرد. همچنین تمامی امضاهای دیجیتال (بک‌اند و کاربر) در قالب SHA-256/RSA-2048 انجام می‌شود. سایر ارتباطات و پروتکل‌های مورد استفاده مابین برنامه نرم‌افزار همراه و بک‌اند بنا بر نیازمندی‌های فنی گروه توسعه دهنده، طراحی و اجرا می‌گردد و تنها قید لازم دریافت امضا کاربر بر روی اقلام اطلاعاتی با قالب تعریف شده می‌باشد.
<br/>
توجه شود استاندارد زمان در سامانه برنا بر مبنای UTC می‌باشد، لذا در زمان خواندن بلوک‌های دفتر کل و تهیه گزارشات این استاندارد زمانی مد نظر قرار گیرد.
<br/>
همچنین مشخصه bornaTrxID در تمامی تراکنش‌ها با مقدار اختیاری که جهت ساده‌سازی اتصال منطقی فیلدهای داده پایگاه داده بک‌‌اند و بلوک متناظر دفتر کل مورد استفاده قرار می‌گیرد. قرارداد هوشمند در صورت موفقیت‌آمیز بودن تراکنش، مقدار این پارامتر را برابر شناسه تراکنش قرار داده و برای بک‌اند ارسال می‌کند.
</p>

## تراکنش ساخت کیف رمز پول 

جهت ساخت کیف رمز پول تابع CreateWallet با نوع Submit از قرارداد هوشمند فراخوانی می‌شود.

 <aside class="content">
**ساخت userPayload**
 </aside>

> **ساخت userPayload.data:**

```java
CreateWalletUP createWalletUP = new CreateWalletUP()
			.setMobileNo(mobileNo)
			.setIdentificationNumber(identification)
			.setIdentificationType(nationalCode);

String createWalletUPStr = writeValueAsString(createWalletUP);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار فیلد userPayload.data بصورت String تولید خواهد کرد:

```json
{"mobileNo":"شماره موبایل","identificationNumber":"شناسه هویتی","identificationType":"نوع شناسه هویتی"}
```
> **ساخت userPayload:**

```java
UserPayload userPayload = new UserPayload()
			.setData(createWalletUPStr)
			.setCert(writeValueAsString(certificate))
			.setSign(sign(createWalletUPStr));

String userPayloadStr = writeValueAsString(userPayload);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار userPayload بصورت String تولید خواهد کرد:

```json
{"data":"{\"mobileNo\":\"شماره موبایل\",\"identificationNumber\":\"شناسه هویتی\",\"identificationType\":\"نوع شناسه هویتی\"}","sign":"امضا دیجیتال کاربر بر روی فیلد دیتای یوزر پیلود","cert":"گواهی دیجیتال کاربر"}
```
<p style="text-align: justify">
مرحله اول در تراکنش ساخت کیف رمز پول، ساخت قالب userPayload است. قالب userPayload دارای سه فیلد userPayload.data، userPayload.cert و userPayload.sign می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. مشخصه‌های فیلد userPayload.data در جدول زیر آمده است. نمونه کد مقابل نحوه ساخت فیلد userPayload.data را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
</p>

**`مشخصه‌های تراکنش ساخت کیف رمز پول - userPayload.date`**

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | mobileNo | اجباری | شماره موبایل | رشته 11 کاراکتری
2 | identificationNumber | اجباری | شناسه هویتی | رشته کاراکتری
3 | identificationType | اجباری | نوع شناسه هویتی | رشته کاراکتری

<p style="text-align: justify">
اکنون رشته متنی فیلد userPayload.data به همراه گواهی دیجیتال کاربر یعنی فیلد userPayload.cert و امضا دیجیتال کاربر یعنی فیلد userPayload.sign که بر روی رشته متنی فیلد userPayload.data انجام شده است، در قالب userPayload قرار می‌گیرد. نمونه کد مقابل نحوه ساخت قالب userPayload را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
</p>
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
   
> **ساخت serverPayload.data:**

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
> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayload.data بصورت String تولید خواهد کرد:

```json
{"walletType":"نوع کیف رمز پول","state":"وضعیت کیف رمز پول","description":"توضیحات","certificates":["لیست گواهی‌های دیجیتال کاربر"],"attributes":[لیستی از ویژگی‌ها که بر روی کیف رمز پول تعریف می‌شود],"walletID":"شناسه کیف رمز پول","enrollmentID":"کد ملی کاربر، شماره پاسپورت اتباع خارجی","bankID":"شناسه بانک سازنده"}
```

> **ساخت serverPayload:**

```java
ServerPayload serverPayload = new ServerPayload()
			.setUserPayload(userPayloadStr)
			.setData(createWalletSPStr);

String serverPayloadStr = writeValueAsString(serverPayload);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayload بصورت String تولید خواهد کرد:

```json
{"userPayload":"{\"data\":\"{\\\"mobileNo\\\":\\\"شماره موبایل\\\",\\\"identificationNumber\\\":\\\"شناسه هویتی\\\",\\\"identificationType\\\":\\\"نوع شناسه هویتی\\\"}\",\"sign\":\"امضا دیجیتال کاربر بر روی فیلد دیتای یوزر پیلود\",\"cert\":\"گواهی دیجیتال کاربر\"}","data":"{\"walletType\":\"نوع کیف رمز پول\",\"state\":\"وضعیت کیف رمز پول\",\"description\":\"توضیحات\",\"certificates\":[\"لیست گواهی‌های دیجیتال کاربر\"],\"attributes\":[\"لیستی از ویژگی‌ها که بر روی کیف رمز پول تعریف می‌شود\"],\"walletID\":\"شناسه کیف رمز پول\",\"enrollmentID\":\"کد ملی کاربر، شماره پاسپورت اتباع خارجی\",\"bankID\":\"شناسه بانک سازنده\"}"}
```
<p style="text-align: justify">
مرحله دوم در تراکنش ساخت کیف رمز پول، ساخت قالب serverPayload است. قالب serverPayload دارای دو فیلد serverPayload.data و serverayload.userPayload می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. مشخصه‌های فیلد serverPayload.data در جدول زیر آمده است.
<br/>
مشخصه enrollmentID برای کاربران عادی (CUSTOMER، CUSTOMER_ELEVATED) برابر کد ملی و یا شماره پاسپورت (اتباع خارجی) می‌باشد. برای کاربران تجاری نیز از کد ملی استفاده می‌گردد.
<br/>
توجه شود برای ساخت کیف رمز پول با نوع کیف بانک، نیازی به ثبت شناسه ملی (و یا هر مشخصه فرد حقیقی) نمی‌باشد، و این پارامتر بنا بر نیاز بانک می‌تواند رشته‌ای از کاراکتر و اعداد در نظر گرفته شود. همچنین محدودیتی در تعداد کیف پول نوع بانک برای یک enrollmentID وجود ندارد.
<br/>
نمونه کد مقابل نحوه ساخت قالب serverPayload.data را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
</p>

**`مشخصه‌های تراکنش ساخت کیف رمز پول - serverPayload.date`**

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | walletID | اجباری | شناسه کیف رمز پول | رشته 14 کاراکتری
2 | walletType | اجباری | نوع کیف رمز پول | رشته کاراکتری
3 | enrollmentID | اجباری | کد ملی کاربر، شماره پاسپورت اتباع خارجی | رشته کاراکتری
4 | state | اختیاری | وضعیت کیف رمز پول | رشته کاراکتری
5 | attributes | اختیاری | لیستی از ویژگی‌هاکه بر روی کیف رمز پول تعریف می‌شود | رشته کاراکتری
6 | bankID | اجباری | شناسه بانک سازنده | رشته 2 رقمی
7 | certificates | اجباری | لیست گواهی‌های دیجیتال کاربر | رشته کاراکتری
8 | description | اختیاری | توضیحات | رشته کاراکتری
9 | bornaTxID | اختیاری | شناسه تراکنش در دفتر کل برنا | رشته کاراکتری

<p style="text-align: justify">
اکنون رشته متنی فیلد قالب serverPayload.data به همراه قالب userPayload که در بالا تولید شده است، در قالب serverPayload قرار می‌گیرد. نمونه کد مقابل نحوه ساخت قالب serverPayload را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
</p>
 
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

> قطعه کد فوق خروجی زیر را به عنوان مقدار request بصورت String تولید خواهد کرد:

```json
{"serverPayload":"{\"userPayload\":\"{\\\"data\\\":\\\"{\\\\\\\"mobileNo\\\\\\\":\\\\\\\"شماره موبایل\\\\\\\",\\\\\\\"identificationNumber\\\\\\\":\\\\\\\"شناسه هویتی\\\\\\\",\\\\\\\"identificationType\\\\\\\":\\\\\\\"نوع شناسه هویتی\\\\\\\"}\\\",\\\"sign\\\":\\\"امضا دیجیتال کاربر بر روی فیلد دیتای یوزر پیلود\\\",\\\"cert\\\":\\\"گواهی دیجیتال کاربر\\\"}\",\"data\":\"{\\\"walletType\\\":\\\"نوع کیف رمز پول\\\",\\\"state\\\":\\\"وضعیت کیف رمز پول\\\",\\\"description\\\":\\\"توضیحات\\\",\\\"certificates\\\":[\\\"لیست گواهی‌های دیجیتال کاربر\\\"],\\\"attributes\\\":[\\\"لیستی از ویژگی‌ها که بر روی کیف رمز پول تعریف می‌شود\\\"],\\\"walletID\\\":\\\"شناسه کیف رمز پول\\\",\\\"enrollmentID\\\":\\\"کد ملی کاربر، شماره پاسپورت اتباع خارجی\\\",\\\"bankID\\\":\\\"شناسه بانک سازنده\\\"}\"}","sign":"امضا دیجیتال بک‌اند بر روی رشته متنی سرور پیلود"}
```
<p style="text-align: justify">
مرحله سوم در تراکنش ساخت کیف رمز پول، ساخت قالب request است. قالب request دارای دو فیلد request.serverPayload و request.sign می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد request.sign امضا دیجیتال بک‌اند بر روی رشته متنی serverPayload تولید شده در بالا می‌باشد. نمونه کد مقابل نحوه ساخت قالب request را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
</p>
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
**ارسال تراکنش**
 </aside>
 
> **ارسال تراکنش:**

```java
Map<String, byte[]> transientMap = new HashMap<>();
transientMap.put("srvCert", certificate);
final org.hyperledger.fabric.gateway.Transaction transaction = org.hyperledger.fabric.gateway.Contract.createTransaction("CreateWallet");
transaction.setTransient(transientMap);
final byte[] transferPKI = transaction.submit(requestStr);
```
<p style="text-align: justify">
مرحله چهارم در تراکنش ساخت کیف رمز پول، ارسال رشته متنی درخواست تولید شده در بالا برای ثبت بر روی دفتر کل به شبکه بلاک چین برنا می‌باشد. همچنین لازم است گواهی دیجیتال بک‌اند به صورت Transient با کلید srvCert نیز ارسال گردد. توجه شود که ارسال تراکنش ساخت کیف رمز پول به صورت Submit می‌باشد.
</p>
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<p style="text-align: justify">
<aside class="success">
در پاسخ به تراکنش ساخت کیف رمز پول، serverPayload.data به صورت رشته برگردانده می‌شود. در صورت موفقیت تراکنش، مشخصه bornaTxID برابر با شناسه تراکنش دفتر کل مقداردهی می‌شود.
</aside>
</p>

## تراکنش بروز رسانی گواهی دیجیتال کاربر 

جهت بروز رسانی گواهی‌های دیجیتال کاربر تابع UpdateWalletCertificate با نوع Submit فراخوانی می‌شود.

 <aside class="content">
**ساخت userPayload**
 </aside>

> **ساخت userPayload:**

```java
String userPayloadStr = "";
```
مرحله اول در تراکنش بروز رسانی گواهی دیجیتال کاربر، ساخت قالب userPayload است. قالب userPayload در این تراکنش رشته‌ای با مقدار خالی می‌باشد. نمونه کد مقابل نحوه ساخت قالب userPayload را نشان می‌دهد.
<br/>
<br/>
<br/>
 
 <aside class="content">
**ساخت serverPayload**
 </aside>
   
> **ساخت کیف رمز پول سرور:**

```java
UpdateWalletCertificateSP updateWalletCertificate = new UpdateWalletCertificateSP()
			.setWalletId(walletID)
			.setEnrollmentId(enrollmentId)
			.setCertificates(List.of(writeValueAsString(certificate)));

String updateWalletCertificateStr = writeValueAsString(updateWalletCertificate);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار کیف رمز پول **سرور** بصورت String در بر خواهد داشت:

```json
{"certificates":["لیست گواهی‌های دیجیتال کاربر"],"walletID":"شناسه کیف رمز پول","enrollmentID":"کد ملی کاربر، شماره پاسپورت اتباع خارجی"}
```

> **ساخت serverPayload:**

```java
ServerPayload serverPayload = new ServerPayload()
			.setUserPayload(userPayloadStr)
			.setData(updateWalletCertificateStr);

String serverPayloadStr = writeValueAsString(serverPayload);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayloadStr بصورت String در بر خواهد داشت:

```json
{"userPayload":"","data":"{\"certificates\":[\"لیست گواهی‌های دیجیتال کاربر\"],\"walletID\":\"شناسه کیف رمز پول\",\"enrollmentID\":\"کد ملی کاربر، شماره پاسپورت اتباع خارجی\"}"}
```
مرحله دوم در تراکنش بروز رسانی گواهی دیجیتال کاربر، ساخت قالب serverPayload است. قالب serverPayload دارای دو فیلد serverPayload.data و serverayload.userPayload می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد serverPayload.data همان رشته متنی کیف رمز پول سرور می‌باشد که مشخصه‌های آن در جدول زیر آمده است. نمونه کد مقابل نحوه ساخت کیف رمز پول سرور را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.

`مشخصه‌های کیف رمز پول سرور - serverPayload.date`

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | walletID | اجباری | شناسه کیف رمز پول | رشته 16 کاراکتری
2 | enrollmentID | اجباری | کد ملی کاربر، شماره پاسپورت اتباع خارجی | رشته کاراکتری
3 | certificates | اجباری | لیست گواهی‌های دیجیتال کاربر | رشته کاراکتری
4 | bornaTxID | اختیاری | شناسه تراکنش در دفتر کل برنا | رشته کاراکتری

<br/>
اکنون رشته متنی کیف رمز پول سرور که به عنوان فیلد serverPayload.data می‌باشد به همراه قالب userPayload که در بالا تولید شده است، در قالب serverPayload قرار می‌گیرد. نمونه کد مقابل نحوه ساخت قالب serverPayload را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
{"serverPayload":"{\"userPayload\":\"\",\"data\":\"{\\\"certificates\\\":[\\\"لیست گواهی‌های دیجیتال کاربر\\\"],\\\"walletID\\\":\\\"شناسه کیف رمز پول\\\",\\\"enrollmentID\\\":\\\"کد ملی کاربر، شماره پاسپورت اتباع خارجی\\\"}\"}","sign":"امضا دیجیتال بک‌اند بر روی رشته متنی سرور پیلود"}
```
مرحله سوم در تراکنش بروز رسانی گواهی دیجیتال کاربر، ساخت قالب request است. قالب request دارای دو فیلد request.serverPayload و request.sign می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد request.sign امضا دیجیتال بک‌اند بر روی رشته متنی serverPayload تولید شده در بالا می‌باشد. نمونه کد مقابل نحوه ساخت قالب request را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
**ارسال تراکنش**
 </aside>
 
> **ارسال تراکنش:**

```java
Map<String, byte[]> transientMap = new HashMap<>();
transientMap.put("srvCert", certificate);
final org.hyperledger.fabric.gateway.Transaction transaction = org.hyperledger.fabric.gateway.Contract.createTransaction("UpdateWalletCertificate");
transaction.setTransient(transientMap);
final byte[] transferPKI = transaction.submit(requestStr);
```
مرحله چهارم در تراکنش بروز رسانی گواهی دیجیتال کاربر، ارسال رشته متنی درخواست تولید شده در بالا برای ثبت بر روی دفتر کل به شبکه بلاک چین برنا می‌باشد. همچنین لازم است گواهی دیجیتال سیستمی بک‌اند به صورت Transient با کلید srvCert نیز ارسال گردد. توجه شود که ارسال تراکنش بروز رسانی گواهی دیجیتال کاربر به صورت **submit** می‌باشد.
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<aside class="success">
در پاسخ به تراکنش بروز رسانی گواهی دیجیتال کاربر، کیف پول سرور به صورت رشته متنی برگردانده می‌شود. در صورت موفقیت تراکنش، مشخصه bornaTxID برابر با شناسه تراکنش دفتر کل مقداردهی می‌شود.
</aside> 

## تراکنش بروز رسانی نوع کیف رمز پول 

جهت بروز رسانی نوع کیف رمز پول تابع UpdateWalletType با نوع Submit فراخوانی می‌شود.

 <aside class="content">
**ساخت userPayload**
 </aside>

> **ساخت userPayload:**

```java
String userPayloadStr = "";
```
مرحله اول در تراکنش بروز رسانی نوع کیف رمز پول، ساخت قالب userPayload است. قالب userPayload در این تراکنش رشته‌ای با مقدار خالی می‌باشد. نمونه کد مقابل نحوه ساخت قالب userPayload را نشان می‌دهد.
<br/>
<br/>
<br/>
 
 <aside class="content">
**ساخت serverPayload**
 </aside>
   
> **ساخت کیف رمز پول سرور:**

```java
UpdateWalletType updateWalletType = new UpdateWalletType()
			.setWalletId(walletID)
			.setWalletType(walletType);

String updateWalletTypeStr = writeValueAsString(updateWalletType);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار کیف رمز پول **سرور** بصورت String در بر خواهد داشت:

```json
{"walletType":"نوع کیف رمز پول","walletID":"شناسه کیف رمز پول"}
```

> **ساخت serverPayload:**

```java
ServerPayload serverPayload = new ServerPayload()
			.setUserPayload(userPayloadStr)
			.setData(updateWalletTypeStr);

String serverPayloadStr = writeValueAsString(serverPayload);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayloadStr بصورت String در بر خواهد داشت:

```json
{"userPayload":"","data":"{\"walletType\":\"نوع کیف رمز پول\",\"walletID\":\"شناسه کیف رمز پول\"}"}
```
مرحله دوم در تراکنش بروز رسانی نوع کیف رمز پول، ساخت قالب serverPayload است. قالب serverPayload دارای دو فیلد serverPayload.data و serverayload.userPayload می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد serverPayload.data همان رشته متنی کیف رمز پول سرور می‌باشد که مشخصه‌های آن در جدول زیر آمده است. نمونه کد مقابل نحوه ساخت کیف رمز پول سرور را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.

`مشخصه‌های کیف رمز پول سرور - serverPayload.date`

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | walletID | اجباری | شناسه کیف رمز پول | رشته 16 کاراکتری
2 | walletType | اجباری | نوع کیف رمز پول | رشته کاراکتری
3 | bornaTxID | اختیاری | شناسه تراکنش در دفتر کل برنا | رشته کاراکتری

<br/>
اکنون رشته متنی کیف رمز پول سرور که به عنوان فیلد serverPayload.data می‌باشد به همراه قالب userPayload که در بالا تولید شده است، در قالب serverPayload قرار می‌گیرد. نمونه کد مقابل نحوه ساخت قالب serverPayload را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
{"serverPayload":"{\"userPayload\":\"\",\"data\":\"{\\\"walletType\\\":\\\"نوع کیف رمز پول\\\",\\\"walletID\\\":\\\"شناسه کیف رمز پول\\\"}\"}","sign":"امضا دیجیتال بک‌اند بر روی سرور پیلود"}
```
مرحله سوم در تراکنش بروز رسانی نوع کیف رمز پول، ساخت قالب request است. قالب request دارای دو فیلد request.serverPayload و request.sign می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد request.sign امضا دیجیتال بک‌اند بر روی رشته متنی serverPayload تولید شده در بالا می‌باشد. نمونه کد مقابل نحوه ساخت قالب request را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
**ارسال تراکنش**
 </aside>
 
> **ارسال تراکنش:**

```java
Map<String, byte[]> transientMap = new HashMap<>();
transientMap.put("srvCert", certificate);
final org.hyperledger.fabric.gateway.Transaction transaction = org.hyperledger.fabric.gateway.Contract.createTransaction("UpdateWalletType");
transaction.setTransient(transientMap);
final byte[] transferPKI = transaction.submit(requestStr);
```
مرحله چهارم در تراکنش بروز رسانی نوع کیف رمز پول، ارسال رشته متنی درخواست تولید شده در بالا برای ثبت بر روی دفتر کل به شبکه بلاک چین برنا می‌باشد. همچنین لازم است گواهی دیجیتال سیستمی بک‌اند به صورت Transient با کلید srvCert نیز ارسال گردد. توجه شود که ارسال تراکنش بروز رسانی نوع کیف رمز پول به صورت **submit** می‌باشد.
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<aside class="success">
در پاسخ به تراکنش بروز رسانی نوع کیف رمز پول، کیف پول سرور به صورت رشته متنی برگردانده می‌شود. در صورت موفقیت تراکنش، مشخصه bornaTxID برابر با شناسه تراکنش دفتر کل مقداردهی می‌شود.
</aside> 

## تراکنش بروز رسانی شماره حساب کیف رمز پول 

جهت بروز رسانی شماره حساب کیف رمز پول تابع UpdateWalletAccountNumber با نوع Submit فراخوانی می‌شود. این شماره حساب در فرآیند دشارژ و تبدیل رمز پول به ریال مورد استفاده قرار می‌گیرد.

 <aside class="content">
**ساخت userPayload**
 </aside>

> **ساخت userPayload:**

```java
String userPayloadStr = "";
```
مرحله اول در تراکنش بروز رسانی شماره حساب کیف رمز پول، ساخت قالب userPayload است. قالب userPayload در این تراکنش رشته‌ای با مقدار خالی می‌باشد. نمونه کد مقابل نحوه ساخت قالب userPayload را نشان می‌دهد.
<br/>
<br/>
<br/>
 
 <aside class="content">
**ساخت serverPayload**
 </aside>
   
> **ساخت کیف رمز پول سرور:**

```java
UpdateWalletAccountNumber updateWalletAccountNumber = new UpdateWalletAccountNumber()
			.setWalletId(walletID)
			.setAccountNumber(accountNumber);

String updateWalletAccountNumberStr = writeValueAsString(updateWalletAccountNumber);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار کیف رمز پول **سرور** بصورت String در بر خواهد داشت:

```json
{"accountNumber":"شماره حساب کاربر","walletID":"شناسه کیف رمز پول"}
```

> **ساخت serverPayload:**

```java
ServerPayload serverPayload = new ServerPayload()
			.setUserPayload(userPayloadStr)
			.setData(updateWalletAccountNumberStr);

String serverPayloadStr = writeValueAsString(serverPayload);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayloadStr بصورت String در بر خواهد داشت:

```json
{"userPayload":"","data":"{\"accountNumber\":\"شماره حساب کاربر\",\"walletID\":\"شناسه کیف رمز پول\"}"}
```
مرحله دوم در تراکنش بروز رسانی شماره حساب کیف رمز پول، ساخت قالب serverPayload است. قالب serverPayload دارای دو فیلد serverPayload.data و serverayload.userPayload می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد serverPayload.data همان رشته متنی کیف رمز پول سرور می‌باشد که مشخصه‌های آن در جدول زیر آمده است. نمونه کد مقابل نحوه ساخت کیف رمز پول سرور را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.

`مشخصه‌های کیف رمز پول سرور - serverPayload.date`

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | walletID | اجباری | شناسه کیف رمز پول | رشته 16 کاراکتری
2 | accountNumber | اجباری | شماره حساب کاربر | رشته کاراکتری
3 | bornaTxID | اختیاری | شناسه تراکنش در دفتر کل برنا | رشته کاراکتری

<br/>
اکنون رشته متنی کیف رمز پول سرور که به عنوان فیلد serverPayload.data می‌باشد به همراه قالب userPayload که در بالا تولید شده است، در قالب serverPayload قرار می‌گیرد. نمونه کد مقابل نحوه ساخت قالب serverPayload را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
{"serverPayload":"{\"userPayload\":\"\",\"data\":\"{\\\"accountNumber\\\":\\\"شماره حساب کاربر\\\",\\\"walletID\\\":\\\"شناسه کیف رمز پول\\\"}\"}","sign":"امضا دیجیتال بک‌اند بر روی سرور پیلود"}
```
مرحله سوم در تراکنش بروز رسانی شماره حساب کیف رمز پول، ساخت قالب request است. قالب request دارای دو فیلد request.serverPayload و request.sign می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد request.sign امضا دیجیتال بک‌اند بر روی رشته متنی serverPayload تولید شده در بالا می‌باشد. نمونه کد مقابل نحوه ساخت قالب request را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
**ارسال تراکنش**
 </aside>
 
> **ارسال تراکنش:**

```java
Map<String, byte[]> transientMap = new HashMap<>();
transientMap.put("srvCert", certificate);
final org.hyperledger.fabric.gateway.Transaction transaction = org.hyperledger.fabric.gateway.Contract.createTransaction("UpdateWalletType");
transaction.setTransient(transientMap);
final byte[] transferPKI = transaction.submit(requestStr);
```
مرحله چهارم در تراکنش بروز رسانی شماره حساب کیف رمز پول، ارسال رشته متنی درخواست تولید شده در بالا برای ثبت بر روی دفتر کل به شبکه بلاک چین برنا می‌باشد. همچنین لازم است گواهی دیجیتال سیستمی بک‌اند به صورت Transient با کلید srvCert نیز ارسال گردد. توجه شود که ارسال تراکنش بروز رسانی شماره حساب کیف رمز پول به صورت **submit** می‌باشد.
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<aside class="success">
در پاسخ به تراکنش بروز رسانی شماره حساب کیف رمز پول، کیف پول سرور به صورت رشته متنی برگردانده می‌شود. در صورت موفقیت تراکنش، مشخصه bornaTxID برابر با شناسه تراکنش دفتر کل مقداردهی می‌شود.
</aside> 

## تراکنش دریافت موجودی کیف رمز پول 

جهت دریافت موجودی کیف رمز پول تابع WalletBalance با نوع Evaluate فراخوانی می‌شود.

 <aside class="content">
**ساخت userPayload**
 </aside>

> **ساخت کیف رمز پول کاربر:**

```java
WalletBalance walletBalance = new WalletBalance()
			.setTokenID(tokenId)
			.setTokenSymbol(tokenSymbol)
			.setWalletId(walletID);

String walletBalanceStr = writeValueAsString(walletBalance);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار کیف رمز پول **کاربر** بصورت String در بر خواهد داشت:

```json
{"tokenID":"شناسه رمز پول","tokenSymbol":"نماد رمز پول","walletID":"شناسه کیف رمز پول"}
```
> **ساخت userPayload:**

```java
UserPayload userPayload = new UserPayload()
			.setData(walletBalanceStr)
			.setCert(writeValueAsString(certificate))
			.setSign(sign(walletBalanceStr));

String userPayloadStr = writeValueAsString(userPayload);
```
> قطعه کد فوق خروجی زیر را به عنوان مقدار userPayloadStr بصورت String در بر خواهد داشت:

```json
{"data":"{\"tokenID\":\"شناسه رمز پول\",\"tokenSymbol\":\"نماد رمز پول\",\"walletID\":\"شناسه کیف رمز پول\"}","sign":"امضا دیجیتال کاربر بر روی فیلد دیتای یوزر پیلود","cert":"گواهی دیجیتال کاربر"}
```
مرحله اول در تراکنش دریافت موجودی کیف رمز پول، ساخت قالب userPayload است. قالب userPayload دارای سه فیلد userPayload.data، userPayload.cert و userPayload.sign می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد userPayload.data همان رشته متنی کیف رمز پول کاربر می‌باشد که مشخصه‌های آن در جدول زیر آمده است. نمونه کد مقابل نحوه ساخت کیف رمز پول کاربر را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.

`مشخصه‌های کیف رمز پول کاربر - userPayload.date`

    ردیف | نام مشخصه | نوع | شرح مشخصه | ساختار 
--------- | ------- | -----------| ------- | -----------| ------- 
1 | walletID | اجباری | شناسه کیف رمز پول | رشته 16 کاراکتری
2 | tokenSymbol | اختیاری | نماد رمز پول | رشته کاراکتری
3 | tokenIID | اجباری | شناسه رمز پول | رشته کاراکتری
<br/>
اکنون رشته متنی کیف رمز پول کاربر که به عنوان فیلد userPayload.data می‌باشد به همراه گواهی دیجیتال کاربر یعنی فیلد userPayload.cert و امضا دیجیتال کاربر یعنی فیلد userPayload.sign که بر روی رشته متنی فیلد userPayload.data انجام شده است، در قالب userPayload قرار می‌گیرد. نمونه کد مقابل نحوه ساخت قالب userPayload را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.   
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
String serverPayloadData = "";
```

> **ساخت serverPayload:**

```java
ServerPayload serverPayload = new ServerPayload()
			.setUserPayload(userPayloadStr)
			.setData(serverPayloadData);

String serverPayloadStr = writeValueAsString(serverPayload);
```

> قطعه کد فوق خروجی زیر را به عنوان مقدار serverPayloadStr بصورت String در بر خواهد داشت:

```json
{"userPayload":"{\"data\":\"{\\\"tokenID\\\":\\\"شناسه رمز پول\\\",\\\"tokenSymbol\\\":\\\"نماد رمز پول\\\",\\\"walletID\\\":\\\"شناسه کیف رمز پول\\\"}\",\"sign\":\"امضا دیجیتال کاربر بر روی فیلد دیتای یوزر پیلود\",\"cert\":\"گواهی دیجیتال کاربر\"}","data":""}
```
مرحله دوم در تراکنش دریافت موجودی کیف رمز پول، ساخت قالب serverPayload است. قالب serverPayload دارای دو فیلد serverPayload.data و serverayload.userPayload می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد serverPayload.data همان رشته متنی کیف رمز پول سرور می‌باشد که در این تراکنش رشته‌ای با مقدار خالی است. نمونه کد مقابل نحوه ساخت کیف رمز پول سرور را نشان می‌دهد.
<br/>
<br/>
اکنون رشته متنی کیف رمز پول سرور که به عنوان فیلد serverPayload.data می‌باشد به همراه قالب userPayload که در بالا تولید شده است، در قالب serverPayload قرار می‌گیرد. نمونه کد مقابل نحوه ساخت قالب serverPayload را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
{"serverPayload":"{\"userPayload\":\"{\\\"data\\\":\\\"{\\\\\\\"tokenID\\\\\\\":\\\\\\\"شناسه رمز پول\\\\\\\",\\\\\\\"tokenSymbol\\\\\\\":\\\\\\\"نماد رمز پول\\\\\\\",\\\\\\\"walletID\\\\\\\":\\\\\\\"شناسه کیف رمز پول\\\\\\\"}\\\",\\\"sign\\\":\\\"امضا دیجیتال کاربر بر روی فیلد دیتای یوزر پیلود\\\",\\\"cert\\\":\\\"گواهی دیجیتال کاربر\\\"}\",\"data\":\"\"}","sign":""}
```
مرحله سوم در تراکنش دریافت موجودی کیف رمز پول، ساخت قالب request است. قالب request دارای دو فیلد request.serverPayload و request.sign می‌باشد که شرح آن در بخش قالب ارسال ارایه شده است. فیلد request.sign امضا دیجیتال بک‌اند بر روی رشته متنی serverPayload تولید شده در بالا می‌باشد. نمونه کد مقابل نحوه ساخت قالب request را نشان می‌دهد که لازم است در انتها به رشته متنی تبدیل شود.
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
**ارسال تراکنش**
 </aside>
 
> **ارسال تراکنش:**

```java
Map<String, byte[]> transientMap = new HashMap<>();
transientMap.put("srvCert", certificate);
final org.hyperledger.fabric.gateway.Transaction transaction = org.hyperledger.fabric.gateway.Contract.createTransaction("UpdateWalletType");
transaction.setTransient(transientMap);
final byte[] transferPKI = transaction.submit(requestStr);
```
مرحله چهارم در تراکنش دریافت موجودی کیف رمز پول، ارسال رشته متنی درخواست تولید شده در بالا برای ثبت بر روی دفتر کل به شبکه بلاک چین برنا می‌باشد. همچنین لازم است گواهی دیجیتال سیستمی بک‌اند به صورت Transient با کلید srvCert نیز ارسال گردد. توجه شود که ارسال تراکنش دریافت موجودی کیف رمز پول به صورت **evaluate** می‌باشد.
<br/>
<br/>
<br/>
<br/>
<br/>
<br/>
<aside class="success">
در پاسخ به تراکنش دریافت موجودی کیف رمز پول، مقدار عددی برابر موجودی کیف رمز پول ارسال می‌شود.
</aside> 




























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


test
test
test
test
test

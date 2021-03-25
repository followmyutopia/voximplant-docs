<!-- vox.description: How to choose a number for SMS in Voximplant. -->
<!-- vox.rank: 1 -->
<!-- vox.filters: isMessaging -->
# How to choose a phone number
If you want to send and receive SMS in Voximplant, make sure that the chosen region and phone category support SMS functionality when purchasing a phone number. **Note that virtual numbers do not support SMS.**

To check if the number is suitable, go to the [numbers](https://manage.voximplant.com/numbers/my_numbers) section in your control panel, click [buy a new phone number](https://manage.voximplant.com/numbers/buy_number), and select a number. If the selected number does not support SMS, the following note appears:

![](/assets/images/guides-sms-phonenumber-nosmssupport.png)

If you do not see this message, the number supports SMS functionality:

![](/assets/images/guides-sms-phonenumber-buyanumber.png)

## HTTP API request
You can check which numbers support SMS via the [GetPhoneNumbers](https://voximplant.com/docs/references/httpapi/managing_phone_numbers#getphonenumbers) HTTP request with **country_code** and **phone_category_name**. If the **is_sms_supported** property equals to **true** in the returned object, the phone numbers in this region support SMS.

```vox.multicode
env: http_api
title:http request
description:
====
highlight : 
link : 
name : script
lang : curl
----
curl "https://api.voximplant.com/platform_api/GetPhoneNumberRegions/?account_id=1&api_key=eec36d6c-a0eb-46b5-a006-1c2b65343bac&country_code=GB&phone_category_name=MOBILE`"
```
```vox.multicode
env: http_api
title:http request
description:
====
highlight : 
link : 
name : return
lang : curl
----
{
    "result": [{
        "is_need_regulation_address": false,
        "country_code": "GB",
        "phone_region_id": 27085,
        "is_sms_supported": true,
        "phone_count": 408,
        "phone_installation_price": 0.0,
        "phone_region_name": "7",
        "phone_period": "0-1-0 0:0:0",
        "phone_category_name": "MOBILE",
        "phone_price": 1.2,
        "multiple_numbers_price": [],
        "phone_region_code": "7"
    }],
    "count": 1
}
```
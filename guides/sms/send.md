<!-- vox.description: How to send an sms in Voximplant. -->
<!-- vox.rank: 2 -->
<!-- vox.filters: isMessaging -->
# How to send an SMS
Sending messages with Voximplant could be extremely useful to inform customers about price movements, policy changes, etc. And do not forget about the customer verification purposes. Most phone numbers support SMS but you can check [How to choose a phone number](/docs/guides/sms/phonenumber) article to be sure.

## Enable SMS functionality
Enable SMS messaging via the [ControlSms](/docs/references/httpapi/managing_sms#controlsms) HTTP API before sending or receiving SMS. If SMS is supported and enabled, you can send SMS from a phone number via the [SendSmsMessage](/docs/references/httpapi/managing_sms#sendsmsmessage) HTTP API and receive via the [InboundSmsCallback](/docs/references/httpapi/inboundsmscallback) property of the HTTP callback. Each inbound SMS message is billed according to the [pricing](https://voximplant.com/pricing).

After you bought a number, go to **Numbers** —> **My phone numbers**. There is an **SMS Enabled** checkbox which is not selected by default. To enable it, choose **Edit** from the phone number's menu. Here, toggle **Disable/Enable SMS** and click **Save**.

![](/assets/images/guides-sms-send-enabled.png)

![](/assets/images/guides-sms-send-toggle.jpg)

To perform this action via HTTP API, use the [ControlSms](/docs/references/httpapi/managing_sms#controlsms) method.


```vox.multicode
env: http_api
title:Enabling SMS
description:
====
highlight : 
link : 
name : script
lang : curl
----
curl "https://api.voximplant.com/platform_api/ControlSms/?account_id=1&api_key=eec36d6c-a0eb-46b5-a006-1c2b65343bac&phone_number=447443332211&command=enable"
```
```vox.multicode
env: http_api
title:Enabling SMS
description:
====
highlight : 
link : 
name : return
lang : curl
----
{
    "result": 1
}
```
```vox.multicode
env: http_api
title:Disabling SMS
description:
====
highlight : 
link : 
name : script
lang : curl
----
curl "https://api.voximplant.com/platform_api/ControlSms/?account_id=1&api_key=eec36d6c-a0eb-46b5-a006-1c2b65343bac&phone_number=447443332211&command=disable"

```
```vox.multicode
env: http_api
title:Disabling SMS
description:
====
highlight : 
link : 
name : return
lang : curl
----
{
    "result": 1
}
```

## Send one-way SMS
To send one-way SMS, use the [A2PSendSms](/docs/references/httpapi/sms#a2psendsms) method with the following parameters:
- **src_number** – source phone number
- **dst_numbers** – destination phone numbers separated by the ';' symbol
- **text** – message text, up to 1600 characters. We split long messages greater than 160 GSM-7 characters or 70 UTF-16 characters into multiple segments. Each segment is billed as one message

Let us see how to send a message with the text "Test one-way messages" 447443332211 to the phone numbers 447443332212 and 447443332213.

```vox.multicode
env: http_api
title:Sending one-way SMS
description:
====
highlight : 
link : 
name : script
lang : curl
----
curl "https://api.voximplant.com/platform_api/A2PSendSms?account_id=1&api_key=eec36d6c-a0eb-46b5-a006-1c2b65343bac&src_number=447443332211&dst_numbers=447443332212;447443332213&text=Test%20message"

```
```vox.multicode
env: http_api
title:Sending one-way SMS
description:
====
highlight : 
link : 
name : return
lang : curl
----
{
  "result": [
    {
      "transaction_id": 3996474,
      "destination_number": "447443332213"
    }
  ],
  "fragments_count": 1,
  "failed": [
    {
      "destination_number": "447443332212",
      "error_description": "SMS failed to send.",
      "error_code": 385
    }
  ]
}
```

## Send two-way SMS
To send two-way SMS, use the [SendSmsMessage](/docs/references/httpapi/managing_sms#sendsmsmessage) method with the following parameters:
- **source** – source phone number
- **destination** – destination phone number
- **sms_body** – message text, up to 765 characters. We split long messages greater than 160 GSM-7 characters or 70 UTF-16 characters into multiple segments. Each segment is billed as one message

Let us see how to send a message with the text "Test message" from the phone number 447443332211 to the phone number 447443332212:

```vox.multicode
env: http_api
title:Sending two-way SMS
description:
====
highlight : 
link : 
name : script
lang : curl
----
curl "https://api.voximplant.com/platform_api/SendSmsMessage/?account_id=1&api_key=eec36d6c-a0eb-46b5-a006-1c2b65343bac&source=447443332211&destination=447443332212&sms_body=Test%20message"
```
```vox.multicode
env: http_api
title:Sending two-way SMS
description:
====
highlight : 
link : 
name : return
lang : curl
----
{
    "result": 1,
    "fragments_count": 1
}
```

**Frangments_count** in the response is the number of fragments into which the message was divided.


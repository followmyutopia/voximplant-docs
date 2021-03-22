<!-- vox.description: How to receive an sms in Voximplant. -->
<!-- vox.rank: 3 -->
<!-- vox.filters: isMessaging -->
# How to receive an SMS
If you set up the receiving SMS to a Voximplant number, your backend server can handle callbacks from our cloud and retrieve SMS. These can be alert messages, customers feedback and much more. You can store all the SMS on your backend for further analysis and research.

For each inbound SMS, a specific [callback](/docs/howtos/integration/httpapi/callbacks) is triggered – [InboundSmsCallback](/docs/references/httpapi/inboundsmscallback), which contains [InboundSmsCallbackItem](/docs/references/httpapi/structure/inboundsmscallbackitem) with source and destination numbers, and the SMS text.

## Callbacks usage

With callbacks, the Voximplant HTTP API becomes even more useful for integration and monitoring. 

To use them, go to the Voximplant control panel's Webhooks section and click **Add** in the middle of the screen. The two textfields appear: **Callback URL** and **Security salt**. In the **Сallback URL**, specify the URL of your backend server and click **Save**.

![](/assets/images/2guides-sms-receive-webhooks.png)

Then, set up your backend server and make it communicate with the Voximplant HTTP API. See [this article](/docs/howtos/integration/httpapi/callbacks) for HTTP callback.

<!-- vox.description: How to retransmit events in Voximplant Messaging platform and why you need it. -->
<!-- vox.rank: 4 -->
<!-- vox.filters: isAudio,isVideo,isMessaging,isOmnichannel -->
# Retransmit events
This article will teach you how to retransmit events and why you need it.

## What is event retransmission
Each action in a Voximplant conversation is represented by **events**. This includes sending, receiving, and editing messages; adding and removing participants, changing user permissions and conversation preferences, and much more.

When a user is logged in to the conversation, the user tracks these events via event listeners. But when a user is logged off, the user *cannot track any events*.

That is why, when you log a user into a conversation, you need to **retransmit events** to update previous messages and other conversation changes.

## How to retransmit events
In **WebSDK**, use the [retransmitEvents](/docs/references/websdk/voximplant/messaging/conversation#retransmitevents) method to retransmit up to 100 previous events.

In the **iOS SDK**, use the following methods to retransmit events:

- [retransmitEvents(from:count:completion:)](/docs/references/iossdk/messaging/viconversation#retransmiteventsfromcountcompletion)
- [retransmitEvents(from:to:completion:)](/docs/references/iossdk/messaging/viconversation#retransmiteventsfromtocompletion)
- [retransmitEvents(to:count:completion:)](/docs/references/iossdk/messaging/viconversation#retransmiteventstocountcompletion)

In the **Android SDK**, use the following methods as well:

- [retransmitEvents](/docs/references/androidsdk/messaging/iconversation#retransmitevents)
- [retransmitEventsFrom](/docs/references/androidsdk/messaging/iconversation#retransmiteventsfrom)
- [retransmitEventsTo](/docs/references/androidsdk/messaging/iconversation#retransmiteventsto)

Please refer to given API reference links to learn more about these methods.

Here is a code example of how to retransmit events in Web, iOS and Android SDKs:

```vox.multicode
env: media
title:Event retransmission
description:
====
highlight : 
link : 
name : 
lang : javascript
----
public retransmitMessageEvents(currentConversation: any, lastEvent ?: number) {
    lastEvent = lastEvent ? lastEvent : currentConversation.lastSeq;
    const eventFrom = lastEvent - 100 > 0 ? lastEvent - 100 : 1;
    store.commit('conversations/updateLastEvent', eventFrom - 1);

    return currentConversation.retransmitEvents(eventFrom, lastEvent)
        .then(async (e) => {
            // event handling
        })
        .catch(logError);
}

```
```vox.multicode
env: media
title:Event retransmission
description:
====
highlight : 
link : 
name : 
lang : swift
----
func requestMessengerEvents(
    for conversation: VIConversation,
    completion: @escaping (Result<[VIMessengerEvent], Error>) -> Void
) {
    conversation.retransmitEvents(
        to: conversation.lastSequence,
        count: UInt(100),
        completion: VIMessengerCompletion<VIRetransmitEvent> (
            success: { retransmitEvents in
                completion(.success(retransmitEvents.events))
            },
            failure: { errorEvent in
                completion(.failure(NSError.buildError(from: errorEvent)))
            }
        )
    )
}
```
```vox.multicode
env: media
title:Event retransmission
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
fun requestMessengerEvents(
    conversation: IConversation,
    numberOfEvents: Int,
    sequence: Long,
    completion: (Result<List<IMessengerEvent>>) -> Unit
) {
    conversation.retransmitEventsTo(
        sequence,
        numberOfEvents,
        object : IMessengerCompletionHandler<IRetransmitEvent> {
            override fun onSuccess(retransmitEvents: IRetransmitEvent) {
                completion(success(retransmitEvents.events))
            }
            override fun onError(errorEvent: IErrorEvent) {
                completion(failure(buildError(errorEvent)))
            }
        }
    )
}
```

The **retransmitEvents** method requires two parameters: **to** and **count**.

The **to** parameter's value equals the conversation's **lastSequence** property, and the **count** parameter's value is an integer that represents the number of events.

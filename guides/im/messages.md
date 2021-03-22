<!-- vox.description: How to send, edit, and delete messages in an Voximplant IM conversation -->
<!-- vox.rank: 3 -->
<!-- vox.filters: isAudio,isVideo,isMessaging,isOmnichannel -->
# Send, edit and delete messages
This article will teach you how to send, edit, and delete messages in an Voximplant IM conversation:

## Send a message
Use the [sendMessage](/docs/references/websdk/voximplant/messaging/conversation#sendmessage) method to send a message to a conversation.

To receive messages, handle the message sending events. Take a look on how we implement it in the code:

```vox.multicode
env: media
title:Send a message
description:
====
highlight : 
link : 
name : 
lang : javascript
----
// Send messages and receive SendMessage events

private addMessengerEventListeners() {

    // Listen to incoming messages
    MessengerService.messenger.on(VoxImplant.Messaging.MessengerEvents.SendMessage, (e) => store.dispatch('conversations/onMessageSent', e));
}

public sendMessage(currentConversation: any, text: string) {
    return currentConversation.sendMessage(text, [{}])
        .catch(logError);
}

```
```vox.multicode
env: media
title:Send a message
description:
====
highlight : 
link : 
name : 
lang : swift
----
// Send messages and receive didSendMessage events
//
// Note that if a completion block is specified in a method call,
// the didSendMessage event won’t be triggered

func sendMessage(
    with text: String,
    in conversation: VIConversation,
    completion: @escaping (Result<VIMessageEvent, Error>) -> Void
) {
    conversation.sendMessage(text, payload: nil, completion:
        VIMessengerCompletion<VIMessageEvent> (
            success: { messageEvent in
                completion(.success(messageEvent))
            },
            failure: { errorEvent in
                completion(.failure(NSError.buildError(from: errorEvent)))
            }
        )
    )
}

func messenger(_ messenger: VIMessenger,
               didSendMessage event: VIMessageEvent) {
    delegate?.didReceive(messageEvent: event)
}

```
```vox.multicode
env: media
title:Send a message
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
// Send messages and receive onSendMessage events
//
// Note that if a completion block is specified in a method call,
// the onSendMessage event won’t be triggered

fun sendMessage(
    text: String,
    conversation: IConversation,
    completion: (Result<IMessageEvent>) -> Unit
) {
    conversation.sendMessage(
        text,
        null,
        object : IMessengerCompletionHandler<IMessageEvent> {
            override fun onSuccess(messageEvent: IMessageEvent) {
                completion(success(messageEvent))
            }
            override fun onError(errorEvent: IErrorEvent) {
                completion(failure(buildError(errorEvent)))
            }
        }
    )
}

override fun onSendMessage(event: IMessageEvent) {
    listener?.onMessageEvent(event)
}

```

## Edit a message
Use the [Message.update](/docs/references/websdk/voximplant/messaging/message#update) method to edit your message.

With default permissions, users can edit own messages only. Users with the [canEditAll](/docs/references/websdk/voximplant/messaging/conversationparticipant#caneditall) permissions can also edit others' messages.

> :info: **Info**: To track changes of other messages, you have to subscribe to the **didEditMessage** / **onEditMessage** and **didRemoveMessage** / **onRemoveMessage**

```vox.multicode
env: media
title:Edit a message
description:
====
highlight : 
link : 
name : 
lang : javascript
----
editMessage: (context, newData) => {
    newData.message.text = newData.newText;
    MessengerService.get().updateMessage(newData.message)
        .catch(logError);
}

```
```vox.multicode
env: media
title:Edit a message
description:
====
highlight : 
link : 
name : 
lang : swift
----
// Edit messages and receive didEditMessage events:
// Note that if a completion block is specified in a method call,
// the didEditMessage event won’t be triggered:
func edit(
    message: VIMessage,
    with text: String,
    completion: @escaping (Result<VIMessageEvent, Error>) -> Void
) {
    message.update(text, payload: nil, completion:
        VIMessengerCompletion<VIMessageEvent> (
            success: { messageEvent in
                completion(.success(messageEvent))
            },
            failure: { errorEvent in
                completion(.failure(NSError.buildError(from: errorEvent)))
            }
        )
    )
}

func messenger(_ messenger: VIMessenger,
               didEditMessage event: VIMessageEvent) {
    delegate?.didReceive(messageEvent: event)
}

```
```vox.multicode
env: media
title:Edit a message
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
fun editMessage(
    message: IMessage,
    text: String,
    completion: (Result<IMessageEvent>) -> Unit
) {
    message.update(
        text,
        null,
        object : IMessengerCompletionHandler<IMessageEvent> {
            override fun onSuccess(messageEvent: IMessageEvent) {
                completion(success(messageEvent))
            }
            override fun onError(errorEvent: IErrorEvent) {
                completion(failure(buildError(errorEvent)))
            }
        }
    )
}

override fun onEditMessage(event: IMessageEvent) {
    listener?.onMessageEvent(event)
}

```

## Delete a message
Use the [Message.remove](/docs/references/websdk/voximplant/messaging/message#remove) method to delete a message from the conversation.

With default permissions, users can delete own messages only. Users with the [canRemoveAll](/docs/references/websdk/voximplant/messaging/conversationparticipant#canremoveall) permissions can also delete others' messages.

> :info: **Info**: To track changes of other messages, you have to subscribe to the **didEditMessage** / **onEditMessage** and **didRemoveMessage** / **onRemoveMessage**

```vox.multicode
env: media
title:Delete a message
description:
====
highlight : 
link : 
name : 
lang : javascript
----
deleteMessage: (context, message) => {
    MessengerService.get().removeMessage(message)
        .catch(logError);
}

```
```vox.multicode
env: media
title:Delete a message
description:
====
highlight : 
link : 
name : 
lang : swift
----
// Note that if a completion block is specified in a method call,
// the didRemoveMessage event won’t be triggered:
func remove(
    message: VIMessage,
    completion: @escaping (Result<VIMessageEvent, Error>) -> Void
) {
    message.remove(completion:
        VIMessengerCompletion<VIMessageEvent> (
            success: { messageEvent in
                completion(.success(messageEvent))
            },
            failure: { errorEvent in
                completion(.failure(NSError.buildError(from: errorEvent)))
            }
        )
    )
}

func messenger(_ messenger: VIMessenger,
               didRemoveMessage event: VIMessageEvent) {
    delegate?.didReceive(messageEvent: event)
}

```
```vox.multicode
env: media
title:Delete a message
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
// Note that if a completion block is specified in a method call,
// the onRemoveMessage event won’t be triggered:
fun removeMessage(
    message: IMessage,
    completion: (Result<IMessageEvent>) -> Unit
) {
    message.remove(
        object : IMessengerCompletionHandler<IMessageEvent> {
            override fun onSuccess(messageEvent: IMessageEvent) {
                completion(success(messageEvent))
            }
            override fun onError(errorEvent: IErrorEvent) {
                completion(failure(buildError(errorEvent)))
            }
        }
    )
}

override fun onRemoveMessage(event: IMessageEvent) {
    listener?.onMessageEvent(event)
}

```

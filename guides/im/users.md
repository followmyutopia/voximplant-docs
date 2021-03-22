<!-- vox.description: How to add, remove, and manage users in an Voximplant IM conversation -->
<!-- vox.rank: 2 -->
<!-- vox.filters: isAudio,isVideo,isMessaging,isOmnichannel -->
# Add, remove and manage users
This article will teach you how to add, remove, and manage users in a conversation.

To manage users, you need to be an [administrator](/docs/references/websdk/voximplant/messaging/conversationparticipant#canmanageparticipants) or an [owner](/docs/references/websdk/voximplant/messaging/conversationparticipant#isowner) in the conversation. To make someone an administrator, the chat room's owner needs to set the **canManageParticipants** permission of a chat member to **true**.

## Add and remove users
Use the [addParticipants](/docs/references/websdk/voximplant/messaging/conversation#addparticipants) method to add new users to the conversation. To remove users from a conversation, use the [removeParticipants](/docs/references/websdk/voximplant/messaging/conversation#removeparticipants) method.

```vox.multicode
env: media
title:Add and remove users
description:
====
highlight : 
link : 
name : 
lang : javascript
----
public addParticipants(currentConversation: Conversation, userIds: number[]) {
    return currentConversation.addParticipants(userIds.map((userId) => ({
        userId,
        ...currentConversation.customData.permissions,
    }))).catch(logError);
}

public removeParticipants(currentConversation: Conversation, userIds: number[]) {
    return currentConversation.removeParticipants(userIds.map((userId) => ({ userId }))).catch(logError);
}

```
```vox.multicode
env: media
title:Add and remove users
description:
====
highlight : 
link : 
name : 
lang : swift
----
func add(
    participants: [VIConversationParticipant],
    to conversation: VIConversation,
    completion: @escaping (Result<VIConversationEvent, Error>) -> Void
) {
    conversation.addParticipants(participants, completion:
        VIMessengerCompletion<VIConversationEvent> (
            success: { conversationEvent in
                completion(.success(conversationEvent))
            },
            failure: { errorEvent in
                completion(.failure(NSError.buildError(from: errorEvent)))
            }
        )
    )
}

func remove(
    participants: [VIConversationParticipant],
    from conversation: VIConversation,
    completion: @escaping (Result<VIConversationEvent, Error>) -> Void
) {
    conversation.removeParticipants(participants, completion:
        VIMessengerCompletion<VIConversationEvent> (
            success: { conversationEvent in
                completion(.success(conversationEvent))
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
title:Add and remove users
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
fun addParticipants(
    participants: List<ConversationParticipant>,
    conversation: IConversation,
    completion: (Result<IConversationEvent>) -> Unit
) {
    conversation.addParticipants(
        participants,
        object : IMessengerCompletionHandler<IConversationEvent> {
            override fun onSuccess(conversationEvent: IConversationEvent) {
                completion(success(conversationEvent))
            }
            override fun onError(errorEvent: IErrorEvent) {
                completion(failure(buildError(errorEvent)))
            }
        }
    )
}

fun removeParticipants(
    participants: List<ConversationParticipant>,
    conversation: IConversation,
    completion: (Result<IConversationEvent>) -> Unit
) {
    conversation.removeParticipants(
        participants,
        object : IMessengerCompletionHandler<IConversationEvent> {
            override fun onSuccess(conversationEvent: IConversationEvent) {
                completion(success(conversationEvent))
            }
            override fun onError(errorEvent: IErrorEvent) {
                completion(failure(buildError(errorEvent)))
            }
        }
    )
}

```

## Manage user permissions
Use the [editParticipants](/docs/references/websdk/voximplant/messaging/conversation#editparticipants) method to change user permissions.

By default, the conversation's members have the following permissions: **canRead**, **canWrite**, **canRemove**. Please [refer to this API](/docs/references/websdk/voximplant/messaging/conversationparticipant) to read more about all the available permission options.

```vox.multicode
env: media
title:Manage user permissions
description:
====
highlight : 
link : 
name : 
lang : javascript
----
editConversation: ({ getters }, newData) => {
    if (newData.title && newData.title !== getters.currentConversation.title) {
        getters.currentConversation.setTitle(newData.title)
            .catch(logError);
    }

    if (newData.customData) {
        getters.currentConversation.setCustomData({ ...getters.currentConversation.customData, ...newData.customData })
            .catch(logError);
    }
}

```
```vox.multicode
env: media
title:Manage user permissions
description:
====
highlight : 
link : 
name : 
lang : swift
----
func edit(
    participants: [VIConversationParticipant],
    in conversation: VIConversation,
    completion: @escaping (Result<VIConversationEvent, Error>) -> Void
) {
    conversation.editParticipants(participants, completion:
        VIMessengerCompletion<VIConversationEvent> (
            success: { conversationEvent in
                completion(.success(conversationEvent))
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
title:Manage user permissions
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
fun editParticipants(
    participants: List<ConversationParticipant>,
    conversation: IConversation,
    completion: (Result<IConversationEvent>) -> Unit
) {
    conversation.editParticipants(
        participants,
        object : IMessengerCompletionHandler<IConversationEvent> {
            override fun onSuccess(conversationEvent: IConversationEvent) {
                completion(success(conversationEvent))
            }
            override fun onError(errorEvent: IErrorEvent) {
                completion(failure(buildError(errorEvent)))
            }
        }
    )
}

```

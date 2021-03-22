<!-- vox.description: How to create, manage, and remove conversations
 with Voximplant. -->
<!-- vox.rank: 1 -->
<!-- vox.filters: isAudio,isVideo,isMessaging,isOmnichannel -->
# Create, manage, and remove conversations
This article will teach you how to create, manage, and remove an IM conversation with Voximplant.

There are two types of conversations in Voximplant: [direct](/docs/references/websdk/voximplant/messaging/conversation#direct) and [public](/docs/references/websdk/voximplant/messaging/conversation#publicjoin). A **direct** conversation is one between two users only. A **public** conversation, or a chat room, can consist of up to 1000 users.

Before creating a conversation, [create an application](/docs/gettingstarted/basicconcepts/applications), [users](/docs/gettingstarted/basicconcepts/applications), and [add Voximplant SDK](/docs/gettingstarted/makeanapp) to your project.

## Create a conversation
To create a conversation, use the [createConversation](/docs/howtos/messaging/im/docs/references/websdk/voximplant/messaging/messenger#createconversation) method:

```vox.multicode
env: media
title:Create a conversation
description:
====
highlight : 
link : 
name : 
lang : javascript
----
private createNewConversation(participants, title: string, direct: boolean, publicJoin: boolean, uber: boolean, customData: object) {
    return MessengerService.messenger.createConversation(participants, title, direct, publicJoin, uber, customData);
}


public createChat(newChatData: NewChatData) {
    const permissions: Permissions = {
        canWrite: true,
        canEdit: true,
        canRemove: true,
        canManageParticipants: true,
        canEditAll: false,
        canRemoveAll: false,
    };

    const participants = newChatData.usersId.map((userId: number) => {
        return {
            userId,
            ...permissions,
        };
    });

    return this.createNewConversation(
        participants,
        newChatData.title,
        false,
        newChatData.isPublic,
        newChatData.isUber,
        {
            type: 'chat',
            image: newChatData.avatar,
            description: newChatData.description,
            permissions,
        });
}

```
```vox.multicode
env: media
title:Create a conversation
description:
====
highlight : 
link : 
name : 
lang : swift
----
func createConversation(
    with title: String, and userImids: [NSNumber],
    description: String, pictureName: String?,
    isPublic: Bool, isUber: Bool, permissions: Permissions,
    completion: @escaping (Result<VIConversation, Error>) -> Void
) {
    let config = VIConversationConfig()
    config.title = title
    config.isDirect = false
    config.isUber = isUber
    config.isPublicJoin = isPublic
    config.participants = userImIds
        .map { self.builder.buildVIParticipant(with: $0, and: permissions) }
    config.customData = builder
        .buildCustomData(for: .chat, with: pictureName, and: description)
    
    messenger.createConversation(config, completion:
        VIMessengerCompletion<VIConversationEvent> (
            success: { conversationEvent in
                completion(.success(conversationEvent.conversation))
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
title:Create a conversation
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
fun createConversation(
    title: String, userImIds: List<Long>, description: String,
    pictureName: String?, isPublic: Boolean, isUber: Boolean,
    permissions: Permissions, completion: (Result<IConversationEvent>) -> Unit
) {
    val config = ConversationConfig.createBuilder()
        .setTitle(title)
        .setDirect(false)
        .setUber(isUber)
        .setPublicJoin(isPublic)
        .setParticipants(
            userImIds
                .map { builder.buildParticipant(it, permissions) }
        )
        .setCustomData(
            builder.buildCustomData(
                ConversationType.CHAT, pictureName, description
            )
        )
        .build()

    messenger.createConversation(
        config,
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
By default, the conversation's members have the following permissions: **canRead**, **canWrite**, **canRemove**. You can specify another default [permissions](/docs/references/websdk/voximplant/messaging/conversationparticipant) at conversation creation.

The user that created the conversation becomes its [owner](/docs/references/websdk/voximplant/messaging/conversationparticipant#isowner). After the conversation is created, other users can join it or be invited by administrators.

## Manage a conversation

[Administrators](/docs/references/websdk/voximplant/messaging/conversationparticipant#canmanageparticipants) of a chat room or its [owner](/docs/references/websdk/voximplant/messaging/conversationparticipant#isowner) can change the following preferences of the conversation:

- Title
- User permissions
- Add and remove the participants
- Custom data

> :info: **Note**: To make someone an administrator, the chat room's owner needs to set the **canManageParticipants** permission of a chat member to **true**.

Here is how you can manage a chat room's preferences:

```vox.multicode
env: media
title:Manage a conversation's preferences
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
title:Manage a conversation's preferences
description:
====
highlight : 
link : 
name : 
lang : swift
----
// Update the VIConversation instance properties and
// call update(conversation:completion:) to update this conversation;
// for example:
// conversation.title = “newTitle”
// conversation.isPublicJoin = true
// update(conversation: conversation) { result in
//     // handle result
// }
func update(
    conversation: VIConversation,
    completion: @escaping (Result<VIConversationEvent, Error>) -> Void
) {
    conversation.update(completion:
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
title:Manage a conversation's preferences
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
// Update the IConversation instance properties and
// call updateConversation(conversation:completion:) to update this conversation;
// for example:
// conversation.title = “newTitle”
// conversation.isPublicJoin = true
// updateConversation(conversation) { result ->
//     // handle result
// }
fun updateConversation(
    conversation: IConversation,
    completion: (Result<IConversationEvent>) -> Unit
) {
    conversation.update(
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

Refer to the [chat room user management](/docs/guides/im/users) guide to learn how to add and remove users and how to change their permissions.

## Remove a conversation

To remove a conversation, the [administrators](/docs/references/websdk/voximplant/messaging/conversationparticipant#canmanageparticipants) of a chat room or its [owner](/docs/references/websdk/voximplant/messaging/conversationparticipant#isowner) need to delete all the participants via the [removeParticipants](/docs/references/websdk/voximplant/messaging/conversation#removeparticipants) method, then leave the conversation via the [leaveConversation](/docs/references/websdk/voximplant/messaging/messenger#leaveconversation) method.

```vox.multicode
env: media
title:Remove a conversation
description:
====
highlight : 
link : 
name : 
lang : javascript
----
// First remove all the participants

public removeParticipants(currentConversation: Conversation, userIds: number[]) {
    return currentConversation.removeParticipants(userIds.map((userId) => ({ userId }))).catch(logError);
}

// Then leave the empty conversation to delete it

public leaveConversation(currentConversationUuid: string) {
    MessengerService.messenger.leaveConversation(currentConversationUuid)
        .catch(logError);
}

```
```vox.multicode
env: media
title:Remove a conversation
description:
====
highlight : 
link : 
name : 
lang : swift
----
// First remove all the participants

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

// Then leave the empty conversation to delete it

func leaveConversation(
    with UUID: String,
    completion: @escaping (Result<VIConversationEvent, Error>) -> Void
) {
    messenger.leaveConversation(UUID, completion:
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
title:Remove a conversation
description:
====
highlight : 
link : 
name : 
lang : kotlin
----
// First remove all the participants

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

// Then leave the empty conversation to delete it

fun leaveConversation(
    uuid: String,
    completion: (Result<IConversationEvent>) -> Unit
) {
    messenger.leaveConversation(
        uuid,
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
After that the conversation is deleted.
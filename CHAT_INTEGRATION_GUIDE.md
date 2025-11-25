# Real-Time Chat Integration Guide

## Overview

This document provides instructions for integrating the real-time chat feature into the SwapMySkill platform. The chat system enables 1:1 messaging between users with features like real-time messaging, read receipts, typing indicators, message history, and document sharing.

## Features Implemented

1. **Real-time Messaging**: Instant message delivery between users
2. **Deterministic Room Creation**: Automatically creates or opens existing chat rooms using UID-based IDs
3. **Message Status Tracking**: Sent, delivered, and read status indicators
4. **Typing Indicators**: Shows when the other user is typing
5. **Message History**: Loads last 50 messages with pagination for older messages
6. **Message Deletion**: Users can delete their own messages
7. **Document Sharing**: Users can share PDF, DOCX, TXT, XML, and other document formats
8. **Security**: Firebase Realtime Database and Storage rules ensure only participants can access chat data

## File Structure

```
src/
├── app/
│   ├── chat/
│   │   ├── page.tsx          # Chat page server component
│   │   └── chat-client.tsx   # Chat UI and logic (client component)
├── lib/
│   └── firebase.ts           # Firebase configuration
FIREBASE_CHAT_RULES.json      # Firebase Realtime Database security rules
FIREBASE_STORAGE_RULES.json   # Firebase Storage security rules
```

## Integration Points

### 1. Dashboard Integration

The chat feature is integrated into the dashboard through clickable user cards:

- Each user card in the dashboard now has:
  - A click handler on the entire card that navigates to `/chat?userId=[UID]`
  - A dedicated chat button that also navigates to the chat
- Both navigation methods create or open an existing chat room with the selected user

### 2. Chat Page

The chat page (`/chat`) accepts a `userId` parameter to initiate a conversation:

- Automatically creates a deterministic room ID using both user UIDs
- Loads message history with pagination (50 most recent messages initially)
- Implements real-time listeners for new messages, typing indicators, and read receipts
- Supports document sharing with real-time upload progress

## Database Schema

### Rooms Structure
```
/rooms/{roomId}
  participants: {
    "<uid1>": true,
    "<uid2>": true
  }
  lastMessage: {
    text: string,
    senderId: string,
    createdAt: timestamp
  }
  createdAt: timestamp
  updatedAt: timestamp
```

### Messages Structure
```
/messages/{roomId}/{messageId}
  senderId: string
  receiverId: string
  text: string
  createdAt: timestamp
  status: "sent" | "delivered" | "read"
  deleted: boolean (optional)
  // For media/documents:
  mediaType: "image" | "video" | "gif" | "sticker" | "file" (optional)
  mediaUrl: string (optional)
  mediaThumbnailUrl: string (optional)
  mediaName: string (optional)
  mediaSize: number (optional)
  mediaWidth: number (optional)
  mediaHeight: number (optional)
```

### Typing Indicators Structure
```
/typing/{roomId}/{userId}: boolean
```

## Firebase Security Rules

The security rules ensure that only authenticated participants can access chat data:

```json
{
  "rules": {
    "rooms": {
      "$roomId": {
        ".read": "auth != null && (data.child('participants').hasChild(auth.uid) || newData.child('participants').hasChild(auth.uid))",
        ".write": "auth != null && (data.child('participants').hasChild(auth.uid) || newData.child('participants').hasChild(auth.uid))",
        ".validate": "newData.hasChildren(['participants', 'createdAt', 'updatedAt', 'lastMessage'])"
      }
    },
    "messages": {
      "$roomId": {
        ".read": "auth != null && root.child('rooms').child($roomId).child('participants').hasChild(auth.uid)",
        ".write": "auth != null && root.child('rooms').child($roomId).child('participants').hasChild(auth.uid)",
        ".indexOn": ["createdAt"],
        "$messageId": {
          ".validate": "newData.hasChildren(['senderId', 'receiverId', 'text', 'createdAt', 'status']) || newData.hasChildren(['senderId', 'receiverId', 'text', 'createdAt', 'status', 'mediaType', 'mediaUrl']) || newData.hasChildren(['senderId', 'receiverId', 'text', 'createdAt', 'status', 'mediaType', 'mediaUrl', 'mediaName', 'mediaSize'])"
        }
      }
    },
    "typing": {
      "$roomId": {
        ".read": "auth != null && root.child('rooms').child($roomId).child('participants').hasChild(auth.uid)",
        ".write": "auth != null && root.child('rooms').child($roomId).child('participants').hasChild(auth.uid)"
      }
    }
  }
}
```

## Firebase Storage Rules

Document sharing uses Firebase Storage with the following security rules:

```json
{
  "rules": {
    "chat-documents": {
      "$roomId": {
        ".read": "auth != null && root.child('rooms').child($roomId).child('participants').hasChild(auth.uid)",
        ".write": "auth != null && root.child('rooms').child($roomId).child('participants').hasChild(auth.uid)",
        "$documentId": {
          ".validate": "newData.hasChildren(['name', 'size', 'contentType', 'uploadedAt'])
        }
      }
    }
  }
}
```

## How to Deploy Rules

### Realtime Database Rules
1. Go to the Firebase Console
2. Navigate to Realtime Database > Rules
3. Replace the existing rules with the content from `FIREBASE_CHAT_RULES.json`
4. Publish the rules

### Storage Rules
1. Go to the Firebase Console
2. Navigate to Storage > Rules
3. Replace the existing rules with the content from `FIREBASE_STORAGE_RULES.json`
4. Publish the rules

## Code Examples

### Creating/Openning a Chat Room
The chat room is automatically created or opened when navigating to `/chat?userId=[UID]`. The deterministic room ID is generated as:
```javascript
const createRoomId = (uid1: string, uid2: string) => {
  return uid1 < uid2 ? `${uid1}_${uid2}` : `${uid2}_${uid1}`;
};
```

### Sending a Message
```javascript
const messageData = {
  roomId,
  senderId: user.uid,
  receiverId: otherUserId,
  text: newMessage.trim(),
  createdAt: Date.now(),
  status: "sent"
};

const newMessageRef = push(ref(database, `messages/${roomId}`));
await set(newMessageRef, {
  ...messageData,
  messageId: newMessageRef.key
});
```

### Sending a Document
```javascript
// Upload document to Firebase Storage
const fileRef = storageRef(storage, `chat-documents/${roomId}/${Date.now()}_${file.name}`);
const uploadTask = uploadBytesResumable(fileRef, file);

// Get download URL after upload
const downloadURL = await getDownloadURL(uploadTask.snapshot.ref);

// Send message with document URL
const messageData = {
  roomId,
  senderId: user.uid,
  receiverId: otherUserId,
  text: caption || file.name,
  createdAt: Date.now(),
  status: "sent",
  mediaType: "file",
  mediaUrl: downloadURL,
  mediaName: file.name,
  mediaSize: file.size
};
```

### Listening for Messages
```javascript
const messagesRef = query(
  ref(database, `messages/${roomId}`),
  orderByChild("createdAt"),
  limitToLast(50)
);

const unsubscribe = onValue(messagesRef, (snapshot) => {
  // Handle incoming messages
});
```

### Typing Indicators
```javascript
// Set typing indicator
const typingRef = ref(database, `typing/${roomId}/${user.uid}`);
set(typingRef, true);

// Clear typing indicator after 3 seconds
setTimeout(() => {
  set(typingRef, false);
}, 3000);
```

## Testing Instructions

### Functional Tests

1. **Room Creation Test**
   - Log in as User A
   - Navigate to dashboard
   - Click on User B's profile card
   - Verify that a new chat room is created with deterministic ID

2. **Message Exchange Test**
   - In the chat interface, send a message from User A to User B
   - Verify that User B receives the message in real-time
   - Verify that message status updates from "sent" to "delivered" to "read"

3. **Document Sharing Test**
   - In the chat interface, attach a document file (PDF, DOCX, etc.)
   - Verify that the file uploads to Firebase Storage
   - Verify that a message with document link appears in the chat
   - Verify that the recipient can download the document

4. **Security Test**
   - Log in as User C (not a participant)
   - Attempt to access `/messages/room_A_B`
   - Verify that access is denied
   - Attempt to access documents in Storage
   - Verify that access is denied

5. **Pagination Test**
   - Create more than 50 messages in a chat
   - Verify that only 50 most recent messages are loaded initially
   - Scroll to top and verify older messages load

6. **Message Deletion Test**
   - Send a message
   - Delete the message
   - Verify that the message is marked as deleted and shows "This message was deleted"

## Performance Considerations

1. **Pagination**: Only loads 50 most recent messages initially, with on-demand loading for older messages
2. **Typing Debouncing**: Typing indicators automatically clear after 3 seconds of inactivity
3. **Efficient Listeners**: Uses Firebase listeners for real-time updates rather than polling
4. **Message Validation**: Client-side validation prevents malformed data from being sent
5. **File Size Limits**: Documents are limited to 10MB to prevent excessive storage usage

## Customization Options

1. **Styling**: The chat UI uses Tailwind CSS and can be customized by modifying the classes in `chat-client.tsx`
2. **Message Limit**: Adjust the `limitToLast()` parameter to change initial message load count
3. **Typing Timeout**: Modify the timeout duration in the typing indicator implementation
4. **Message Validation**: Add additional validation rules in the Firebase security rules
5. **File Types**: Modify the accepted file types in the file input and validation logic
6. **File Size Limit**: Adjust the maxFileSize constant to change the upload limit

## Troubleshooting

### Common Issues

1. **Messages Not Appearing**
   - Check Firebase security rules
   - Verify that both users are authenticated
   - Confirm that the room was created correctly

2. **Typing Indicators Not Working**
   - Ensure both users are in the same room
   - Check network connectivity
   - Verify that the typing path in the database is correct

3. **Pagination Issues**
   - Confirm that messages are properly ordered by timestamp
   - Check that the `endAt()` query is correctly implemented

4. **Document Upload Issues**
   - Check Firebase Storage rules
   - Verify that the user has proper authentication
   - Check file size limits
   - Confirm that the file type is supported

### Debugging Tips

1. Use Firebase Console to inspect real-time database and storage values
2. Check browser console for JavaScript errors
3. Verify that all required Firebase permissions are enabled
4. Test with different network conditions to ensure robustness
5. Use browser developer tools to monitor network requests for file uploads
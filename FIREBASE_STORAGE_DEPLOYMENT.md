# Firebase Storage Rules Deployment Guide

This document explains how to deploy the Firebase Storage rules for the document sharing feature in the chat system.

## Prerequisites

1. Firebase CLI installed
2. Logged into Firebase account with appropriate permissions
3. Firebase project initialized

## Deployment Steps

1. Go to the Firebase Console
2. Navigate to Storage > Rules
3. Replace the existing rules with the content from `FIREBASE_STORAGE_RULES.json`
4. Publish the rules

## Rules Explanation

The storage rules ensure that:
- Only authenticated users who are participants in a chat room can access documents in that room's folder
- Files are validated to have proper metadata (name, size, contentType, uploadedAt)
- Access is restricted by room ID to prevent unauthorized access to documents

## Security Notes

- Documents are stored in a path structure: `chat-documents/{roomId}/{timestamp}_{filename}`
- Access is controlled through the same room participant validation used in the Realtime Database
- All document uploads are secure and only accessible to chat participants
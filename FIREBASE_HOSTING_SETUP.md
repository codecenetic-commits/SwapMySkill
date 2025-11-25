# Firebase Hosting Setup for SwapMySkill

This document outlines the Firebase Hosting configuration for the SwapMySkill website.

## Project Details
- **Project Name**: SwapMySkill
- **Firebase Project ID**: swapmyskill-d25e2
- **Hosting Account**: codecenetic@gmail.com

## Deployment Instructions

1. Make sure you have the Firebase CLI installed:
   ```
   npm install -g firebase-tools
   ```

2. Login to Firebase with the designated account:
   ```
   firebase login
   ```
   (Use codecenetic@gmail.com when prompted)

3. Deploy to Firebase Hosting:
   ```
   npm run firebase-deploy
   ```

## Configuration Details

The Firebase hosting configuration is defined in `firebase.json`:

- Public directory: `out` (Next.js export directory)
- Clean URLs: Enabled (rewrites all requests to /index.html)
- Asset caching: Enabled for static assets
- Cache prevention: Enabled for HTML files

## Firebase Services Used

- Firebase Authentication
- Firebase Realtime Database
- Firebase Storage
- Firebase Hosting

All services are already configured in `src/lib/firebase.ts`.
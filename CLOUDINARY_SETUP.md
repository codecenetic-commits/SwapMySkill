# Cloudinary Setup Instructions

This document provides instructions for properly configuring Cloudinary for the SwapMySkill application.

## Upload Preset Configuration

The application uses the `Swap_My_Skill` upload preset for image uploads. To ensure proper functionality, this preset must be configured as an **unsigned** upload preset in your Cloudinary account.

### Steps to Configure the Upload Preset:

1. Log in to your Cloudinary account at [https://cloudinary.com](https://cloudinary.com)
2. Navigate to **Settings** → **Upload**
3. Find the `Swap_My_Skill` upload preset in the list (or create it if it doesn't exist)
4. Click **Edit** next to the preset
5. Set the **Signing Mode** to **Unsigned**
6. Configure the following settings:
   - **Mode**: Unsigned
   - **Overwrite**: false
   - **Use filename**: false
   - **Unique filename**: true
   - **Use filename as display name**: true
   - **Use asset folder as public ID prefix**: false
   - **Type**: upload
   - **Folder**: `profile-images`
7. Save the changes

### Required Settings for Unsigned Upload Preset:

- **Mode**: Unsigned
- **Overwrite**: false
- **Use filename**: false
- **Unique filename**: true
- **Use filename as display name**: true
- **Use asset folder as public ID prefix**: false
- **Type**: upload
- **Folder**: `profile-images`
- **Allowed file types**: images (jpg, png, gif, etc.)
- **Max file size**: As appropriate for your needs (recommended: 10MB)
- **Responsive breakpoints**: Optional

## Troubleshooting

### Common Issues:

1. **"Upload preset must be whitelisted for unsigned uploads" error**:
   - Solution: Ensure the upload preset is set to "Unsigned" mode in Cloudinary settings

2. **Generic "Error uploading image" message**:
   - Check browser console for detailed error information
   - Verify Cloudinary account credentials
   - Confirm upload preset configuration

3. **Widget not loading**:
   - Check network tab for script loading errors
   - Verify Cloudinary script URL is accessible
   - Ensure no ad blockers are interfering

### Error Handling:

The application includes enhanced error handling that provides detailed logging for Cloudinary upload errors. Check the browser console for specific error messages that can help diagnose issues.

## Security Considerations:

- Never expose your Cloudinary API secret in client-side code
- Use unsigned upload presets for client-side uploads
- Restrict allowed file types and sizes in the upload preset
- Consider enabling moderation for user-uploaded content
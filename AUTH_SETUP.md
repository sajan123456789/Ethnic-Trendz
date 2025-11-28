# Authentication Setup Guide

## What's Been Done
✓ Database table created: `users_profile`
✓ Auth trigger created to automatically create user profiles
✓ Login form and signup form integrated
✓ Google OAuth configured in code
✓ All code issues fixed and build successful

## Next Steps - REQUIRED IN SUPABASE DASHBOARD

### 1. Enable Email Authentication
1. Go to Supabase Dashboard → Authentication → Providers
2. Find "Email" provider
3. Make sure it's enabled and configured

### 2. Configure Google OAuth (Required for Google Sign-In)
1. Go to Supabase Dashboard → Authentication → Providers → Google
2. Click "Enable Google provider"
3. You need Google OAuth credentials:
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project (if you don't have one)
   - Enable Google+ API
   - Create OAuth 2.0 credentials (OAuth consent screen → Web application)
   - Add authorized redirect URIs:
     - `https://bhftpwgxuizutmxnjzcy.supabase.co/auth/v1/callback`
     - `http://localhost:3000/auth/callback` (for local testing)
   - Copy Client ID and Client Secret
4. Paste these into Supabase Google provider settings
5. Save

### 3. Configure Redirect URLs
1. Go to Supabase Dashboard → Authentication → URL Configuration
2. Add these URLs:
   - Redirect URL: `http://localhost:3000/auth/callback` (for local testing)
   - Redirect URL: `https://yourdomain.com/auth/callback` (for production)

### 4. Test Email/Password Auth
1. Start the dev server: `npm run dev`
2. Go to Sign Up page
3. Create account with email and password
4. Should auto-login after signup
5. Check Supabase Dashboard → Authentication → Users to see the new user

### 5. Test Google Sign-In
1. Click "Sign in with Google" button
2. Should redirect to Google login
3. After login, should redirect back and auto-login

## Files Modified
- `/src/context/AuthContext.tsx` - Fixed async/await handling, added error handling
- `/src/components/LoginPage.tsx` - Already configured
- `/src/components/SignupPage.tsx` - Already configured
- Database created with RLS policies enabled

## Environment Variables
Your `.env` file has been updated with correct Supabase credentials:
```
VITE_SUPABASE_URL=https://bhftpwgxuizutmxnjzcy.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## Common Issues & Solutions

### "Invalid API Key" Error
- Make sure `.env` file has correct VITE_SUPABASE_ANON_KEY
- Verify the key hasn't expired in Supabase dashboard
- The build includes the correct key from `.env`

### Google Sign-In Not Working
- Make sure Google OAuth is enabled in Supabase
- Verify OAuth credentials are correct
- Check redirect URL is configured in both Supabase AND Google Cloud Console
- Ensure `http://localhost:3000/auth/callback` is added to Google authorized redirect URIs

### User Not Staying Logged In
- Check browser cookies are allowed
- Verify RLS policies on `users_profile` table are correct
- The trigger should auto-create profile, but may need a few seconds to sync

## Testing Checklist
- [ ] Email/Password signup works
- [ ] Email/Password login works
- [ ] Google Sign-In works
- [ ] User stays logged in after refresh
- [ ] Logout works
- [ ] Admin emails (contact@ethnictrendz.top, pp8900852@gmail.com) have admin role
- [ ] Other users have customer role

## Database Schema
```
users_profile table:
- id (uuid) - refs auth.users
- email (text)
- role (text) - 'admin' or 'customer'
- created_at (timestamp)
- updated_at (timestamp)
```

Auth trigger automatically creates entry in users_profile when new user signs up.

# Supabase Storage Setup Guide for Image Upload

## Problem
The image upload feature requires a storage bucket named "products" in Supabase, which may not exist yet.

## Solution Steps

### 1. Create the Storage Bucket

1. Go to your Supabase Dashboard: https://supabase.com/dashboard
2. Select your project
3. Navigate to **Storage** in the left sidebar
4. Click **"New bucket"**
5. Enter the following details:
   - **Name**: `products`
   - **Public bucket**: ✅ **Check this box** (to allow public read access)
   - **File size limit**: Leave default or set to 5MB
   - **Allowed MIME types**: Leave empty or add: `image/jpeg, image/png, image/webp, image/gif`
6. Click **"Create bucket"**

### 2. Configure Storage RLS Policies

After creating the bucket, you need to set up Row Level Security (RLS) policies:

1. In the Supabase Dashboard, go to **Storage** > **Policies**
2. Select the `products` bucket
3. Add the following policies:

#### Policy 1: Public Read Access
- **Policy name**: `Public read access for product images`
- **Allowed operation**: `SELECT`
- **Target roles**: `public`
- **Policy definition**:
  ```sql
  bucket_id = 'products'
  ```

#### Policy 2: Authenticated Upload
- **Policy name**: `Allow authenticated users to upload`
- **Allowed operation**: `INSERT`
- **Target roles**: `authenticated`
- **Policy definition**:
  ```sql
  bucket_id = 'products'
  ```

#### Policy 3: Authenticated Update
- **Policy name**: `Allow authenticated users to update`
- **Allowed operation**: `UPDATE`
- **Target roles**: `authenticated`
- **Policy definition**:
  ```sql
  bucket_id = 'products'
  ```

#### Policy 4: Authenticated Delete
- **Policy name**: `Allow authenticated users to delete`
- **Allowed operation**: `DELETE`
- **Target roles**: `authenticated`
- **Policy definition**:
  ```sql
  bucket_id = 'products'
  ```

### 3. Alternative: SQL Script Method

If you prefer to use SQL, run this in the **SQL Editor**:

```sql
-- Note: Bucket creation must be done via Dashboard or API, not SQL
-- After creating the bucket via Dashboard, run these policies:

-- Allow public read access
CREATE POLICY "Public read access for product images"
ON storage.objects
FOR SELECT
TO public
USING (bucket_id = 'products');

-- Allow authenticated uploads
CREATE POLICY "Allow authenticated uploads to products bucket"
ON storage.objects
FOR INSERT
TO authenticated
WITH CHECK (bucket_id = 'products');

-- Allow authenticated updates
CREATE POLICY "Allow authenticated updates to products bucket"
ON storage.objects
FOR UPDATE
TO authenticated
USING (bucket_id = 'products');

-- Allow authenticated deletes
CREATE POLICY "Allow authenticated deletes from products bucket"
ON storage.objects
FOR DELETE
TO authenticated
USING (bucket_id = 'products');
```

### 4. Verify Setup

To verify everything is working:

1. Try uploading an image through the admin form
2. Check the browser console for any errors
3. If you see "Bucket not found" error, make sure you created the bucket with the exact name `products`
4. If you see "Permission denied" error, check that the RLS policies are correctly set up

## Common Issues

### Issue: "Bucket not found"
**Solution**: Create the `products` bucket in Supabase Dashboard > Storage

### Issue: "new row violates row-level security policy"
**Solution**: 
- Make sure you're logged in as an authenticated user
- Verify the RLS policies are set up correctly
- Check that the policies target the correct roles (`authenticated` for uploads, `public` for reads)

### Issue: "Failed to upload image"
**Solution**:
- Check your internet connection
- Verify Supabase credentials in `.env` file
- Check browser console for detailed error messages
- Ensure the file size is within limits (default 50MB, but can be configured)

## Testing

After setup, test the upload:

1. Go to `/admin/products/new`
2. Fill in product details
3. Select "Upload" for image source
4. Choose an image file (JPEG, PNG, WebP, or GIF)
5. You should see "✓ Selected: filename.jpg" below the file input
6. Click "Save Product"
7. The image should upload and the product should be created

If successful, you'll be redirected to the products list and your new product will appear with the uploaded image.

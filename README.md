# Ethnic Trendz

A modern e-commerce platform for traditional and ethnic wear, created by pranshu.

## Environment Setup

Before running the application, you need to set up your environment variables. Create a `.env` file in the root directory with the following variables:

```env
VITE_SUPABASE_URL=your-supabase-project-url
VITE_SUPABASE_ANON_KEY=your-supabase-anon-key
```

Replace `your-supabase-project-url` and `your-supabase-anon-key` with your actual Supabase project credentials.

## Development

1. Install dependencies:
```bash
npm install
```

2. Start the development server:
```bash
npm run dev
```

## Building for Production

To create a production build:
```bash
npm run build
```

## Project Structure

- `/src` - Source code
  - `/components` - React components
  - `/context` - React context providers
  - `/hooks` - Custom React hooks
  - `/services` - API services
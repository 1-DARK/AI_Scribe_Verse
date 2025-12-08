# Quick Start Deployment Guide

## Overview
Your app has 3 services that need to be deployed on Render:
1. Frontend (Static Site)
2. Numerical API (Python Web Service)
3. Categorical API (Python Web Service)

## Option 1: Deploy Using render.yaml (Recommended)

1. **Push your code to GitHub/GitLab/Bitbucket**

2. **Go to Render Dashboard** → Click **"New +"** → **"Blueprint"**

3. **Connect your repository** and Render will automatically create all 3 services from `render.yaml`

4. **Configure Environment Variables** for each service:

   **Frontend:**
   - `VITE_SUPABASE_URL` = Your Supabase URL
   - `VITE_SUPABASE_PUBLISHABLE_KEY` = Your Supabase key
   - `VITE_NUMERICAL_API_URL` = https://your-numerical-api.onrender.com
   - `VITE_CATEGORICAL_API_URL` = https://your-categorical-api.onrender.com

   **Both Backend Services:**
   - `ALLOWED_ORIGINS` = https://your-frontend.onrender.com (optional, for production CORS)

5. **Wait for deployment** (takes 5-10 minutes)

## Option 2: Manual Deployment

### Step 1: Deploy Numerical API
- Type: Web Service
- Build: `pip install -r requirements_numerical.txt`
- Start: `uvicorn AutoInsight_numerical:app --host 0.0.0.0 --port $PORT`
- Note the URL (e.g., `https://ai-scribe-verse-numerical-api.onrender.com`)

### Step 2: Deploy Categorical API
- Type: Web Service
- Build: `pip install -r requirements_categorical.txt`
- Start: `uvicorn AutoInsight_categorical:app --host 0.0.0.0 --port $PORT`
- Note the URL (e.g., `https://ai-scribe-verse-categorical-api.onrender.com`)

### Step 3: Deploy Frontend
- Type: Static Site
- Build: `npm install && npm run build`
- Publish: `dist`
- Add environment variables (see Option 1)

## Important Notes

⚠️ **Free Tier Limitations:**
- Services spin down after 15 minutes of inactivity
- First request after spin-down may take 30-60 seconds
- Consider Starter plan ($7/month) for always-on services

🔒 **CORS Configuration:**
- By default, backends allow all origins (`*`)
- For production, set `ALLOWED_ORIGINS` environment variable in backend services

📝 **After Deployment:**
1. Update frontend env vars with actual API URLs
2. Test file upload functionality
3. Verify Supabase connection

## Troubleshooting

- **Build fails?** Check logs for missing dependencies
- **CORS errors?** Set `ALLOWED_ORIGINS` environment variable
- **API timeout?** Services may be spinning up (first request is slow)

For detailed instructions, see [DEPLOYMENT.md](./DEPLOYMENT.md)


# Deployment Guide for Render

This guide will help you deploy AI Scribe Verse on Render.

## Architecture

Your application consists of 3 services:
1. **Frontend** - React/Vite static site
2. **Numerical API** - FastAPI backend for numerical analysis
3. **Categorical API** - FastAPI backend for categorical analysis

## Prerequisites

1. A [Render](https://render.com) account
2. A [Supabase](https://supabase.com) project (already set up)
3. Your code pushed to a Git repository (GitHub, GitLab, or Bitbucket)

## Step 1: Prepare Your Repository

Ensure all files are committed and pushed to your Git repository:
- `render.yaml` (deployment configuration)
- `requirements_numerical.txt` (Python dependencies for numerical API)
- `requirements_categorical.txt` (Python dependencies for categorical API)
- `static.json` (frontend configuration)
- Updated `.env.example` file

## Step 2: Deploy Backend Services

### Deploy Numerical API

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click **"New +"** → **"Web Service"**
3. Connect your repository
4. Configure the service:
   - **Name**: `ai-scribe-verse-numerical-api`
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements_numerical.txt`
   - **Start Command**: `uvicorn AutoInsight_numerical:app --host 0.0.0.0 --port $PORT`
   - **Plan**: Starter (or Free for testing)
5. Click **"Create Web Service"**
6. Note the service URL (e.g., `https://ai-scribe-verse-numerical-api.onrender.com`)

### Deploy Categorical API

1. Click **"New +"** → **"Web Service"**
2. Connect the same repository
3. Configure the service:
   - **Name**: `ai-scribe-verse-categorical-api`
   - **Environment**: `Python 3`
   - **Build Command**: `pip install -r requirements_categorical.txt`
   - **Start Command**: `uvicorn AutoInsight_categorical:app --host 0.0.0.0 --port $PORT`
   - **Plan**: Starter (or Free for testing)
4. Click **"Create Web Service"**
5. Note the service URL (e.g., `https://ai-scribe-verse-categorical-api.onrender.com`)

### Alternative: Deploy Using render.yaml

If you prefer using the `render.yaml` file:

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Click **"New +"** → **"Blueprint"**
3. Connect your repository
4. Render will automatically detect and deploy all services from `render.yaml`

## Step 3: Deploy Frontend

1. Click **"New +"** → **"Static Site"**
2. Connect your repository
3. Configure the site:
   - **Name**: `ai-scribe-verse-frontend`
   - **Build Command**: `npm install && npm run build`
   - **Publish Directory**: `dist`
4. Add Environment Variables:
   ```
   VITE_SUPABASE_URL=https://your-project.supabase.co
   VITE_SUPABASE_PUBLISHABLE_KEY=your_publishable_key
   VITE_NUMERICAL_API_URL=https://ai-scribe-verse-numerical-api.onrender.com
   VITE_CATEGORICAL_API_URL=https://ai-scribe-verse-categorical-api.onrender.com
   ```
5. Click **"Create Static Site"**
6. Note the site URL (e.g., `https://ai-scribe-verse-frontend.onrender.com`)

## Step 4: Configure CORS (Important!)

The backend services need to allow requests from your frontend domain.

### For Numerical API:
1. Go to your Numerical API service on Render
2. Add an environment variable:
   - **Key**: `ALLOWED_ORIGINS`
   - **Value**: `https://ai-scribe-verse-frontend.onrender.com` (your frontend URL)

Update `AutoInsight_numerical.py` to use this:
```python
import os

allowed_origins = os.environ.get("ALLOWED_ORIGINS", "*").split(",")

app.add_middleware(
    CORSMiddleware,
    allow_origins=allowed_origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### For Categorical API:
Do the same for `AutoInsight_categorical.py`

## Step 5: Update Frontend Environment Variables

After deployment, update your frontend environment variables on Render:
1. Go to your Frontend static site
2. Navigate to **Environment** tab
3. Update:
   - `VITE_NUMERICAL_API_URL` = Your Numerical API URL
   - `VITE_CATEGORICAL_API_URL` = Your Categorical API URL

## Step 6: Enable Auto-Deploy

By default, Render will auto-deploy when you push to your main branch. This is usually enabled automatically.

## Troubleshooting

### Backend Services Not Starting
- Check the build logs for dependency installation errors
- Verify Python version compatibility (Python 3.11 recommended)
- Check if all required files are in the repository

### CORS Errors
- Ensure `ALLOWED_ORIGINS` includes your frontend URL
- Verify CORS middleware is properly configured
- Check browser console for specific CORS error messages

### Frontend Not Loading
- Verify environment variables are set correctly
- Check that `dist` folder is being generated during build
- Review build logs for compilation errors

### API Timeout Issues
- Render free tier services spin down after 15 minutes of inactivity
- Consider upgrading to a paid plan for always-on services
- First request after spin-down may take 30-60 seconds

## Cost Considerations

- **Free Tier**: 
  - Services spin down after inactivity
  - Build time limits
  - Suitable for development/testing
  
- **Starter Plan** ($7/month per service):
  - Always-on services
  - Better performance
  - Recommended for production

## Security Notes

1. Never commit `.env` files with real credentials
2. Use Render's environment variables for sensitive data
3. Enable HTTPS (enabled by default on Render)
4. Consider adding rate limiting for production

## Post-Deployment Checklist

- [ ] All three services deployed successfully
- [ ] Environment variables configured
- [ ] CORS properly configured
- [ ] Frontend can communicate with both APIs
- [ ] Test file upload functionality
- [ ] Verify Supabase connection works
- [ ] Check logs for any errors

## Updating Your Deployment

To update your application:
1. Push changes to your Git repository
2. Render will automatically detect and deploy changes
3. Monitor the deployment logs for any issues

## Support

For Render-specific issues, check:
- [Render Documentation](https://render.com/docs)
- [Render Status Page](https://status.render.com)
- [Render Community](https://community.render.com)


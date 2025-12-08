# Railway Deployment Guide

## Overview
Deploy your AutoInsight app on Railway with 3 services:
1. **Frontend** - React/Vite static site
2. **Numerical API** - Python FastAPI backend
3. **Categorical API** - Python FastAPI backend

---

## Step-by-Step Deployment

### Step 1: Create Railway Account
1. Go to [railway.app](https://railway.app)
2. Sign up with GitHub (recommended)

### Step 2: Create New Project
1. Click **"New Project"**
2. Select **"Empty Project"**

### Step 3: Deploy Frontend Service
1. Click **"+ New"** → **"GitHub Repo"**
2. Select your repository
3. Railway will auto-detect the frontend
4. Go to **Settings** → **Variables** and add:
   ```
   VITE_SUPABASE_URL=https://bikkpzcghpcokucxemen.supabase.co
   VITE_SUPABASE_PUBLISHABLE_KEY=your_key_here
   VITE_NUMERICAL_API_URL=https://your-numerical-api.up.railway.app
   VITE_CATEGORICAL_API_URL=https://your-categorical-api.up.railway.app
   ```
5. Go to **Settings** → **Build**:
   - Build Command: `npm install && npm run build`
   - Start Command: `npx serve dist -s -l $PORT`

### Step 4: Deploy Numerical API Service
1. Click **"+ New"** → **"GitHub Repo"**
2. Select the same repository
3. Go to **Settings** → **General**:
   - Root Directory: `/` (or leave empty)
4. Go to **Settings** → **Build**:
   - Build Command: `pip install -r requirements_numerical.txt`
   - Start Command: `uvicorn AutoInsight_numerical:app --host 0.0.0.0 --port $PORT`
5. Add environment variable:
   ```
   PYTHON_VERSION=3.11
   ```

### Step 5: Deploy Categorical API Service
1. Click **"+ New"** → **"GitHub Repo"**
2. Select the same repository
3. Go to **Settings** → **Build**:
   - Build Command: `pip install -r requirements_categorical.txt`
   - Start Command: `uvicorn AutoInsight_categorical:app --host 0.0.0.0 --port $PORT`
4. Add environment variable:
   ```
   PYTHON_VERSION=3.11
   ```

### Step 6: Generate Domains
1. For each service, go to **Settings** → **Networking**
2. Click **"Generate Domain"**
3. Note down the URLs for each service

### Step 7: Update Frontend Environment Variables
1. Go back to your Frontend service
2. Update `VITE_NUMERICAL_API_URL` and `VITE_CATEGORICAL_API_URL` with actual Railway URLs
3. Redeploy the frontend

---

## Environment Variables Summary

### Frontend Service
| Variable | Value |
|----------|-------|
| `VITE_SUPABASE_URL` | `https://bikkpzcghpcokucxemen.supabase.co` |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | Your Supabase anon key |
| `VITE_NUMERICAL_API_URL` | `https://[numerical-service].up.railway.app` |
| `VITE_CATEGORICAL_API_URL` | `https://[categorical-service].up.railway.app` |

### Backend Services (Both)
| Variable | Value |
|----------|-------|
| `PYTHON_VERSION` | `3.11` |
| `ALLOWED_ORIGINS` | `https://[frontend].up.railway.app` (optional) |

---

## Railway vs Render Comparison

| Feature | Railway | Render |
|---------|---------|--------|
| Cold starts | No (paid) | Yes (free tier) |
| Free tier | $5 credit/month | Limited hours |
| Setup | Manual per service | Blueprint auto-setup |
| Pricing | Usage-based | Fixed plans |

---

## Troubleshooting

### Build Fails
- Check logs in Railway dashboard
- Verify Python version is set to 3.11
- Ensure all dependencies are in requirements files

### CORS Errors
- Set `ALLOWED_ORIGINS` environment variable in backend services
- Value should be your frontend Railway URL

### API Not Responding
- Check if service is deployed (green status)
- Verify the domain is generated
- Check service logs for errors

---

## Useful Commands

```bash
# Install Railway CLI (optional)
npm install -g @railway/cli

# Login to Railway
railway login

# Link to project
railway link

# Deploy from CLI
railway up
```

---

## Cost Estimation

Railway uses usage-based pricing:
- **Starter Plan**: $5/month credit (covers small apps)
- **Pro Plan**: $20/month + usage

For a small app like this, the $5 credit typically covers:
- ~500 hours of backend runtime
- ~10GB bandwidth
- Unlimited frontend hosting

---

For more help, visit [Railway Docs](https://docs.railway.app)

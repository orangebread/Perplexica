# Digital Ocean App Platform Deployment Guide

This repository is configured for secure deployment to DigitalOcean App Platform with proper API key management.

## 🔒 Security First Approach

**NEVER commit API keys to the repository.** This setup uses environment variables for all sensitive data.

## Prerequisites

1. DigitalOcean account
2. OpenAI API key (get from: https://platform.openai.com/api-keys)
3. This GitHub repository (orangebread/Perplexica)

## Deployment Steps

### 1. Deploy to DigitalOcean App Platform

1. Go to https://cloud.digitalocean.com/apps
2. Click "Create App"
3. Choose "GitHub" as source
4. Select the `orangebread/Perplexica` repository
5. Choose the `main` branch (or `master` if that's your default)
6. **Important**: Check "Autodeploy" for automatic updates

### 2. Configure Environment Variables (CRITICAL)

**The app WILL NOT work without proper API keys set as environment variables.**

1. After creating your app, go to the App Platform console
2. Click on your app → Settings → App-Level Environment Variables
3. Add each API key as a "SECRET" type environment variable:

**Required:**
- `OPENAI_API_KEY`: Your OpenAI API key

**Optional (add only if you have these keys):**
- `GROQ_API_KEY`: For Groq models
- `ANTHROPIC_API_KEY`: For Claude models  
- `GEMINI_API_KEY`: For Gemini models
- `DEEPSEEK_API_KEY`: For DeepSeek models

4. Click "Save" and redeploy the app

### 3. Access Your Application

After deployment (5-10 minutes), you'll get a URL like:
`https://perplexica-app-xxxxx.ondigitalocean.app`

### 4. Configure Models in UI

1. Open your Perplexica application
2. Click the settings icon (⚙️) in the bottom left
3. Set your preferred models:
   - **Chat Model**: `gpt-3.5-turbo` (cost-effective) or `gpt-4` (higher quality)
   - **Embedding Model**: `text-embedding-ada-002`

## Cost Estimate

**DigitalOcean App Platform:**
- SearxNG Service: ~$5/month (basic-xxs)
- Main App Service: ~$5/month (basic-xs)
- **Total Infrastructure**: ~$10/month

**OpenAI API Usage:**
- Depends on usage, typically $10-50/month for moderate use
- Much cheaper than commercial Perplexity API ($44,000/month for enterprise)

## API Integration for Your Applications

Once deployed, you can integrate with the API:

```bash
curl -X POST https://your-app-url.ondigitalocean.app/api/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "Tesla sustainability initiatives 2024",
    "mode": "academic"
  }'
```

## Security Features

✅ **API keys stored as encrypted environment variables**
✅ **No sensitive data in version control**
✅ **Automatic SSL certificates from DigitalOcean**
✅ **Private networking between services**

## Troubleshooting

### App Won't Start / 500 Errors
- **Check environment variables**: Ensure `OPENAI_API_KEY` is set correctly
- **Check logs**: In DO console → Your App → Runtime Logs
- **Verify API key**: Test your OpenAI key at https://platform.openai.com/

### SearxNG Issues
- **Search not working**: Check if searxng service is running in DO console
- **No results**: SearxNG may take 1-2 minutes to fully initialize

### Model Configuration Issues
- **Models not appearing**: Ensure API keys are set and valid
- **"No API key" errors**: Double-check environment variable names match exactly

## Local Development

To run locally for testing:

```bash
# Set environment variables
export OPENAI_API_KEY="your-key-here"
export SEARXNG_API_URL="http://localhost:4000"

# Start with Docker Compose
docker-compose up --build
```

Access at: http://localhost:3000

## Architecture

This deployment includes:
- **SearxNG**: Privacy-focused metasearch engine (port 8080)
- **Perplexica App**: Main application with AI-powered search (port 3000)
- **Automatic SSL**: Provided by DigitalOcean
- **Auto-scaling**: Handles traffic spikes automatically
- **Encrypted secrets**: API keys stored securely

## Why App Platform vs Manual Server?

✅ **No server management** - fully managed
✅ **Automatic SSL certificates** - zero config HTTPS
✅ **Built-in monitoring** - logs and metrics included
✅ **Auto-scaling** - handles traffic spikes
✅ **$10/month vs $96/month** for manual droplet setup

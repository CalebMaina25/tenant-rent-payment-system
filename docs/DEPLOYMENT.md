# Deployment Guide

## Backend Deployment (Railway or Heroku)

### Using Railway (Recommended - Free tier available)

1. **Create Railway Account**
   - Go to [railway.app](https://railway.app)
   - Sign up with GitHub

2. **Connect Repository**
   - Click "New Project"
   - Select "GitHub Repo"
   - Connect your repository

3. **Configure Environment**
   - Go to Variables tab
   - Add all variables from `.env.example`:
     ```
     NODE_ENV=production
     PORT=5000
     DB_HOST=<railway-mysql-host>
     DB_USER=root
     DB_PASSWORD=<your-password>
     DB_NAME=rent_payment_db
     JWT_SECRET=<strong-secret-key>
     MPESA_API_KEY=<your-mpesa-key>
     ... (other variables)
     ```

4. **Add MySQL Database**
   - In Railway, add MySQL service
   - Copy connection details to backend environment

5. **Deploy**
   - Commit and push to GitHub
   - Railway auto-deploys on push

### Using Heroku

1. **Install Heroku CLI**
   ```bash
   npm install -g heroku
   ```

2. **Login to Heroku**
   ```bash
   heroku login
   ```

3. **Create App**
   ```bash
   heroku create your-app-name
   ```

4. **Add MySQL Database**
   ```bash
   heroku addons:create cleardb:ignite
   ```

5. **Set Environment Variables**
   ```bash
   heroku config:set NODE_ENV=production
   heroku config:set JWT_SECRET=your-secret-key
   # ... set all other variables
   ```

6. **Deploy**
   ```bash
   git push heroku main
   ```

### Procfile for Heroku

Create `Procfile` in backend root:
```
web: node server.js
```

## Frontend Deployment

### Using Vercel (Recommended)

1. **Push Code to GitHub**
   ```bash
   git push origin main
   ```

2. **Connect to Vercel**
   - Go to [vercel.com](https://vercel.com)
   - Sign up with GitHub
   - Import your repository
   - Select `frontend` as root directory

3. **Configure Environment**
   - Add environment variables in Vercel dashboard:
     ```
     REACT_APP_API_URL=https://your-backend.railway.app/api
     ```

4. **Deploy**
   - Vercel automatically deploys on push

### Using Netlify

1. **Build Frontend**
   ```bash
   cd frontend
   npm run build
   ```

2. **Deploy to Netlify**
   - Drag and drop `build` folder to Netlify
   - Or connect GitHub repository

3. **Set Environment Variables**
   - In Netlify: Settings → Build & Deploy → Environment
   - Add `REACT_APP_API_URL`

4. **Configure Redirects**
   - Create `netlify.toml` in frontend root:
   ```toml
   [[redirects]]
   from = "/*"
   to = "/index.html"
   status = 200
   ```

### Using AWS (S3 + CloudFront)

1. **Build Frontend**
   ```bash
   cd frontend
   npm run build
   ```

2. **Create S3 Bucket**
   - AWS Console → S3
   - Create new bucket
   - Enable static website hosting

3. **Upload Build Files**
   ```bash
   aws s3 sync build/ s3://your-bucket-name
   ```

4. **Set Up CloudFront**
   - AWS Console → CloudFront
   - Create distribution
   - Point to S3 bucket

5. **Configure Custom Domain**
   - Register domain or use Route 53
   - Point to CloudFront distribution

## Database Deployment

### Using Managed MySQL Services

#### AWS RDS
1. Create RDS MySQL instance
2. Copy endpoint to backend `DB_HOST`
3. Configure security groups to allow backend access

#### Railway MySQL
1. Add MySQL add-on
2. Connection details auto-populate in environment

#### DigitalOcean Managed MySQL
1. Create managed MySQL database
2. Get connection string
3. Update backend environment variables

## SSL/HTTPS Configuration

### For Backend (Railway/Heroku)
- Automatically provided with HTTPS

### For Frontend (Vercel/Netlify)
- Automatically provided with HTTPS

### Custom Domain with SSL
- Use Let's Encrypt (free)
- Configure through your hosting provider

## CI/CD Pipeline

### GitHub Actions Example

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: railway-app/action@v1
        with:
          railway-token: ${{ secrets.RAILWAY_TOKEN }}

  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: vercel/action@master
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
          vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
```

## Environment Variables Checklist

### Production Backend
- [ ] NODE_ENV=production
- [ ] PORT (set by platform)
- [ ] Database credentials
- [ ] JWT_SECRET (strong key)
- [ ] M-Pesa credentials
- [ ] Bank API credentials
- [ ] Email credentials
- [ ] Frontend URL (for CORS)
- [ ] API URL

### Production Frontend
- [ ] REACT_APP_API_URL (production backend)
- [ ] NODE_ENV=production

## Monitoring & Logging

### Backend Monitoring
- Use platform monitoring (Railway/Heroku)
- Set up error tracking (Sentry)
- Monitor logs in production

### Frontend Monitoring
- Use Vercel/Netlify analytics
- Set up error tracking (Sentry)
- Monitor performance with Web Vitals

## Backup & Recovery

### Database Backups
- Enable automatic backups in MySQL provider
- Weekly manual exports recommended

### Code Backups
- GitHub is your backup
- Keep main branch stable

## Performance Optimization

### Backend
- Enable caching (Redis)
- Use CDN for static files
- Optimize database queries

### Frontend
- Enable gzip compression
- Use code splitting
- Optimize images

## Security Checklist

- [ ] HTTPS/SSL enabled
- [ ] Environment variables secured
- [ ] CORS properly configured
- [ ] Rate limiting enabled
- [ ] Input validation implemented
- [ ] SQL injection prevented
- [ ] XSS protection enabled
- [ ] CSRF tokens implemented
- [ ] Sensitive data not logged
- [ ] Regular security updates

## Post-Deployment

1. **Test all features**
   - User registration/login
   - Payment processing
   - Receipt generation
   - Email notifications

2. **Monitor logs**
   - Check for errors
   - Monitor performance
   - Track usage patterns

3. **Update documentation**
   - Production URLs
   - Deployment procedures
   - Troubleshooting guide

4. **Set up alerts**
   - Error notifications
   - Uptime monitoring
   - Performance alerts

## Common Issues

### Payment Processing Failed
- Check M-Pesa credentials
- Verify callback URL configuration
- Check network connectivity

### Emails Not Sending
- Verify email credentials
- Check email provider settings
- Review email logs

### Database Connection Issues
- Check database credentials
- Verify network access
- Check firewall rules

### CORS Errors
- Verify FRONTEND_URL in backend
- Check allowed origins
- Review CORS middleware

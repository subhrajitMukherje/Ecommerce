# Deployment Guide

This guide covers deploying the e-commerce application to production environments.

## 🌐 Deployment Options

### Frontend Deployment
- **Vercel** (Recommended for React apps)
- **Netlify** (Great for static sites)
- **AWS S3 + CloudFront** (Scalable solution)
- **GitHub Pages** (Free for public repos)

### Backend Deployment
- **Railway** (Modern, easy deployment)
- **Heroku** (Popular platform-as-a-service)
- **DigitalOcean App Platform** (Cost-effective)
- **AWS EC2** (Full control)

### Database Hosting
- **MongoDB Atlas** (Recommended managed service)
- **AWS DocumentDB** (MongoDB-compatible)

## 🚀 Quick Deployment (Recommended)

### Frontend: Vercel Deployment

1. **Prepare the Build**
```bash
cd client
npm run build
```

2. **Deploy to Vercel**
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

3. **Environment Variables**
Set in Vercel dashboard:
- `VITE_API_URL`: Your backend URL

### Backend: Railway Deployment

1. **Create Railway Account**
- Go to [railway.app](https://railway.app)
- Connect your GitHub account

2. **Deploy from GitHub**
- Create new project
- Connect your repository
- Select the `server` folder as root

3. **Environment Variables**
Set in Railway dashboard:
```env
MONGODB_URI=your_mongodb_atlas_uri
JWT_SECRET=your_jwt_secret
CLOUDINARY_CLOUD_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_cloudinary_key
CLOUDINARY_API_SECRET=your_cloudinary_secret
PAYPAL_MODE=live
PAYPAL_CLIENT_ID=your_paypal_client_id
PAYPAL_CLIENT_SECRET=your_paypal_client_secret
PORT=5000
```

## 📋 Pre-Deployment Checklist

### Security
- [ ] Environment variables are properly configured
- [ ] JWT secret is strong and unique
- [ ] Database credentials are secure
- [ ] CORS is configured for production domains
- [ ] HTTPS is enabled
- [ ] API keys are not exposed in frontend code

### Performance
- [ ] Frontend is built and optimized
- [ ] Images are optimized via Cloudinary
- [ ] Database indexes are created
- [ ] Unused dependencies are removed
- [ ] Bundle size is optimized

### Functionality
- [ ] All features work in production environment
- [ ] Payment processing works with live PayPal
- [ ] Email notifications are configured (if implemented)
- [ ] Error handling is comprehensive
- [ ] Logging is properly configured

## 🔧 Detailed Deployment Steps

### 1. MongoDB Atlas Setup

1. **Create Cluster**
   - Go to [MongoDB Atlas](https://www.mongodb.com/atlas)
   - Create new cluster (free tier available)
   - Choose cloud provider and region

2. **Configure Access**
   - Add IP addresses to whitelist (0.0.0.0/0 for all IPs)
   - Create database user with read/write permissions

3. **Get Connection String**
   - Click "Connect" → "Connect your application"
   - Copy the connection string
   - Replace `<password>` with your database user password

### 2. Cloudinary Production Setup

1. **Upgrade Plan** (if needed)
   - Free tier: 25GB storage, 25GB bandwidth
   - Paid plans for higher usage

2. **Configure Settings**
   - Enable auto-optimization
   - Set up transformations for different image sizes
   - Configure security settings

### 3. PayPal Production Setup

1. **Switch to Live Mode**
   - Change `PAYPAL_MODE` from `sandbox` to `live`
   - Use live PayPal credentials

2. **Update Return URLs**
   - Set return URL to your production domain
   - Update cancel URL as well

3. **Test Payments**
   - Use real PayPal accounts for testing
   - Verify webhook endpoints (if implemented)

### 4. Frontend Production Build

#### Update API URLs
```javascript
// In Redux slices, replace localhost URLs
const API_BASE_URL = import.meta.env.VITE_API_URL || 'http://localhost:5000';
```

#### Build Configuration
```bash
cd client
npm run build
```

#### Environment Variables
Create production environment variables:
```env
VITE_API_URL=https://your-backend-domain.com/api
```

### 5. Backend Production Configuration

#### Update CORS for Production
```javascript
// server/server.js
app.use(
  cors({
    origin: ["https://your-frontend-domain.com", "http://localhost:5173"],
    methods: ["GET", "POST", "DELETE", "PUT"],
    allowedHeaders: [
      "Content-Type",
      "Authorization",
      "Cache-Control",
      "Expires",
      "Pragma",
    ],
    credentials: true,
  })
);
```

#### Production Dependencies
```bash
cd server
npm install --production
```

## 🌍 Platform-Specific Guides

### Vercel Deployment (Frontend)

1. **Connect Repository**
   - Import project from GitHub
   - Select `client` folder as root directory

2. **Build Settings**
   - Build Command: `npm run build`
   - Output Directory: `dist`
   - Install Command: `npm install`

3. **Environment Variables**
   ```
   VITE_API_URL=https://your-backend-url.com/api
   ```

### Railway Deployment (Backend)

1. **Project Setup**
   - Create new project from GitHub
   - Set root directory to `server`

2. **Build Configuration**
   - Start Command: `npm start`
   - Build Command: `npm install`

3. **Environment Variables**
   - Add all required environment variables
   - Use Railway's built-in PostgreSQL if needed

### Heroku Deployment (Backend)

1. **Prepare Application**
```bash
# Add Procfile in server directory
echo "web: node server.js" > server/Procfile
```

2. **Deploy**
```bash
cd server
heroku create your-app-name
git add .
git commit -m "Deploy to Heroku"
git push heroku main
```

3. **Configure Environment**
```bash
heroku config:set MONGODB_URI=your_mongodb_uri
heroku config:set JWT_SECRET=your_jwt_secret
# ... add all other environment variables
```

## 🔍 Post-Deployment Testing

### Functionality Testing
1. **User Registration/Login**
   - Test user registration flow
   - Verify login functionality
   - Check authentication persistence

2. **Product Operations**
   - Browse products
   - Search functionality
   - Product detail views

3. **Shopping Flow**
   - Add products to cart
   - Checkout process
   - Payment completion

4. **Admin Functions**
   - Product management
   - Order management
   - Image uploads

### Performance Testing
1. **Load Testing**
   - Test with multiple concurrent users
   - Monitor response times
   - Check database performance

2. **Image Loading**
   - Verify Cloudinary integration
   - Test image optimization
   - Check loading speeds

## 📊 Monitoring & Maintenance

### Error Monitoring
```javascript
// Add error tracking (e.g., Sentry)
import * as Sentry from "@sentry/react";

Sentry.init({
  dsn: "your-sentry-dsn",
  environment: "production"
});
```

### Performance Monitoring
- Set up uptime monitoring (Pingdom, UptimeRobot)
- Monitor API response times
- Track database performance
- Monitor Cloudinary usage

### Backup Strategy
- **Database**: MongoDB Atlas automatic backups
- **Images**: Cloudinary automatic backup
- **Code**: Git repository with regular commits

## 🔄 CI/CD Pipeline

### GitHub Actions Example
```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy-frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Vercel
        uses: amondnet/vercel-action@v20
        with:
          vercel-token: ${{ secrets.VERCEL_TOKEN }}
          vercel-org-id: ${{ secrets.ORG_ID }}
          vercel-project-id: ${{ secrets.PROJECT_ID }}
          working-directory: ./client

  deploy-backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Deploy to Railway
        # Add Railway deployment action
```

## 🚨 Troubleshooting Deployment Issues

### Common Issues

**1. Build Failures**
- Check Node.js version compatibility
- Verify all dependencies are installed
- Review build logs for specific errors

**2. Environment Variable Issues**
- Ensure all required variables are set
- Check variable names match exactly
- Verify sensitive data is properly encoded

**3. Database Connection Issues**
- Verify MongoDB Atlas IP whitelist
- Check connection string format
- Test database connectivity

**4. CORS Issues**
- Update CORS configuration for production domains
- Verify frontend and backend URLs
- Check browser network tab for specific errors

**5. Payment Issues**
- Switch PayPal to live mode
- Update return URLs to production domain
- Test with real PayPal accounts

### Debug Steps
1. Check deployment logs
2. Verify environment variables
3. Test API endpoints individually
4. Check database connectivity
5. Review browser console errors

## 📈 Scaling Considerations

### Performance Optimization
- **CDN**: Use Cloudinary's CDN for images
- **Caching**: Implement Redis for session storage
- **Database**: Add indexes for frequently queried fields
- **Load Balancing**: Use multiple server instances

### Infrastructure Scaling
- **Horizontal Scaling**: Multiple server instances
- **Database Scaling**: MongoDB sharding or read replicas
- **Microservices**: Split into smaller services
- **Container Deployment**: Docker and Kubernetes

## 💰 Cost Optimization

### Free Tier Limits
- **Vercel**: 100GB bandwidth/month
- **Railway**: $5 credit/month
- **MongoDB Atlas**: 512MB storage
- **Cloudinary**: 25GB storage, 25GB bandwidth

### Cost Monitoring
- Set up billing alerts
- Monitor usage regularly
- Optimize image sizes and formats
- Use efficient database queries

## 🔐 Production Security

### Essential Security Measures
1. **HTTPS Everywhere**: SSL certificates for all domains
2. **Environment Variables**: Never commit secrets to Git
3. **Database Security**: Use strong passwords and IP restrictions
4. **API Security**: Implement rate limiting and input validation
5. **Regular Updates**: Keep dependencies updated

### Security Headers
```javascript
// Add security headers
app.use((req, res, next) => {
  res.setHeader('X-Content-Type-Options', 'nosniff');
  res.setHeader('X-Frame-Options', 'DENY');
  res.setHeader('X-XSS-Protection', '1; mode=block');
  next();
});
```

## 📞 Support & Maintenance

### Regular Maintenance Tasks
- Monitor application performance
- Update dependencies regularly
- Review and rotate API keys
- Backup database regularly
- Monitor error logs

### Emergency Procedures
1. **Service Outage**: Check hosting provider status
2. **Database Issues**: Verify MongoDB Atlas connectivity
3. **Payment Problems**: Check PayPal service status
4. **Image Loading Issues**: Verify Cloudinary service

This deployment guide ensures your e-commerce application runs smoothly in production with proper security, performance, and monitoring in place.
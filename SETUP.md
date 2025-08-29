# Setup Guide

This guide will walk you through setting up the e-commerce application step by step.

## 📋 Prerequisites Checklist

Before starting, ensure you have:
- [ ] Node.js (version 16+) installed
- [ ] MongoDB installed locally OR MongoDB Atlas account
- [ ] Cloudinary account for image storage
- [ ] PayPal Developer account for payments
- [ ] Git installed (for version control)

## 🔧 Step-by-Step Setup

### Step 1: Environment Setup

#### 1.1 Install Node.js
- Download from [nodejs.org](https://nodejs.org/)
- Verify installation: `node --version` and `npm --version`

#### 1.2 MongoDB Setup
**Option A: Local MongoDB**
- Download from [mongodb.com](https://www.mongodb.com/try/download/community)
- Start MongoDB service
- Default connection: `mongodb://localhost:27017`

**Option B: MongoDB Atlas (Recommended)**
- Create account at [mongodb.com/atlas](https://www.mongodb.com/atlas)
- Create a new cluster
- Get connection string from "Connect" button

#### 1.3 Cloudinary Setup
- Create account at [cloudinary.com](https://cloudinary.com/)
- Go to Dashboard to find:
  - Cloud Name
  - API Key
  - API Secret

#### 1.4 PayPal Setup
- Create developer account at [developer.paypal.com](https://developer.paypal.com/)
- Create a new app in the dashboard
- Get Client ID and Client Secret
- Use "sandbox" mode for testing

### Step 2: Project Installation

#### 2.1 Download Project
```bash
# If using Git
git clone <repository-url>
cd ecommerce-app

# Or download and extract ZIP file
```

#### 2.2 Install Dependencies
```bash
# Install backend dependencies
cd server
npm install

# Install frontend dependencies
cd ../client
npm install
```

### Step 3: Configuration

#### 3.1 Backend Configuration
Create `server/.env` file:
```env
# Database Connection
MONGODB_URI=mongodb://localhost:27017/ecommerce
# OR for Atlas: mongodb+srv://username:password@cluster.mongodb.net/ecommerce

# Security
JWT_SECRET=your_super_secret_jwt_key_here

# Cloudinary (for image uploads)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# PayPal (for payments)
PAYPAL_MODE=sandbox
PAYPAL_CLIENT_ID=your_paypal_client_id
PAYPAL_CLIENT_SECRET=your_paypal_client_secret
```

#### 3.2 Update Configuration Files

**Update `server/helpers/cloudinary.js`:**
```javascript
cloudinary.config({
  cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
  api_key: process.env.CLOUDINARY_API_KEY,
  api_secret: process.env.CLOUDINARY_API_SECRET,
});
```

**Update `server/helpers/paypal.js`:**
```javascript
paypal.configure({
  mode: process.env.PAYPAL_MODE,
  client_id: process.env.PAYPAL_CLIENT_ID,
  client_secret: process.env.PAYPAL_CLIENT_SECRET,
});
```

**Update `server/server.js`:**
```javascript
// Add at the top
require('dotenv').config();

// Update MongoDB connection
mongoose
  .connect(process.env.MONGODB_URI)
  .then(() => console.log("MongoDB connected"))
  .catch((error) => console.log(error));
```

**Update `server/controllers/auth/auth-controller.js`:**
```javascript
// Replace "CLIENT_SECRET_KEY" with process.env.JWT_SECRET
const token = jwt.sign(
  {
    id: checkUser._id,
    role: checkUser.role,
    email: checkUser.email,
    userName: checkUser.userName,
  },
  process.env.JWT_SECRET,
  { expiresIn: "60m" }
);

// Also update in authMiddleware function
const decoded = jwt.verify(token, process.env.JWT_SECRET);
```

### Step 4: Running the Application

#### 4.1 Start Backend Server
```bash
cd server
npm run dev
```
Server will start at `http://localhost:5000`

#### 4.2 Start Frontend Application
```bash
cd client
npm run dev
```
Application will open at `http://localhost:5173`

### Step 5: Initial Setup

#### 5.1 Create Admin Account
1. Go to `http://localhost:5173/auth/register`
2. Register with your details
3. Manually update the user role in MongoDB:
   ```javascript
   // In MongoDB Compass or shell
   db.users.updateOne(
     { email: "your-admin-email@example.com" },
     { $set: { role: "admin" } }
   )
   ```

#### 5.2 Add Sample Products (Admin)
1. Login with admin account
2. Go to Admin Dashboard → Products
3. Add sample products with images

#### 5.3 Add Featured Images (Admin)
1. Go to Admin Dashboard
2. Upload banner images for homepage carousel

## 🧪 Testing the Setup

### Test Customer Flow
1. Register as a new customer
2. Browse products on homepage
3. Add products to cart
4. Complete checkout process (use PayPal sandbox)

### Test Admin Flow
1. Login as admin
2. Add/edit products
3. View and manage orders
4. Update order statuses

## 🚨 Common Setup Issues

### Issue: MongoDB Connection Failed
**Solution:**
- Check if MongoDB service is running
- Verify connection string format
- For Atlas: ensure IP whitelist includes your IP

### Issue: Images Not Uploading
**Solution:**
- Verify Cloudinary credentials
- Check file size (max 10MB)
- Ensure internet connection for Cloudinary API

### Issue: PayPal Payments Failing
**Solution:**
- Verify PayPal credentials
- Ensure using sandbox mode for testing
- Check return URLs match your domain

### Issue: CORS Errors
**Solution:**
- Verify frontend URL in server CORS config
- Check that both servers are running
- Clear browser cache and cookies

### Issue: JWT Token Errors
**Solution:**
- Verify JWT_SECRET is set in environment
- Clear browser cookies
- Check token expiration (default: 60 minutes)

## 🔄 Development Workflow

### Adding New Features
1. **Plan**: Define requirements and user stories
2. **Design**: Create UI mockups if needed
3. **Backend**: Add API endpoints and database models
4. **Frontend**: Create components and integrate with API
5. **Test**: Verify functionality works as expected
6. **Document**: Update relevant documentation

### Code Organization
- Keep components small and focused
- Use proper file naming conventions
- Follow existing folder structure
- Add comments for complex logic
- Use TypeScript for better type safety (future enhancement)

## 📈 Performance Optimization

### Current Optimizations
- Image optimization via Cloudinary
- Lazy loading for product images
- Efficient Redux state management
- Responsive design for all devices

### Future Optimizations
- Implement caching strategies
- Add image lazy loading
- Optimize bundle size
- Add service worker for offline functionality

## 🔒 Security Best Practices

### Implemented Security
- Password hashing with bcryptjs
- JWT token authentication
- HTTP-only cookies
- CORS protection
- Input validation

### Additional Security (Recommended)
- Rate limiting for API endpoints
- Input sanitization
- HTTPS in production
- Environment variable validation
- Security headers (helmet.js)

## 📊 Monitoring & Analytics

### Recommended Tools
- **Error Tracking**: Sentry for error monitoring
- **Analytics**: Google Analytics for user behavior
- **Performance**: New Relic or DataDog for server monitoring
- **Uptime**: Pingdom or UptimeRobot for availability

## 🎯 Next Steps

After successful setup:
1. Customize the design to match your brand
2. Add your product catalog
3. Configure payment methods
4. Set up email notifications
5. Implement additional features as needed
6. Deploy to production environment

## 📞 Getting Help

If you encounter issues:
1. Check this setup guide thoroughly
2. Review error messages in browser console
3. Check server logs for backend issues
4. Verify all environment variables are set correctly
5. Ensure all services (MongoDB, Cloudinary, PayPal) are properly configured
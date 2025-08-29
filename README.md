# E-Commerce Application

A full-stack e-commerce application built with React, Node.js, Express, and MongoDB. This application provides a complete shopping experience with separate admin and customer interfaces.

## 🚀 Features

### Customer Features
- **User Authentication**: Secure registration and login system
- **Product Browsing**: Browse products by categories and brands
- **Product Search**: Search products by title, description, category, or brand
- **Shopping Cart**: Add, remove, and update product quantities
- **Product Reviews**: View and add product reviews with star ratings
- **Address Management**: Manage multiple shipping addresses
- **Order Management**: Place orders and track order history
- **PayPal Integration**: Secure payment processing

### Admin Features
- **Product Management**: Add, edit, and delete products
- **Image Upload**: Upload product images via Cloudinary
- **Order Management**: View and update order statuses
- **Dashboard**: Manage featured images for the homepage
- **Inventory Control**: Track product stock levels

## 📁 Project Structure

```
├── client/                 # Frontend React application
│   ├── src/
│   │   ├── components/     # Reusable UI components
│   │   │   ├── admin-view/ # Admin-specific components
│   │   │   ├── auth/       # Authentication components
│   │   │   ├── common/     # Shared components
│   │   │   ├── shopping-view/ # Customer-facing components
│   │   │   └── ui/         # Base UI components (shadcn/ui)
│   │   ├── pages/          # Page components
│   │   │   ├── admin-view/ # Admin pages
│   │   │   ├── auth/       # Authentication pages
│   │   │   └── shopping-view/ # Customer pages
│   │   ├── store/          # Redux state management
│   │   │   ├── admin/      # Admin-related state
│   │   │   └── shop/       # Shopping-related state
│   │   └── config/         # Configuration files
│   └── public/             # Static assets
└── server/                 # Backend Node.js application
    ├── controllers/        # Request handlers
    │   ├── admin/          # Admin controllers
    │   ├── auth/           # Authentication controllers
    │   ├── common/         # Shared controllers
    │   └── shop/           # Shopping controllers
    ├── models/             # MongoDB schemas
    ├── routes/             # API route definitions
    └── helpers/            # Utility functions
```

## 🛠️ Technology Stack

### Frontend
- **React 18**: Modern React with hooks and functional components
- **Redux Toolkit**: State management with RTK Query
- **React Router**: Client-side routing
- **Tailwind CSS**: Utility-first CSS framework
- **shadcn/ui**: Modern UI component library
- **Lucide React**: Icon library
- **Axios**: HTTP client for API requests

### Backend
- **Node.js**: JavaScript runtime
- **Express.js**: Web application framework
- **MongoDB**: NoSQL database
- **Mongoose**: MongoDB object modeling
- **JWT**: JSON Web Tokens for authentication
- **bcryptjs**: Password hashing
- **Cloudinary**: Image upload and management
- **PayPal SDK**: Payment processing

## 📋 Prerequisites

Before running this application, make sure you have:

- **Node.js** (version 16 or higher)
- **MongoDB** (local installation or MongoDB Atlas)
- **Cloudinary Account** (for image uploads)
- **PayPal Developer Account** (for payments)

## ⚙️ Environment Setup

### Backend Configuration

Create a `.env` file in the `server` directory with the following variables:

```env
# Database
MONGODB_URI=your_mongodb_connection_string

# JWT Secret
JWT_SECRET=your_jwt_secret_key

# Cloudinary Configuration
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret

# PayPal Configuration
PAYPAL_MODE=sandbox  # or 'live' for production
PAYPAL_CLIENT_ID=your_paypal_client_id
PAYPAL_CLIENT_SECRET=your_paypal_client_secret
```

### Frontend Configuration

The frontend is configured to connect to the backend at `http://localhost:5000`. If you need to change this, update the API URLs in the Redux slices.

## 🚀 Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/subhrajitMukherje/Ecommerce.git
cd ecommerce-app
```

### 2. Install Backend Dependencies
```bash
cd server
npm install
```

### 3. Install Frontend Dependencies
```bash
cd ../client
npm install
```

### 4. Configure Environment Variables
- Set up your `.env` file in the server directory as described above
- Update the configuration files in `server/helpers/` with your credentials

### 5. Start the Application

**Backend (Terminal 1):**
```bash
cd server
npm run dev
```

**Frontend (Terminal 2):**
```bash
cd client
npm run dev
```

The application will be available at:
- Frontend: `http://localhost:5173`
- Backend API: `http://localhost:5000`

## 🔐 Authentication & Authorization

### User Roles
- **Customer**: Can browse products, manage cart, place orders, and write reviews
- **Admin**: Can manage products, view all orders, and update order statuses

### Authentication Flow
1. Users register with username, email, and password
2. Passwords are hashed using bcryptjs
3. JWT tokens are issued upon successful login
4. Tokens are stored in HTTP-only cookies for security
5. Protected routes verify tokens via middleware

## 🛒 Core Functionality

### Product Management (Admin)
- **Add Products**: Upload images, set prices, manage inventory
- **Edit Products**: Update product information and pricing
- **Delete Products**: Remove products from the catalog
- **Image Upload**: Cloudinary integration for image storage

### Shopping Experience (Customer)
- **Product Catalog**: Browse products with filtering and sorting
- **Product Details**: View detailed product information and reviews
- **Shopping Cart**: Persistent cart with quantity management
- **Checkout Process**: Address selection and PayPal payment
- **Order Tracking**: View order history and status updates

### Review System
- **Star Ratings**: 1-5 star rating system
- **Written Reviews**: Text-based product reviews
- **Purchase Verification**: Only customers who purchased can review
- **Average Ratings**: Calculated and displayed on products

## 🗄️ Database Schema

### User Model
```javascript
{
  userName: String (required, unique),
  email: String (required, unique),
  password: String (required, hashed),
  role: String (default: 'user')
}
```

### Product Model
```javascript
{
  image: String,
  title: String,
  description: String,
  category: String,
  brand: String,
  price: Number,
  salePrice: Number,
  totalStock: Number,
  averageReview: Number
}
```

### Order Model
```javascript
{
  userId: String,
  cartId: String,
  cartItems: Array,
  addressInfo: Object,
  orderStatus: String,
  paymentMethod: String,
  paymentStatus: String,
  totalAmount: Number,
  orderDate: Date,
  paymentId: String,
  payerId: String
}
```

## 🎨 UI Components

The application uses **shadcn/ui** components for a consistent, modern design:

- **Forms**: Input fields, selects, textareas with validation
- **Navigation**: Headers, sidebars, and mobile-responsive menus
- **Data Display**: Tables, cards, badges for product and order information
- **Feedback**: Toasts, dialogs, and loading states
- **Interactive**: Buttons, dropdowns, and modal dialogs

## 🔄 State Management

### Redux Store Structure
```
store/
├── auth-slice          # User authentication state
├── admin/
│   ├── products-slice  # Admin product management
│   └── order-slice     # Admin order management
├── shop/
│   ├── products-slice  # Product catalog and details
│   ├── cart-slice      # Shopping cart state
│   ├── address-slice   # User addresses
│   ├── order-slice     # Customer orders
│   ├── search-slice    # Product search
│   └── review-slice    # Product reviews
└── common-slice        # Shared state (featured images)
```

## 🛡️ Security Features

- **Password Hashing**: bcryptjs with salt rounds
- **JWT Authentication**: Secure token-based authentication
- **HTTP-Only Cookies**: Prevents XSS attacks
- **CORS Configuration**: Controlled cross-origin requests
- **Input Validation**: Server-side validation for all inputs
- **Role-Based Access**: Admin and customer role separation

## 📱 Responsive Design

The application is fully responsive with:
- **Mobile-First Approach**: Optimized for mobile devices
- **Breakpoint System**: Tailwind CSS responsive utilities
- **Flexible Layouts**: Grid and flexbox layouts
- **Touch-Friendly**: Large touch targets for mobile users

## 🔧 API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/check-auth` - Verify authentication

### Admin Products
- `GET /api/admin/products/get` - Get all products
- `POST /api/admin/products/add` - Add new product
- `PUT /api/admin/products/edit/:id` - Edit product
- `DELETE /api/admin/products/delete/:id` - Delete product
- `POST /api/admin/products/upload-image` - Upload product image

### Shopping
- `GET /api/shop/products/get` - Get filtered products
- `GET /api/shop/products/get/:id` - Get product details
- `POST /api/shop/cart/add` - Add to cart
- `GET /api/shop/cart/get/:userId` - Get cart items
- `PUT /api/shop/cart/update-cart` - Update cart quantity
- `DELETE /api/shop/cart/:userId/:productId` - Remove from cart

### Orders
- `POST /api/shop/order/create` - Create new order
- `POST /api/shop/order/capture` - Capture PayPal payment
- `GET /api/shop/order/list/:userId` - Get user orders
- `GET /api/shop/order/details/:id` - Get order details

## 🧪 Testing

Currently, the application doesn't include automated tests. For future development, consider adding:
- **Unit Tests**: Jest for component and function testing
- **Integration Tests**: API endpoint testing
- **E2E Tests**: Cypress or Playwright for user flow testing

## 🚀 Deployment

### Frontend Deployment
The React application can be deployed to:
- **Vercel**: Zero-config deployment
- **Netlify**: Static site hosting
- **AWS S3 + CloudFront**: Scalable hosting solution

### Backend Deployment
The Node.js server can be deployed to:
- **Heroku**: Easy deployment with add-ons
- **AWS EC2**: Full control over server environment
- **DigitalOcean**: Cost-effective VPS hosting
- **Railway**: Modern deployment platform

### Database Hosting
- **MongoDB Atlas**: Managed MongoDB service
- **AWS DocumentDB**: MongoDB-compatible database service

## 🔮 Future Enhancements

### Planned Features
- **Wishlist**: Save products for later
- **Product Recommendations**: AI-powered suggestions
- **Advanced Search**: Filters by price range, ratings, etc.
- **Email Notifications**: Order confirmations and updates
- **Inventory Alerts**: Low stock notifications for admins
- **Analytics Dashboard**: Sales and user behavior insights

### Technical Improvements
- **TypeScript Migration**: Add type safety
- **API Rate Limiting**: Prevent abuse
- **Caching**: Redis for improved performance
- **Image Optimization**: WebP format and lazy loading
- **PWA Features**: Offline functionality and push notifications

## 🐛 Troubleshooting

### Common Issues

**1. MongoDB Connection Error**
- Verify your MongoDB URI in the environment variables
- Ensure MongoDB service is running (if using local installation)
- Check network connectivity for MongoDB Atlas

**2. Image Upload Failing**
- Verify Cloudinary credentials in environment variables
- Check file size limits (default: 10MB)
- Ensure proper file formats (JPEG, PNG, WebP)

**3. PayPal Payment Issues**
- Verify PayPal credentials and mode (sandbox/live)
- Check return URLs match your domain
- Ensure proper PayPal account setup

**4. CORS Errors**
- Verify frontend URL in CORS configuration
- Check that credentials are included in requests
- Ensure proper headers are allowed

## 📞 Support

For technical support or questions:
1. Check the troubleshooting section above
2. Review the API documentation
3. Check browser console for error messages
4. Verify environment variable configuration

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📚 Additional Resources

- [React Documentation](https://reactjs.org/docs)
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [MongoDB Manual](https://docs.mongodb.com/manual/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [shadcn/ui Components](https://ui.shadcn.com/docs)
- [PayPal Developer Documentation](https://developer.paypal.com/docs/)

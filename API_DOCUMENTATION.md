# API Documentation

This document provides comprehensive information about all API endpoints available in the e-commerce application.

## 🔗 Base URL
```
http://localhost:5000/api
```

## 🔐 Authentication

All protected endpoints require a valid JWT token sent via HTTP-only cookies.

### Authentication Headers
```javascript
// Cookies are automatically sent by the browser
// No manual headers required for authentication
```

## 📚 API Endpoints

### 🔑 Authentication Endpoints

#### Register User
```http
POST /auth/register
```

**Request Body:**
```json
{
  "userName": "john_doe",
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Registration successful"
}
```

#### Login User
```http
POST /auth/login
```

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Logged in successfully",
  "user": {
    "id": "user_id",
    "email": "john@example.com",
    "userName": "john_doe",
    "role": "user"
  }
}
```

#### Logout User
```http
POST /auth/logout
```

**Response:**
```json
{
  "success": true,
  "message": "Logged out successfully!"
}
```

#### Check Authentication
```http
GET /auth/check-auth
```

**Response:**
```json
{
  "success": true,
  "message": "Authenticated user!",
  "user": {
    "id": "user_id",
    "email": "john@example.com",
    "userName": "john_doe",
    "role": "user"
  }
}
```

---

### 🛍️ Shopping Endpoints

#### Get Filtered Products
```http
GET /shop/products/get?category=men&brand=nike&sortBy=price-lowtohigh
```

**Query Parameters:**
- `category` (optional): Filter by category (men, women, kids, accessories, footwear)
- `brand` (optional): Filter by brand (nike, adidas, puma, levi, zara, h&m)
- `sortBy` (optional): Sort order (price-lowtohigh, price-hightolow, title-atoz, title-ztoa)

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "product_id",
      "title": "Nike Air Max",
      "description": "Comfortable running shoes",
      "category": "footwear",
      "brand": "nike",
      "price": 120,
      "salePrice": 100,
      "totalStock": 50,
      "image": "cloudinary_url",
      "averageReview": 4.5
    }
  ]
}
```

#### Get Product Details
```http
GET /shop/products/get/:id
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "product_id",
    "title": "Nike Air Max",
    "description": "Comfortable running shoes with advanced cushioning",
    "category": "footwear",
    "brand": "nike",
    "price": 120,
    "salePrice": 100,
    "totalStock": 50,
    "image": "cloudinary_url",
    "averageReview": 4.5,
    "createdAt": "2024-01-01T00:00:00.000Z"
  }
}
```

#### Search Products
```http
GET /shop/search/:keyword
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "product_id",
      "title": "Nike Air Max",
      "description": "Comfortable running shoes",
      "category": "footwear",
      "brand": "nike",
      "price": 120,
      "salePrice": 100,
      "totalStock": 50,
      "image": "cloudinary_url"
    }
  ]
}
```

---

### 🛒 Cart Endpoints

#### Add to Cart
```http
POST /shop/cart/add
```

**Request Body:**
```json
{
  "userId": "user_id",
  "productId": "product_id",
  "quantity": 2
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "cart_id",
    "userId": "user_id",
    "items": [
      {
        "productId": "product_id",
        "quantity": 2
      }
    ]
  }
}
```

#### Get Cart Items
```http
GET /shop/cart/get/:userId
```

**Response:**
```json
{
  "success": true,
  "data": {
    "_id": "cart_id",
    "userId": "user_id",
    "items": [
      {
        "productId": "product_id",
        "title": "Nike Air Max",
        "image": "cloudinary_url",
        "price": 120,
        "salePrice": 100,
        "quantity": 2
      }
    ]
  }
}
```

#### Update Cart Quantity
```http
PUT /shop/cart/update-cart
```

**Request Body:**
```json
{
  "userId": "user_id",
  "productId": "product_id",
  "quantity": 3
}
```

#### Remove from Cart
```http
DELETE /shop/cart/:userId/:productId
```

---

### 📍 Address Endpoints

#### Add Address
```http
POST /shop/address/add
```

**Request Body:**
```json
{
  "userId": "user_id",
  "address": "123 Main Street",
  "city": "New York",
  "pincode": "10001",
  "phone": "+1234567890",
  "notes": "Apartment 4B"
}
```

#### Get User Addresses
```http
GET /shop/address/get/:userId
```

#### Update Address
```http
PUT /shop/address/update/:userId/:addressId
```

#### Delete Address
```http
DELETE /shop/address/delete/:userId/:addressId
```

---

### 📦 Order Endpoints

#### Create Order
```http
POST /shop/order/create
```

**Request Body:**
```json
{
  "userId": "user_id",
  "cartId": "cart_id",
  "cartItems": [
    {
      "productId": "product_id",
      "title": "Nike Air Max",
      "image": "cloudinary_url",
      "price": 100,
      "quantity": 1
    }
  ],
  "addressInfo": {
    "addressId": "address_id",
    "address": "123 Main Street",
    "city": "New York",
    "pincode": "10001",
    "phone": "+1234567890",
    "notes": "Apartment 4B"
  },
  "orderStatus": "pending",
  "paymentMethod": "paypal",
  "paymentStatus": "pending",
  "totalAmount": 100,
  "orderDate": "2024-01-01T00:00:00.000Z"
}
```

**Response:**
```json
{
  "success": true,
  "approvalURL": "https://www.sandbox.paypal.com/checkoutnow?token=...",
  "orderId": "order_id"
}
```

#### Capture Payment
```http
POST /shop/order/capture
```

**Request Body:**
```json
{
  "paymentId": "paypal_payment_id",
  "payerId": "paypal_payer_id",
  "orderId": "order_id"
}
```

#### Get User Orders
```http
GET /shop/order/list/:userId
```

#### Get Order Details
```http
GET /shop/order/details/:id
```

---

### ⭐ Review Endpoints

#### Add Product Review
```http
POST /shop/review/add
```

**Request Body:**
```json
{
  "productId": "product_id",
  "userId": "user_id",
  "userName": "john_doe",
  "reviewMessage": "Great product! Highly recommended.",
  "reviewValue": 5
}
```

#### Get Product Reviews
```http
GET /shop/review/:productId
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "_id": "review_id",
      "productId": "product_id",
      "userId": "user_id",
      "userName": "john_doe",
      "reviewMessage": "Great product!",
      "reviewValue": 5,
      "createdAt": "2024-01-01T00:00:00.000Z"
    }
  ]
}
```

---

### 👨‍💼 Admin Endpoints

#### Get All Products (Admin)
```http
GET /admin/products/get
```

#### Add Product (Admin)
```http
POST /admin/products/add
```

**Request Body:**
```json
{
  "title": "Nike Air Max",
  "description": "Comfortable running shoes",
  "category": "footwear",
  "brand": "nike",
  "price": 120,
  "salePrice": 100,
  "totalStock": 50,
  "image": "cloudinary_url"
}
```

#### Upload Product Image (Admin)
```http
POST /admin/products/upload-image
```

**Request:** Multipart form data with file field named `my_file`

**Response:**
```json
{
  "success": true,
  "result": {
    "url": "https://res.cloudinary.com/...",
    "public_id": "image_public_id"
  }
}
```

#### Edit Product (Admin)
```http
PUT /admin/products/edit/:id
```

#### Delete Product (Admin)
```http
DELETE /admin/products/delete/:id
```

#### Get All Orders (Admin)
```http
GET /admin/orders/get
```

#### Get Order Details (Admin)
```http
GET /admin/orders/details/:id
```

#### Update Order Status (Admin)
```http
PUT /admin/orders/update/:id
```

**Request Body:**
```json
{
  "orderStatus": "confirmed"
}
```

**Available Order Statuses:**
- `pending`
- `inProcess`
- `inShipping`
- `delivered`
- `rejected`

---

### 🖼️ Feature Image Endpoints

#### Add Feature Image (Admin)
```http
POST /common/feature/add
```

**Request Body:**
```json
{
  "image": "cloudinary_image_url"
}
```

#### Get Feature Images
```http
GET /common/feature/get
```

---

## 📊 Response Format

### Success Response
```json
{
  "success": true,
  "message": "Operation successful",
  "data": { /* response data */ }
}
```

### Error Response
```json
{
  "success": false,
  "message": "Error description"
}
```

## 🚨 Error Codes

| Status Code | Description |
|-------------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

## 🔄 Rate Limiting

Currently, no rate limiting is implemented. For production, consider adding:
- Request rate limiting per IP
- User-specific rate limiting
- API key authentication for external access

## 📝 Request/Response Examples

### Complete Order Flow Example

1. **Add to Cart:**
```bash
curl -X POST http://localhost:5000/api/shop/cart/add \
  -H "Content-Type: application/json" \
  -d '{"userId":"user123","productId":"prod456","quantity":1}'
```

2. **Create Order:**
```bash
curl -X POST http://localhost:5000/api/shop/order/create \
  -H "Content-Type: application/json" \
  -d '{
    "userId": "user123",
    "cartItems": [...],
    "addressInfo": {...},
    "totalAmount": 100
  }'
```

3. **Capture Payment:**
```bash
curl -X POST http://localhost:5000/api/shop/order/capture \
  -H "Content-Type: application/json" \
  -d '{
    "paymentId": "paypal_payment_id",
    "payerId": "paypal_payer_id",
    "orderId": "order_id"
  }'
```

## 🧪 Testing with Postman

### Import Collection
Create a Postman collection with the following structure:
1. Authentication folder (register, login, logout, check-auth)
2. Products folder (get products, product details, search)
3. Cart folder (add, get, update, delete)
4. Orders folder (create, capture, list, details)
5. Admin folder (product management, order management)

### Environment Variables
Set up Postman environment with:
- `baseUrl`: `http://localhost:5000/api`
- `userId`: Your test user ID
- `productId`: A test product ID
- `orderId`: A test order ID

## 🔧 Development Tips

### Testing API Endpoints
1. Use Postman or similar tool for manual testing
2. Test both success and error scenarios
3. Verify authentication requirements
4. Check response data structure

### Adding New Endpoints
1. Create controller function
2. Add route definition
3. Update this documentation
4. Add frontend integration
5. Test thoroughly

### Error Handling
All endpoints should return consistent error responses:
```json
{
  "success": false,
  "message": "Descriptive error message"
}
```

## 📋 Validation Rules

### User Registration
- `userName`: Required, unique, 3-50 characters
- `email`: Required, valid email format, unique
- `password`: Required, minimum 6 characters

### Product Creation
- `title`: Required, 1-200 characters
- `description`: Required, 1-1000 characters
- `category`: Required, must be valid category
- `brand`: Required, must be valid brand
- `price`: Required, positive number
- `totalStock`: Required, non-negative integer

### Address Creation
- All fields required except `notes`
- `phone`: Valid phone number format
- `pincode`: Valid postal code format

## 🔄 Pagination

Currently not implemented. For future enhancement:
```http
GET /shop/products/get?page=1&limit=20
```

## 🔍 Filtering & Sorting

### Available Filters
- **Category**: men, women, kids, accessories, footwear
- **Brand**: nike, adidas, puma, levi, zara, h&m

### Available Sort Options
- `price-lowtohigh`: Price ascending
- `price-hightolow`: Price descending
- `title-atoz`: Title A to Z
- `title-ztoa`: Title Z to A

## 📈 Performance Considerations

### Database Queries
- Products are indexed by category and brand
- Use pagination for large datasets
- Implement caching for frequently accessed data

### Image Handling
- Images are stored on Cloudinary
- Automatic optimization and resizing
- CDN delivery for fast loading

## 🔒 Security Considerations

### Input Validation
- All inputs are validated on the server
- SQL injection prevention (using Mongoose)
- XSS protection through proper encoding

### Authentication Security
- JWT tokens expire after 60 minutes
- HTTP-only cookies prevent XSS attacks
- Passwords are hashed with bcryptjs

## 🐛 Common Issues & Solutions

### Issue: 401 Unauthorized
**Cause:** Invalid or expired JWT token
**Solution:** Login again to get a new token

### Issue: 404 Not Found
**Cause:** Invalid endpoint URL or resource doesn't exist
**Solution:** Check endpoint URL and verify resource exists

### Issue: 500 Internal Server Error
**Cause:** Server-side error (database connection, etc.)
**Solution:** Check server logs and database connectivity

### Issue: CORS Error
**Cause:** Frontend and backend on different origins
**Solution:** Verify CORS configuration in server.js

## 📞 Support

For API-related questions:
1. Check this documentation first
2. Verify request format and required fields
3. Check server logs for detailed error messages
4. Test with a tool like Postman to isolate issues
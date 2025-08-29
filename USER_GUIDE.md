# User Guide

Welcome to the E-Commerce Application! This guide will help you navigate and use all the features available to both customers and administrators.

## 🛍️ For Customers

### Getting Started

#### 1. Creating an Account
1. Visit the application homepage
2. Click "Register" in the top navigation
3. Fill in your details:
   - **Username**: Choose a unique username
   - **Email**: Your email address (used for login)
   - **Password**: Create a secure password
4. Click "Sign Up" to create your account
5. You'll be redirected to the login page

#### 2. Logging In
1. Click "Login" from the homepage
2. Enter your email and password
3. Click "Sign In"
4. You'll be taken to the shopping homepage

### Shopping Features

#### Browsing Products

**Homepage Shopping**
- View featured products on the homepage
- Browse by category (Men, Women, Kids, Accessories, Footwear)
- Browse by brand (Nike, Adidas, Puma, Levi's, Zara, H&M)
- Use the image carousel to see featured items

**Product Listing Page**
- Click "Products" in the navigation to see all items
- Use filters on the left sidebar:
  - Filter by category
  - Filter by brand
  - Apply multiple filters at once
- Sort products by:
  - Price: Low to High
  - Price: High to Low
  - Title: A to Z
  - Title: Z to A

**Product Details**
- Click on any product to see detailed information
- View high-quality product images
- Read product descriptions
- Check customer reviews and ratings
- See current price and sale prices
- Check stock availability

#### Shopping Cart

**Adding Items**
- Click "Add to Cart" on any product
- Items are automatically added to your cart
- Cart icon shows the number of items

**Managing Your Cart**
- Click the cart icon to view your items
- Increase or decrease quantities using + and - buttons
- Remove items by clicking the trash icon
- View total price automatically calculated

**Stock Limitations**
- System prevents adding more items than available in stock
- You'll see warnings if you try to exceed stock limits

#### Checkout Process

**1. Review Your Cart**
- Click "Checkout" from your cart
- Review all items and quantities
- Verify the total amount

**2. Select Shipping Address**
- Choose from your saved addresses
- Or add a new address with:
  - Street address
  - City
  - Postal code
  - Phone number
  - Special delivery notes

**3. Payment**
- Currently supports PayPal payments
- Click "Checkout with PayPal"
- Complete payment on PayPal's secure platform
- Return to the site for order confirmation

#### Managing Your Account

**Address Management**
- Go to "Account" → "Address" tab
- Add up to 3 shipping addresses
- Edit existing addresses
- Delete addresses you no longer need

**Order History**
- Go to "Account" → "Orders" tab
- View all your past orders
- Check order status:
  - **Pending**: Order received, processing
  - **In Process**: Order being prepared
  - **In Shipping**: Order shipped
  - **Delivered**: Order completed
  - **Rejected**: Order cancelled
- Click "View Details" to see complete order information

#### Product Reviews

**Writing Reviews**
- You can only review products you've purchased
- Open product details page
- Scroll down to the reviews section
- Rate the product (1-5 stars)
- Write your review message
- Click "Submit" to publish your review

**Reading Reviews**
- All product reviews are visible to everyone
- See average star rating
- Read individual customer experiences
- Reviews help other customers make decisions

#### Search Functionality

**Product Search**
- Click "Search" in the navigation
- Type keywords to find products
- Search looks through:
  - Product titles
  - Product descriptions
  - Categories
  - Brand names
- Results update automatically as you type

---

## 👨‍💼 For Administrators

### Admin Access
- Admin accounts must be created manually in the database
- Contact your system administrator for admin access
- Admin users see different navigation and features

### Product Management

#### Adding New Products
1. Go to "Admin Panel" → "Products"
2. Click "Add New Product"
3. Fill in product information:
   - **Title**: Product name
   - **Description**: Detailed product description
   - **Category**: Select from available categories
   - **Brand**: Select from available brands
   - **Price**: Regular selling price
   - **Sale Price**: Discounted price (optional)
   - **Total Stock**: Number of items available
4. Upload product image:
   - Drag and drop image file
   - Or click to browse and select
   - Supported formats: JPG, PNG, WebP
   - Maximum file size: 10MB
5. Click "Add" to create the product

#### Managing Existing Products
- **Edit Products**: Click "Edit" on any product card
- **Delete Products**: Click "Delete" to remove products
- **View Products**: See all products in a grid layout
- **Stock Management**: Update stock levels when editing

#### Image Management
- Images are automatically uploaded to Cloudinary
- Images are optimized for web delivery
- Original images are preserved for quality

### Order Management

#### Viewing Orders
1. Go to "Admin Panel" → "Orders"
2. See all customer orders in a table format
3. Information displayed:
   - Order ID
   - Order date
   - Current status
   - Total amount
   - Customer information

#### Managing Order Status
1. Click "View Details" on any order
2. See complete order information:
   - Customer details
   - Shipping address
   - Items ordered
   - Payment information
3. Update order status using the dropdown:
   - **Pending**: New orders
   - **In Process**: Being prepared
   - **In Shipping**: Shipped to customer
   - **Delivered**: Successfully delivered
   - **Rejected**: Cancelled orders
4. Click "Update Order Status" to save changes

#### Order Details
Each order shows:
- **Customer Information**: Name and contact details
- **Shipping Address**: Complete delivery address
- **Items Ordered**: Products, quantities, and prices
- **Payment Details**: Payment method and status
- **Order Timeline**: Creation and update dates

### Dashboard Features

#### Featured Images Management
1. Go to "Admin Panel" → "Dashboard"
2. Upload banner images for the homepage carousel
3. Images appear in the rotating banner on the customer homepage
4. Use high-quality, wide images (recommended: 1200x400 pixels)

#### Analytics Overview
- View uploaded featured images
- Monitor recent activity
- Quick access to other admin functions

---

## 💡 Tips & Best Practices

### For Customers

**Shopping Tips**
- Check for sale prices (shown with strikethrough on regular price)
- Read reviews before purchasing
- Add items to cart early if stock is low
- Save multiple addresses for easy checkout

**Account Security**
- Use a strong, unique password
- Log out when using shared computers
- Keep your contact information updated

**Order Management**
- Save your order confirmation details
- Track order status in your account
- Contact support if orders are delayed

### For Administrators

**Product Management**
- Use high-quality product images
- Write detailed, accurate descriptions
- Keep stock levels updated
- Monitor which products are popular

**Order Processing**
- Update order statuses promptly
- Communicate with customers about delays
- Process orders in chronological order
- Keep accurate inventory records

**Customer Service**
- Monitor product reviews for issues
- Respond to customer concerns quickly
- Use order details to resolve problems
- Keep product information accurate

## 🚨 Troubleshooting

### Common Customer Issues

**Can't Add Items to Cart**
- Check if the product is in stock
- Try refreshing the page
- Clear your browser cache
- Make sure you're logged in

**Payment Issues**
- Ensure your PayPal account has sufficient funds
- Check that your PayPal account is verified
- Try using a different browser
- Contact support if problems persist

**Login Problems**
- Verify your email and password are correct
- Check if Caps Lock is on
- Try resetting your password (if feature is available)
- Clear browser cookies and try again

**Images Not Loading**
- Check your internet connection
- Try refreshing the page
- Clear browser cache
- Images are hosted on Cloudinary - check if the service is available

### Common Admin Issues

**Can't Upload Images**
- Check file size (maximum 10MB)
- Use supported formats (JPG, PNG, WebP)
- Verify internet connection
- Try a different image file

**Orders Not Updating**
- Refresh the orders page
- Check if you have admin permissions
- Verify the order exists in the system
- Try logging out and back in

**Products Not Appearing**
- Check if the product was saved successfully
- Verify all required fields were filled
- Refresh the products page
- Check if there are any error messages

## 📞 Getting Help

### Customer Support
- Check this user guide first
- Look for error messages on screen
- Try the troubleshooting steps above
- Contact your system administrator

### Technical Support
- Provide specific error messages
- Include steps to reproduce the issue
- Mention which browser you're using
- Include screenshots if helpful

### Feature Requests
- Describe the feature you'd like to see
- Explain how it would help your workflow
- Provide examples from other applications
- Submit through your organization's feedback process

## 🔄 Regular Maintenance

### For Customers
- **Weekly**: Check for new products and sales
- **Monthly**: Review and update your addresses
- **As Needed**: Update account information

### For Administrators
- **Daily**: Process new orders and update statuses
- **Weekly**: Add new products and update inventory
- **Monthly**: Review sales data and customer feedback
- **Quarterly**: Update featured images and promotions

This user guide covers all the essential features and functions of the e-commerce application. Whether you're shopping or managing the store, these instructions will help you make the most of the platform.
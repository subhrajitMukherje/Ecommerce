# Developer Guide

This guide provides detailed technical information for developers working on the e-commerce application.

## 🏗️ Architecture Overview

### Frontend Architecture
```
React Application (SPA)
├── Component Layer (UI Components)
├── State Management (Redux Toolkit)
├── Routing (React Router)
├── API Layer (Axios + Redux Thunks)
└── Styling (Tailwind CSS + shadcn/ui)
```

### Backend Architecture
```
Express.js Server
├── Route Layer (Express Routes)
├── Controller Layer (Business Logic)
├── Model Layer (Mongoose Schemas)
├── Middleware (Auth, CORS, etc.)
└── External Services (Cloudinary, PayPal)
```

## 🔧 Development Setup

### Prerequisites
- Node.js 16+
- MongoDB (local or Atlas)
- Git
- Code editor (VS Code recommended)

### Recommended VS Code Extensions
- ES7+ React/Redux/React-Native snippets
- Tailwind CSS IntelliSense
- Auto Rename Tag
- Bracket Pair Colorizer
- GitLens
- Thunder Client (API testing)

### Development Environment
```bash
# Clone repository
git clone <repo-url>
cd ecommerce-app

# Install dependencies
cd server && npm install
cd ../client && npm install

# Start development servers
# Terminal 1 (Backend)
cd server && npm run dev

# Terminal 2 (Frontend)
cd client && npm run dev
```

## 📁 Code Organization

### Frontend Structure
```
src/
├── components/          # Reusable UI components
│   ├── admin-view/     # Admin-specific components
│   ├── auth/           # Authentication components
│   ├── common/         # Shared components
│   ├── shopping-view/  # Customer components
│   └── ui/             # Base UI components (shadcn/ui)
├── pages/              # Page-level components
├── store/              # Redux state management
├── config/             # Configuration constants
├── lib/                # Utility functions
└── assets/             # Static assets
```

### Backend Structure
```
server/
├── controllers/        # Request handlers
├── models/            # Database schemas
├── routes/            # API route definitions
├── helpers/           # Utility functions
└── middleware/        # Custom middleware
```

## 🎯 Development Guidelines

### Code Style
- Use functional components with hooks
- Follow camelCase naming convention
- Use descriptive variable and function names
- Keep components small and focused (< 200 lines)
- Use TypeScript for better type safety (future enhancement)

### Component Guidelines
```javascript
// Good: Functional component with hooks
function ProductCard({ product, onAddToCart }) {
  const [isLoading, setIsLoading] = useState(false);
  
  const handleAddToCart = async () => {
    setIsLoading(true);
    await onAddToCart(product.id);
    setIsLoading(false);
  };

  return (
    <Card>
      <CardContent>
        <h3>{product.title}</h3>
        <Button onClick={handleAddToCart} disabled={isLoading}>
          {isLoading ? 'Adding...' : 'Add to Cart'}
        </Button>
      </CardContent>
    </Card>
  );
}
```

### State Management Patterns
```javascript
// Redux slice example
const productSlice = createSlice({
  name: 'products',
  initialState: {
    items: [],
    loading: false,
    error: null
  },
  reducers: {
    setLoading: (state, action) => {
      state.loading = action.payload;
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchProducts.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchProducts.fulfilled, (state, action) => {
        state.loading = false;
        state.items = action.payload;
      })
      .addCase(fetchProducts.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  }
});
```

## 🔄 API Integration

### Async Thunk Pattern
```javascript
export const fetchProducts = createAsyncThunk(
  'products/fetchProducts',
  async (params, { rejectWithValue }) => {
    try {
      const response = await axios.get('/api/products', { params });
      return response.data;
    } catch (error) {
      return rejectWithValue(error.response.data);
    }
  }
);
```

### Error Handling
```javascript
// Component error handling
const { data, loading, error } = useSelector(state => state.products);

if (loading) return <Skeleton />;
if (error) return <ErrorMessage message={error} />;
if (!data.length) return <EmptyState />;
```

## 🎨 Styling Guidelines

### Tailwind CSS Best Practices
```javascript
// Good: Semantic class grouping
<div className="flex items-center justify-between p-4 bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow">

// Better: Extract to component classes
<div className="product-card">
```

### Component Styling
```css
/* Use @apply for component classes */
.product-card {
  @apply flex items-center justify-between p-4 bg-white rounded-lg shadow-md hover:shadow-lg transition-shadow;
}

.btn-primary {
  @apply bg-blue-600 hover:bg-blue-700 text-white font-medium py-2 px-4 rounded-md transition-colors;
}
```

### Responsive Design
```javascript
// Mobile-first responsive classes
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4">
```

## 🗄️ Database Development

### Schema Design Principles
- Use appropriate data types
- Add indexes for frequently queried fields
- Implement proper relationships
- Include timestamps for audit trails

### Model Example
```javascript
const ProductSchema = new mongoose.Schema({
  title: {
    type: String,
    required: true,
    trim: true,
    maxlength: 200
  },
  price: {
    type: Number,
    required: true,
    min: 0
  },
  category: {
    type: String,
    required: true,
    enum: ['men', 'women', 'kids', 'accessories', 'footwear']
  }
}, {
  timestamps: true,
  toJSON: { virtuals: true }
});

// Add indexes
ProductSchema.index({ category: 1, brand: 1 });
ProductSchema.index({ title: 'text', description: 'text' });
```

### Query Optimization
```javascript
// Good: Use lean() for read-only queries
const products = await Product.find(filters).lean();

// Good: Select only needed fields
const users = await User.find({}, 'userName email role');

// Good: Use populate efficiently
const cart = await Cart.findOne({ userId })
  .populate('items.productId', 'title price image');
```

## 🔐 Security Implementation

### Authentication Middleware
```javascript
const authMiddleware = async (req, res, next) => {
  try {
    const token = req.cookies.token;
    if (!token) {
      return res.status(401).json({
        success: false,
        message: 'Access denied. No token provided.'
      });
    }

    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    res.status(401).json({
      success: false,
      message: 'Invalid token.'
    });
  }
};
```

### Input Validation
```javascript
// Server-side validation example
const validateProduct = (req, res, next) => {
  const { title, price, category } = req.body;
  
  if (!title || title.trim().length === 0) {
    return res.status(400).json({
      success: false,
      message: 'Product title is required'
    });
  }
  
  if (!price || price <= 0) {
    return res.status(400).json({
      success: false,
      message: 'Valid price is required'
    });
  }
  
  next();
};
```

## 🧪 Testing Strategy

### Unit Testing Setup
```bash
# Install testing dependencies
npm install --save-dev jest @testing-library/react @testing-library/jest-dom
```

### Component Testing Example
```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import { Provider } from 'react-redux';
import ProductCard from '../ProductCard';
import store from '../../store/store';

test('renders product information correctly', () => {
  const mockProduct = {
    id: '1',
    title: 'Test Product',
    price: 99.99
  };

  render(
    <Provider store={store}>
      <ProductCard product={mockProduct} />
    </Provider>
  );

  expect(screen.getByText('Test Product')).toBeInTheDocument();
  expect(screen.getByText('$99.99')).toBeInTheDocument();
});
```

### API Testing
```javascript
// Use supertest for API testing
const request = require('supertest');
const app = require('../server');

describe('Products API', () => {
  test('GET /api/shop/products/get', async () => {
    const response = await request(app)
      .get('/api/shop/products/get')
      .expect(200);
    
    expect(response.body.success).toBe(true);
    expect(Array.isArray(response.body.data)).toBe(true);
  });
});
```

## 🔄 State Management Patterns

### Redux Toolkit Best Practices
```javascript
// Use createAsyncThunk for async operations
export const fetchProducts = createAsyncThunk(
  'products/fetchProducts',
  async (params, { getState, rejectWithValue }) => {
    try {
      const { auth } = getState();
      const response = await api.get('/products', {
        headers: { Authorization: `Bearer ${auth.token}` },
        params
      });
      return response.data;
    } catch (error) {
      return rejectWithValue(error.response.data);
    }
  }
);

// Use extraReducers for async state updates
extraReducers: (builder) => {
  builder
    .addCase(fetchProducts.pending, (state) => {
      state.loading = true;
      state.error = null;
    })
    .addCase(fetchProducts.fulfilled, (state, action) => {
      state.loading = false;
      state.items = action.payload.data;
    })
    .addCase(fetchProducts.rejected, (state, action) => {
      state.loading = false;
      state.error = action.payload?.message || 'Failed to fetch products';
    });
}
```

### Component State vs Global State
```javascript
// Use local state for UI-only state
const [isOpen, setIsOpen] = useState(false);
const [formData, setFormData] = useState(initialFormData);

// Use global state for shared data
const { products, loading } = useSelector(state => state.products);
const { user } = useSelector(state => state.auth);
```

## 🎨 UI Development

### Component Composition
```javascript
// Good: Composable components
function ProductList({ products, onProductClick }) {
  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
      {products.map(product => (
        <ProductCard 
          key={product.id}
          product={product}
          onClick={() => onProductClick(product.id)}
        />
      ))}
    </div>
  );
}

// Good: Reusable form components
function FormField({ label, error, children }) {
  return (
    <div className="space-y-2">
      <Label>{label}</Label>
      {children}
      {error && <p className="text-red-500 text-sm">{error}</p>}
    </div>
  );
}
```

### Custom Hooks
```javascript
// Custom hook for API calls
function useProducts(filters = {}) {
  const dispatch = useDispatch();
  const { products, loading, error } = useSelector(state => state.products);

  useEffect(() => {
    dispatch(fetchProducts(filters));
  }, [dispatch, filters]);

  return { products, loading, error };
}

// Custom hook for form handling
function useForm(initialValues, validationSchema) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});

  const handleChange = (name, value) => {
    setValues(prev => ({ ...prev, [name]: value }));
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({ ...prev, [name]: null }));
    }
  };

  const validate = () => {
    const newErrors = {};
    // Validation logic here
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  return { values, errors, handleChange, validate };
}
```

## 🔌 API Development

### Controller Pattern
```javascript
// Good: Async/await with proper error handling
const getProducts = async (req, res) => {
  try {
    const { category, brand, sortBy } = req.query;
    
    // Build filter object
    const filters = {};
    if (category) filters.category = { $in: category.split(',') };
    if (brand) filters.brand = { $in: brand.split(',') };
    
    // Build sort object
    const sort = {};
    switch (sortBy) {
      case 'price-lowtohigh':
        sort.price = 1;
        break;
      case 'price-hightolow':
        sort.price = -1;
        break;
      default:
        sort.createdAt = -1;
    }
    
    const products = await Product.find(filters).sort(sort);
    
    res.status(200).json({
      success: true,
      data: products,
      count: products.length
    });
  } catch (error) {
    console.error('Error fetching products:', error);
    res.status(500).json({
      success: false,
      message: 'Failed to fetch products'
    });
  }
};
```

### Middleware Development
```javascript
// Validation middleware
const validateProductData = (req, res, next) => {
  const { title, price, category, brand } = req.body;
  const errors = [];

  if (!title || title.trim().length === 0) {
    errors.push('Title is required');
  }

  if (!price || price <= 0) {
    errors.push('Valid price is required');
  }

  if (errors.length > 0) {
    return res.status(400).json({
      success: false,
      message: 'Validation failed',
      errors
    });
  }

  next();
};

// Role-based access control
const requireAdmin = (req, res, next) => {
  if (req.user.role !== 'admin') {
    return res.status(403).json({
      success: false,
      message: 'Admin access required'
    });
  }
  next();
};
```

## 🔄 Data Flow Patterns

### Frontend Data Flow
```
User Action → Component → Redux Action → API Call → Response → Redux State → Component Update
```

### Example: Adding Product to Cart
```javascript
// 1. User clicks "Add to Cart" button
const handleAddToCart = () => {
  dispatch(addToCart({ userId, productId, quantity: 1 }));
};

// 2. Redux action dispatched
export const addToCart = createAsyncThunk(
  'cart/addToCart',
  async ({ userId, productId, quantity }) => {
    const response = await axios.post('/api/shop/cart/add', {
      userId, productId, quantity
    });
    return response.data;
  }
);

// 3. API endpoint processes request
const addToCart = async (req, res) => {
  // Controller logic here
};

// 4. State updated, component re-renders
const { cartItems, loading } = useSelector(state => state.cart);
```

## 🛠️ Common Development Tasks

### Adding a New Feature

1. **Plan the Feature**
   - Define requirements
   - Design database schema changes
   - Plan API endpoints
   - Design UI components

2. **Backend Development**
   ```javascript
   // 1. Create/update model
   const FeatureSchema = new mongoose.Schema({
     // schema definition
   });

   // 2. Create controller
   const createFeature = async (req, res) => {
     // controller logic
   };

   // 3. Add routes
   router.post('/create', authMiddleware, createFeature);
   ```

3. **Frontend Development**
   ```javascript
   // 1. Create Redux slice
   const featureSlice = createSlice({
     // slice definition
   });

   // 2. Create components
   function FeatureComponent() {
     // component logic
   }

   // 3. Add routes
   <Route path="/feature" element={<FeatureComponent />} />
   ```

### Debugging Techniques

#### Frontend Debugging
```javascript
// Redux DevTools
// Install Redux DevTools browser extension

// Console debugging
console.log('Current state:', useSelector(state => state));

// React Developer Tools
// Install React Developer Tools browser extension

// Network debugging
// Use browser Network tab to inspect API calls
```

#### Backend Debugging
```javascript
// Logging middleware
app.use((req, res, next) => {
  console.log(`${req.method} ${req.path}`, req.body);
  next();
});

// Error logging
app.use((error, req, res, next) => {
  console.error('Error:', error);
  res.status(500).json({
    success: false,
    message: 'Internal server error'
  });
});
```

## 📊 Performance Optimization

### Frontend Optimization
```javascript
// Lazy loading components
const AdminDashboard = lazy(() => import('./pages/admin-view/dashboard'));

// Memoization for expensive calculations
const expensiveValue = useMemo(() => {
  return products.reduce((sum, product) => sum + product.price, 0);
}, [products]);

// Debounced search
const debouncedSearch = useCallback(
  debounce((searchTerm) => {
    dispatch(searchProducts(searchTerm));
  }, 300),
  [dispatch]
);
```

### Backend Optimization
```javascript
// Database query optimization
const products = await Product.find(filters)
  .select('title price image category brand')
  .limit(20)
  .lean(); // Returns plain objects instead of Mongoose documents

// Caching (with Redis)
const getCachedProducts = async (cacheKey) => {
  const cached = await redis.get(cacheKey);
  if (cached) return JSON.parse(cached);
  
  const products = await Product.find({});
  await redis.setex(cacheKey, 3600, JSON.stringify(products)); // Cache for 1 hour
  return products;
};
```

## 🔧 Development Tools

### Useful npm Scripts
```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "lint": "eslint src --ext .js,.jsx",
    "lint:fix": "eslint src --ext .js,.jsx --fix",
    "format": "prettier --write src/**/*.{js,jsx}",
    "test": "jest",
    "test:watch": "jest --watch"
  }
}
```

### Git Workflow
```bash
# Feature development workflow
git checkout -b feature/new-feature
# Make changes
git add .
git commit -m "feat: add new feature"
git push origin feature/new-feature
# Create pull request
```

### Commit Message Convention
```
feat: add new feature
fix: resolve bug in cart calculation
docs: update API documentation
style: format code with prettier
refactor: reorganize component structure
test: add unit tests for auth
```

## 🐛 Common Issues & Solutions

### Frontend Issues

**Issue: Component not re-rendering**
```javascript
// Problem: Mutating state directly
state.items.push(newItem); // ❌

// Solution: Create new state
state.items = [...state.items, newItem]; // ✅
```

**Issue: Infinite re-renders**
```javascript
// Problem: Missing dependency array
useEffect(() => {
  fetchData();
}); // ❌

// Solution: Add dependency array
useEffect(() => {
  fetchData();
}, [dependency]); // ✅
```

### Backend Issues

**Issue: CORS errors**
```javascript
// Solution: Proper CORS configuration
app.use(cors({
  origin: process.env.FRONTEND_URL,
  credentials: true
}));
```

**Issue: Memory leaks**
```javascript
// Problem: Not closing database connections
// Solution: Proper connection management
process.on('SIGINT', async () => {
  await mongoose.connection.close();
  process.exit(0);
});
```

## 📈 Code Quality

### ESLint Configuration
```json
{
  "extends": [
    "react-app",
    "react-app/jest"
  ],
  "rules": {
    "no-unused-vars": "warn",
    "no-console": "warn",
    "prefer-const": "error"
  }
}
```

### Prettier Configuration
```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2
}
```

## 🚀 Advanced Topics

### Custom Hooks Development
```javascript
// API hook with caching
function useApiData(endpoint, dependencies = []) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let cancelled = false;
    
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await api.get(endpoint);
        if (!cancelled) {
          setData(response.data);
          setError(null);
        }
      } catch (err) {
        if (!cancelled) {
          setError(err.message);
        }
      } finally {
        if (!cancelled) {
          setLoading(false);
        }
      }
    };

    fetchData();

    return () => {
      cancelled = true;
    };
  }, dependencies);

  return { data, loading, error };
}
```

### Higher-Order Components
```javascript
// HOC for authentication
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const { isAuthenticated, loading } = useSelector(state => state.auth);
    
    if (loading) return <Spinner />;
    if (!isAuthenticated) return <Navigate to="/auth/login" />;
    
    return <WrappedComponent {...props} />;
  };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);
```

## 📚 Learning Resources

### React & Redux
- [React Official Docs](https://reactjs.org/docs)
- [Redux Toolkit Documentation](https://redux-toolkit.js.org/)
- [React Router Documentation](https://reactrouter.com/)

### Node.js & Express
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [Node.js Best Practices](https://github.com/goldbergyoni/nodebestpractices)

### Styling & UI
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [shadcn/ui Components](https://ui.shadcn.com/docs)
- [Lucide Icons](https://lucide.dev/)

## 🤝 Contributing Guidelines

### Code Review Checklist
- [ ] Code follows project conventions
- [ ] No console.log statements in production code
- [ ] Proper error handling implemented
- [ ] Components are properly tested
- [ ] Documentation is updated
- [ ] No security vulnerabilities introduced

### Pull Request Template
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Manual testing completed
- [ ] No console errors

## Screenshots (if applicable)
Add screenshots for UI changes
```

This developer guide provides the foundation for maintaining and extending the e-commerce application. Follow these patterns and guidelines to ensure code quality and maintainability.
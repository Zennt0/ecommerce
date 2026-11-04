# 🛒 EverGreen Store — Full-Stack E-Commerce Platform

> A modern, full-stack e-commerce web application featuring secure JWT authentication, Stripe checkout integration, shopping cart and coupon management, Redis caching, Cloudinary media storage, and a comprehensive admin analytics dashboard.

---

## ✨ Key Features

### 🛍️ Customer Experience
- **Interactive Product Catalog**: Browse products by category (t-shirts, jeans, shoes, glasses, jackets, suits, bags) with rich cards and dynamic status badges.
- **Featured Products Carousel**: Dynamic showcase of highlighted items powered by Framer Motion animations.
- **Smart Recommendations**: "People Also Bought" section recommending relevant items to shoppers.
- **Shopping Cart Management**: Add, remove, and adjust quantities in real-time with automatic order summary calculations.
- **Coupon & Discount System**: Enter and validate discount codes with instant percentage deductions before checkout.

### 💳 Payments & Checkout
- **Stripe Checkout Integration**: Seamless, secure payment processing with pre-filled customer email and order metadata.
- **Success & Cancellation Pages**: Dedicated checkout post-processing with celebratory confetti effect upon successful purchase and automatic cart clearance.

### 🔐 Authentication & Security
- **Dual-Token JWT Authentication**: Short-lived access tokens paired with long-lived refresh tokens stored securely in `httpOnly` cookies to protect against XSS attacks.
- **Redis Session Caching**: Refresh tokens stored and validated in Redis (Upstash) for instant revocation and low latency.
- **Role-Based Access Control (RBAC)**: Distinct authorization middleware for `customer` and `admin` roles, protecting administrative endpoints and dashboard routes.
- **Password Hashing**: Secure password encryption using `bcryptjs`.

### 📊 Admin Dashboard (`/secret-dashboard`)
- **Product Management**: Create new products with drag-and-drop / file upload to Cloudinary, view all listings, toggle featured statuses, and delete items.
- **Sales & Revenue Analytics**: Interactive charts powered by Recharts displaying total revenue, total sales count, active user count, and 7-day daily sales trends.

---

## 🛠️ Tech Stack

### Frontend
| Technology | Role |
| :--- | :--- |
| **React 19** | Modern declarative UI component library |
| **Vite** | Blazing-fast build tool and dev server |
| **React Router v7** | Client-side routing and navigation guards |
| **Zustand** | Lightweight, reactive state management stores |
| **Tailwind CSS** | Utility-first styling with custom glassmorphism effects |
| **Framer Motion** | Fluid animations and transitions |
| **Recharts** | Data visualization for admin sales & revenue analytics |
| **Stripe.js** | Client-side payment redirection and handling |
| **Lucide React** | Modern, clean iconography |
| **React Hot Toast** | Instant, non-intrusive feedback notifications |

### Backend
| Technology | Role |
| :--- | :--- |
| **Node.js & Express.js** | Scalable REST API architecture and route handling |
| **MongoDB & Mongoose** | Primary document database for users, products, orders, and coupons |
| **Redis (Upstash)** | High-speed caching for featured products and refresh token validation |
| **Stripe Node SDK** | Server-side checkout session creation and order fulfillment |
| **Cloudinary** | Cloud-based media storage and CDN optimization for product images |
| **JSON Web Tokens (JWT)** | Stateless authentication with access & refresh token pair |

---

## 📁 Project Architecture

```
e-commerce/
├── backend/
│   ├── server.js                # Server entry point & middleware mounting
│   ├── controllers/
│   │   ├── auth.controller.js       # Signup, login, logout, refresh token, profile
│   │   ├── product.controller.js    # CRUD, featured, recommendations, categories
│   │   ├── cart.controller.js       # Cart item operations
│   │   ├── coupon.controller.js     # Coupon retrieval & validation
│   │   ├── payment.controller.js    # Stripe checkout session & verification
│   │   └── analytics.controller.js  # Revenue & order analytics aggregation
│   ├── middleware/
│   │   └── auth.middleware.js       # protectRoute & adminRoute guards
│   ├── models/
│   │   ├── user.model.js            # User schema & password hashing
│   │   ├── product.model.js         # Product schema
│   │   ├── order.model.js           # Order record schema
│   │   └── coupon.model.js          # Discount coupon schema
│   ├── routes/                      # API endpoint definitions
│   └── lib/                         # DB, Redis, Stripe, Cloudinary connectors
│
├── frontend/
│   ├── src/
│   │   ├── components/              # UI components (Navbar, CartItem, AnalyticsTab, etc.)
│   │   ├── pages/                   # Route views (Homepage, CartPage, AdminPage, etc.)
│   │   ├── stores/                  # Zustand stores (useUserStore, useProductStore, useCartStore)
│   │   └── lib/                     # Axios instance & HTTP utilities
│   ├── public/                      # Static assets & screenshots
│   └── package.json
│
├── .env.example                     # Environment template
└── package.json                     # Root orchestrator scripts
```

---

## 🔌 API Endpoints Summary

| Group | Method | Endpoint | Auth Required | Description |
| :--- | :--- | :--- | :--- | :--- |
| **Auth** | `POST` | `/api/auth/signup` | Public | Register new customer |
| | `POST` | `/api/auth/login` | Public | Authenticate user & issue cookies |
| | `POST` | `/api/auth/logout` | Public | Invalidate cookies & clear session |
| | `POST` | `/api/auth/refresh-token` | Public | Exchange refresh token for new access token |
| | `GET` | `/api/auth/profile` | Protected | Retrieve authenticated user profile |
| **Products** | `GET` | `/api/products` | Admin | Retrieve all products |
| | `GET` | `/api/products/featured` | Public | Get featured products (Redis cached) |
| | `GET` | `/api/products/category/:category` | Public | Filter products by category |
| | `GET` | `/api/products/recommendations` | Public | Get sample recommendations |
| | `POST` | `/api/products` | Admin | Create product with image upload |
| | `PATCH` | `/api/products/:id` | Admin | Toggle featured status |
| | `DELETE` | `/api/products/:id` | Admin | Delete product and Cloudinary image |
| **Cart** | `GET` | `/api/cart` | Protected | Get items in user's cart |
| | `POST` | `/api/cart` | Protected | Add item to cart |
| | `PUT` | `/api/cart/:id` | Protected | Update item quantity |
| | `DELETE` | `/api/cart` | Protected | Clear entire cart |
| **Coupons** | `GET` | `/api/coupons` | Protected | Get active user coupon |
| | `POST` | `/api/coupons/validate` | Protected | Validate coupon code |
| **Payments** | `POST` | `/api/payments/create-checkout-session` | Protected | Initialize Stripe checkout |
| | `POST` | `/api/payments/checkout-success` | Protected | Process order upon successful payment |
| **Analytics**| `GET` | `/api/analytics` | Admin | Fetch sales count, revenue & chart data |

---

## 🚀 Getting Started

### 1. Prerequisites
- **Node.js** (v18 or higher recommended)
- **MongoDB** database instance (Atlas or local)
- **Redis** database instance (Upstash recommended)
- **Stripe** account (for API secret & publishable keys)
- **Cloudinary** account (for product image hosting)

### 2. Clone the Repository
```bash
git clone https://github.com/Iano023/ecommerce.git
cd ecommerce
```

### 3. Configure Environment Variables
Create a `.env` file in the root directory based on `.env.example`:
```bash
PORT=5000
MONGO_URI=your_mongodb_connection_string
UPSTASH_REDIS_URL=your_upstash_redis_url
ACCESS_TOKEN_SECRET=your_jwt_access_token_secret
REFRESH_TOKEN_SECRET=your_jwt_refresh_token_secret
STRIPE_SECRET_KEY=your_stripe_secret_key
CLOUDINARY_CLOUD_NAME=your_cloudinary_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
NODE_ENV=development
```

And configure frontend environment in `frontend/.env`:
```bash
VITE_STRIPE_KEY=your_stripe_publishable_key
```

### 4. Install Dependencies
```bash
# Install root/backend dependencies
npm install

# Install frontend dependencies
npm install --prefix frontend
```

### 5. Run the Application

#### Development Mode:
Run backend and frontend concurrently:
```bash
# Terminal 1: Backend
npm run dev

# Terminal 2: Frontend
cd frontend
npm run dev
```

#### Production Build & Run:
```bash
npm run build
npm start
```


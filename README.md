# 🍣 Temaki Box Builder

A full-stack web application for ordering customizable temaki (hand roll) boxes with ingredient selection, pre-made combos, and comprehensive order management.

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Tech Stack](#tech-stack)
- [Features Roadmap](#features-roadmap)
- [Setup Instructions](#setup-instructions)
- [API Documentation](#api-documentation)
- [Database Schema](#database-schema)
- [Development Guidelines](#development-guidelines)
- [Deployment](#deployment)

## 🎯 Project Overview

Temaki Box Builder is an e-commerce platform specializing in customizable Japanese hand roll boxes. Users can create personalized boxes with at least 5 rolls, choose from pre-made combos, and manage their orders through a complete cart and checkout system.

### Key Business Rules
- **Minimum Order**: Each box must contain at least 5 temaki rolls
- **Maximum Order**: Each box is limited to 10 rolls maximum
- **Custom Builder**: Users select ingredients and sauces for each roll
- **Pre-made Options**: Curated combo boxes for quick ordering

## 🛠 Tech Stack

### Frontend
- **Framework**: React 18+ with TypeScript
- **Styling**: Tailwind CSS
- **State Management**: Redux Toolkit or Zustand
- **Forms**: React Hook Form with Zod validation
- **HTTP Client**: Axios or React Query

### Backend
- **Runtime**: Node.js with Express.js
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: JWT tokens
- **Payments**: Stripe integration
- **File Storage**: AWS S3 or Cloudinary (for images)

### DevOps
- **Containerization**: Docker
- **CI/CD**: GitHub Actions
- **Hosting**: Vercel (frontend) + Railway/Heroku (backend)
- **Database**: Supabase or ElephantSQL

## 🗺 Features Roadmap

### 🎯 CORE FEATURES (MVP)
**Priority: Phase 1 - Essential for launch**

#### Authentication System
- [ ] User registration with email validation
- [ ] Login/logout with JWT tokens
- [ ] Password reset functionality
- [ ] Protected routes middleware
- [ ] Session management

```javascript
// Example API endpoints
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
POST /api/auth/forgot-password
```

#### User Profiles
- [ ] Profile creation and editing
- [ ] Personal information storage
- [ ] Delivery address management
- [ ] Dietary preferences tracking

#### Menu Management
- [ ] Ingredients catalog with categories
- [ ] Sauce varieties listing
- [ ] Pre-made combo display
- [ ] Ingredient images and descriptions
- [ ] Basic filtering (vegetarian, spicy, etc.)

#### Temaki Box Builder
- [ ] Interactive roll customization interface
- [ ] Real-time roll counter (min 5, max 10)
- [ ] Ingredient selection per roll
- [ ] Sauce pairing options
- [ ] Visual roll preview
- [ ] Build validation before cart addition

#### Pre-made Combo System
- [ ] Curated combo box display
- [ ] Quick-add to cart functionality
- [ ] Combo customization options
- [ ] Combo rating display

#### Cart System
- [ ] Persistent cart storage in database
- [ ] Cart restoration on login
- [ ] Quantity management
- [ ] Cart total calculation
- [ ] Remove/modify items

#### Order Management
- [ ] Order placement workflow
- [ ] Order confirmation system
- [ ] Order history viewing
- [ ] Order status tracking
- [ ] Order details breakdown

### ⚡ ADVANCED FEATURES
**Priority: Phase 2 - Enhanced functionality**

#### Payment Integration
- [ ] Stripe payment processor setup
- [ ] Secure checkout flow
- [ ] Payment confirmation handling
- [ ] Transaction logging
- [ ] Refund capabilities

#### Search & Discovery
- [ ] Ingredient search functionality
- [ ] Auto-complete suggestions
- [ ] Search result highlighting
- [ ] Popular ingredients trending

#### Dynamic Pricing
- [ ] Ingredient-based pricing model
- [ ] Real-time total calculation
- [ ] Tax calculation integration
- [ ] Delivery fee management

#### Review System
- [ ] Combo rating system (1-5 stars)
- [ ] Written reviews with moderation
- [ ] Review aggregation and display
- [ ] User review history

#### Admin Panel
- [ ] Admin authentication system
- [ ] Ingredient CRUD operations
- [ ] Sauce management interface
- [ ] Combo creation/editing tools
- [ ] Order management dashboard
- [ ] User management system

#### Smart Ordering Features
- [ ] Build limit warnings and validation
- [ ] Multi-box ordering capability
- [ ] Bulk operations support

#### Enhanced Filtering
- [ ] Diet-based filtering (vegan, gluten-free)
- [ ] Spice level filtering
- [ ] Allergen information display
- [ ] Custom filter combinations

#### Nutritional Information
- [ ] Calorie tracking per ingredient
- [ ] Macro nutrient breakdown
- [ ] Total box nutrition summary
- [ ] Dietary goal tracking

#### Promotional System
- [ ] Promo code creation and management
- [ ] Discount calculation engine
- [ ] Usage tracking and limits
- [ ] Expired code handling

### 🚀 STRETCH GOALS
**Priority: Phase 3 - Advanced features**

#### User Experience Enhancements
- [ ] Favorites system for boxes/combos
- [ ] Custom box naming functionality
- [ ] Box sharing capabilities
- [ ] Reorder previous boxes

#### Location Services
- [ ] Geolocation integration
- [ ] Delivery time estimation
- [ ] Service area validation
- [ ] Location-based pricing

#### Subscription Service
- [ ] Recurring delivery setup
- [ ] Subscription management
- [ ] Pause/resume functionality
- [ ] Billing cycle management

#### Advanced Analytics
- [ ] Sales dashboard for admins
- [ ] Popular ingredients tracking
- [ ] Revenue analytics
- [ ] User behavior insights

#### Inventory Management
- [ ] Real-time stock tracking
- [ ] Out-of-stock notifications
- [ ] Automatic substitution suggestions
- [ ] Inventory alerts for admins

#### Enhanced Order Tracking
- [ ] Real-time order status updates
- [ ] Delivery timeline visualization
- [ ] SMS/email notifications
- [ ] Live delivery tracking

## 🚀 Setup Instructions

### Prerequisites
- Node.js 18+ and npm/yarn
- PostgreSQL 14+
- Git

### Development Setup

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/temaki-box-builder.git
cd temaki-box-builder
```

2. **Install dependencies**
```bash
# Backend
cd backend
npm install

# Frontend
cd ../frontend
npm install
```

3. **Environment setup**
```bash
# Backend .env
DATABASE_URL="postgresql://username:password@localhost:5432/temaki_db"
JWT_SECRET="your-super-secret-jwt-key"
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_PUBLISHABLE_KEY="pk_test_..."

# Frontend .env
REACT_APP_API_URL="http://localhost:3001"
REACT_APP_STRIPE_PUBLISHABLE_KEY="pk_test_..."
```

4. **Database setup**
```bash
cd backend
npx prisma migrate dev
npx prisma db seed
```

5. **Start development servers**
```bash
# Terminal 1 - Backend
cd backend
npm run dev

# Terminal 2 - Frontend
cd frontend
npm start
```

## 📊 Database Schema

### Core Tables

```sql
-- Users table
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email VARCHAR(255) UNIQUE NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  first_name VARCHAR(100),
  last_name VARCHAR(100),
  phone VARCHAR(20),
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW()
);

-- Ingredients table
CREATE TABLE ingredients (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(100) NOT NULL,
  category VARCHAR(50), -- 'fish', 'vegetable', 'protein', etc.
  price_cents INTEGER NOT NULL,
  calories_per_serving INTEGER,
  is_vegetarian BOOLEAN DEFAULT FALSE,
  is_vegan BOOLEAN DEFAULT FALSE,
  is_spicy BOOLEAN DEFAULT FALSE,
  contains_gluten BOOLEAN DEFAULT FALSE,
  image_url VARCHAR(255),
  description TEXT,
  is_available BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Sauces table
CREATE TABLE sauces (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name VARCHAR(100) NOT NULL,
  spice_level INTEGER DEFAULT 0, -- 0-5 scale
  price_cents INTEGER NOT NULL,
  is_vegetarian BOOLEAN DEFAULT TRUE,
  is_vegan BOOLEAN DEFAULT FALSE,
  description TEXT,
  is_available BOOLEAN DEFAULT TRUE
);

-- Boxes table
CREATE TABLE boxes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  name VARCHAR(200), -- custom box names
  total_price_cents INTEGER NOT NULL,
  status VARCHAR(50) DEFAULT 'draft', -- 'draft', 'ordered', 'preparing', 'delivered'
  created_at TIMESTAMP DEFAULT NOW()
);

-- Temaki rolls table
CREATE TABLE temaki_rolls (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  box_id UUID REFERENCES boxes(id) ON DELETE CASCADE,
  position INTEGER, -- order within box
  created_at TIMESTAMP DEFAULT NOW()
);

-- Roll ingredients (many-to-many)
CREATE TABLE roll_ingredients (
  roll_id UUID REFERENCES temaki_rolls(id) ON DELETE CASCADE,
  ingredient_id UUID REFERENCES ingredients(id),
  quantity INTEGER DEFAULT 1,
  PRIMARY KEY (roll_id, ingredient_id)
);

-- Roll sauces (many-to-many)
CREATE TABLE roll_sauces (
  roll_id UUID REFERENCES temaki_rolls(id) ON DELETE CASCADE,
  sauce_id UUID REFERENCES sauces(id),
  PRIMARY KEY (roll_id, sauce_id)
);

-- Orders table
CREATE TABLE orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id),
  total_amount_cents INTEGER NOT NULL,
  stripe_payment_intent_id VARCHAR(255),
  status VARCHAR(50) DEFAULT 'pending',
  delivery_address TEXT,
  special_instructions TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Order boxes (many-to-many)
CREATE TABLE order_boxes (
  order_id UUID REFERENCES orders(id),
  box_id UUID REFERENCES boxes(id),
  quantity INTEGER DEFAULT 1,
  PRIMARY KEY (order_id, box_id)
);
```

## 📡 API Documentation

### Authentication Endpoints
```javascript
POST   /api/auth/register     // User registration
POST   /api/auth/login        // User login
POST   /api/auth/logout       // User logout
GET    /api/auth/profile      // Get user profile
PUT    /api/auth/profile      // Update user profile
```

### Menu Endpoints
```javascript
GET    /api/ingredients       // List all ingredients
GET    /api/ingredients/:id   // Get ingredient details
GET    /api/sauces           // List all sauces
GET    /api/combos           // List pre-made combos
```

### Box Builder Endpoints
```javascript
POST   /api/boxes            // Create new box
GET    /api/boxes/:id        // Get box details
PUT    /api/boxes/:id        // Update box
DELETE /api/boxes/:id        // Delete box
POST   /api/boxes/:id/rolls  // Add roll to box
PUT    /api/rolls/:id        // Update roll
DELETE /api/rolls/:id        // Delete roll
```

### Cart & Orders
```javascript
GET    /api/cart             // Get user's cart
POST   /api/cart/add         // Add box to cart
PUT    /api/cart/update      // Update cart item
DELETE /api/cart/remove      // Remove from cart
POST   /api/orders           // Create order
GET    /api/orders           // Get order history
GET    /api/orders/:id       // Get order details
```

### Admin Endpoints
```javascript
// Ingredients management
GET    /api/admin/ingredients
POST   /api/admin/ingredients
PUT    /api/admin/ingredients/:id
DELETE /api/admin/ingredients/:id

// Orders management
GET    /api/admin/orders
PUT    /api/admin/orders/:id/status

// Analytics
GET    /api/admin/stats/sales
GET    /api/admin/stats/popular-ingredients
```

## 👨‍💻 Development Guidelines

### Code Structure
```
/frontend
  /src
    /components     # Reusable UI components
    /pages         # Route components
    /hooks         # Custom React hooks
    /store         # Redux/Zustand store
    /utils         # Helper functions
    /types         # TypeScript type definitions

/backend
  /src
    /controllers   # Route handlers
    /middleware    # Express middleware
    /models        # Database models (Prisma)
    /routes        # API route definitions
    /services      # Business logic
    /utils         # Helper functions
    /types         # TypeScript types
```

### Development Workflow

1. **Feature Development**
   - Create feature branch from `main`
   - Implement feature following TDD approach
   - Write tests for new functionality
   - Update documentation if needed

2. **Code Standards**
   - Use TypeScript for type safety
   - Follow ESLint configuration
   - Write meaningful commit messages
   - Add JSDoc comments for functions

3. **Testing Strategy**
   - Unit tests for utilities and services
   - Integration tests for API endpoints
   - Component testing with React Testing Library
   - E2E tests for critical user flows

### Git Workflow
```bash
# Feature development
git checkout -b feature/temaki-box-builder
git commit -m "feat: add roll customization interface"
git push origin feature/temaki-box-builder

# Create pull request for review
```

## 🚀 Deployment

### Production Environment Setup

1. **Database Setup**
   - Use managed PostgreSQL service
   - Run production migrations
   - Set up database backups

2. **Backend Deployment**
   - Configure environment variables
   - Set up SSL certificates
   - Configure CORS for production domain
   - Set up monitoring and logging

3. **Frontend Deployment**
   - Build optimized production bundle
   - Configure CDN for static assets
   - Set up error tracking (Sentry)

4. **CI/CD Pipeline**
   - Automated testing on PR creation
   - Automated deployment on main branch merge
   - Database migration automation
   - Health check monitoring

### Environment Variables
```bash
# Production backend
DATABASE_URL="postgresql://..."
JWT_SECRET="production-secret"
STRIPE_SECRET_KEY="sk_live_..."
NODE_ENV="production"
PORT="3001"

# Production frontend
REACT_APP_API_URL="https://api.temakibox.com"
REACT_APP_STRIPE_PUBLISHABLE_KEY="pk_live_..."
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes with clear messages
4. Push to your branch
5. Create a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For questions or support, please create an issue in the GitHub repository or contact the development team.

---

**Happy coding! 🍣✨**

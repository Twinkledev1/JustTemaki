# 🍣 JustTemaki

A full-stack web application for ordering customizable temaki (hand roll) boxes with ingredient selection, pre-made combos, and comprehensive order management.

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Tech Stack](#-tech-stack)
- [Features Roadmap](#-features-roadmap)
- [Setup Instructions](#-setup-instructions)
- [API Documentation](#-api-documentation)
- [Database Schema](#-database-schema)
- [Development Guidelines](#-development-guidelines)
- [Deployment](#-deployment)

## 🎯 Project Overview

JustTemaki is an e-commerce platform specializing in customizable Japanese hand roll boxes. Users can create personalized boxes with at least 5 rolls, choose from pre-made combos, and manage their orders through a complete cart and checkout system.

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
- **Database**: PostgreSQL/MongoDb
- **Authentication**: JWT tokens
- **Payments**: Stripe integration
- **File Storage**: AWS S3 or Cloudinary (for images)

### DevOps
- **CI/CD**: GitHub Actions
- **Hosting**: Render 
- **Database**: Postgres Aiven or MongoDb Atlas

## 🗺 Features Roadmap

### 🎯 CORE FEATURES (MVP)
**Priority: Phase 1 - Essential for launch**

#### Authentication System
```javascript
// Example API endpoints
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
POST /api/auth/forgot-password
```

## 🚀 Setup Instructions

### Prerequisites
- Node.js 18+ and npm/yarn
- PostgreSQL 14+
- Git

### Development Setup

1. **Clone the repository**
```bash
git clone git@github.com:Twinkledev1/JustTemaki.git
cd JustTemaki
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

#### Dummy ER Diagram

```mermaid
erDiagram
    users {
        uuid id PK
        varchar email UK
        varchar password_hash
        varchar first_name
        varchar last_name
        varchar phone
        timestamp created_at
        timestamp updated_at
    }

    ingredients {
        uuid id PK
        varchar name
        varchar category
        integer price_cents
        integer calories_per_serving
        boolean is_vegetarian
        boolean is_vegan
        boolean is_spicy
        boolean contains_gluten
        varchar image_url
        text description
        boolean is_available
        timestamp created_at
    }

    sauces {
        uuid id PK
        varchar name
        integer spice_level
        integer price_cents
        boolean is_vegetarian
        boolean is_vegan
        text description
        boolean is_available
    }

    boxes {
        uuid id PK
        uuid user_id FK
        varchar name
        integer total_price_cents
        varchar status
        timestamp created_at
    }

    temaki_rolls {
        uuid id PK
        uuid box_id FK
        integer position
        timestamp created_at
    }

    roll_ingredients {
        uuid roll_id FK
        uuid ingredient_id FK
        integer quantity
    }

    roll_sauces {
        uuid roll_id FK
        uuid sauce_id FK
    }

    orders {
        uuid id PK
        uuid user_id FK
        integer total_amount_cents
        varchar stripe_payment_intent_id
        varchar status
        text delivery_address
        text special_instructions
        timestamp created_at
    }

    order_boxes {
        uuid order_id FK
        uuid box_id FK
        integer quantity
    }

    %% Relationships
    users ||--o{ boxes : "creates"
    users ||--o{ orders : "places"
    
    boxes ||--o{ temaki_rolls : "contains"
    boxes ||--o{ order_boxes : "included_in"
    
    temaki_rolls ||--o{ roll_ingredients : "has"
    temaki_rolls ||--o{ roll_sauces : "has"
    
    ingredients ||--o{ roll_ingredients : "used_in"
    sauces ||--o{ roll_sauces : "used_in"
    
    orders ||--o{ order_boxes : "contains"
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

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
- **Framework**: React 18+ with Java Script (Vanilla JS)
- **Styling**:##: CSS3
- **Markup**: HTML5 
- **Forms**: React Hook Form with Zod validation
- **HTTP Client**: Axios or React Query

### Backend
- **Runtime**: Node.js with Express.js
- **Database**: PostgreSQL/MongoDb
- **Authentication**: JWT tokens
- **Payments**: Stripe integration

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
- Node.js 18+ and npm
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





**Happy coding! 🍣✨**

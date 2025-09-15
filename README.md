# Notes SaaS Application

<!-- Developed by Devanshu Pathak -->
<!-- A comprehensive multi-tenant notes management system with React frontend and Node.js backend -->

## Overview

A full-stack multi-tenant Software-as-a-Service (SaaS) application for notes management, built with React frontend and Node.js backend. This application demonstrates enterprise-grade multi-tenancy implementation with strict data isolation between tenants.

**Developer:** Devanshu Pathak  
**Architecture:** Full-stack JavaScript (MERN Stack)  
**Multi-tenancy Approach:** Shared Schema with Tenant ID Column  

## ✨ Features

### Core Functionality
- 📝 **Notes Management**: Create, read, update, and delete notes
- 🔐 **Authentication & Authorization**: JWT-based auth with role-based access control
- 👥 **User Management**: Admin and member roles with different permissions
- 📧 **User Invitations**: Invite users to join tenants with email-based workflow
- 🔍 **Search & Pagination**: Full-text search across notes with paginated results
- 📱 **Responsive Design**: Mobile-first responsive UI built with Tailwind CSS

### Multi-Tenant Features
- 🏢 **Tenant Isolation**: Complete data separation between tenants
- 🎯 **Role-Based Access**: Admin and member roles with appropriate permissions
- 💳 **Subscription Plans**: Free and Pro plans with usage limits
- 🚀 **Scalable Architecture**: Designed to handle multiple tenants efficiently

### Security Features
- 🔒 **Data Isolation**: Strict tenant-level data separation
- 🛡️ **Input Validation**: Comprehensive validation using Joi
- 🚫 **Rate Limiting**: API rate limiting to prevent abuse
- 🔐 **Password Security**: Bcrypt hashing with salt rounds
- 🌐 **CORS Protection**: Cross-origin request security

## 🏗️ Multi-Tenancy Architecture

### Current Implementation: Shared Schema with Tenant ID

This application implements the **Shared Schema with Tenant ID** approach for multi-tenancy, designed and developed by **Devanshu Pathak**.

#### Why This Approach?

1. **Cost Efficiency**: Single database and schema for all tenants
2. **Maintenance Simplicity**: One codebase, one database to maintain
3. **Resource Optimization**: Efficient resource utilization
4. **Scalability**: Easy to scale horizontally

#### Implementation Details

```javascript
// Every data model includes tenantId for isolation
const noteSchema = new mongoose.Schema({
  title: String,
  content: String,
  userId: ObjectId,
  tenantId: ObjectId,  // ← Tenant isolation key
  // ... other fields
});

// Compound indexes ensure performance and isolation
noteSchema.index({ tenantId: 1, userId: 1 });
noteSchema.index({ tenantId: 1, createdAt: -1 });
```

#### Data Isolation Strategy

1. **Schema Level**: All models include `tenantId` field
2. **Query Level**: All database queries filter by `tenantId`
3. **Middleware Level**: Authentication middleware injects tenant context
4. **API Level**: All routes validate tenant access

#### Supported Tenants

- **Acme Corporation** (`acme`) - Demonstration tenant
- **Globex Corporation** (`globex`) - Demonstration tenant

Additional tenants can be easily added through the tenant management system.

## 🏢 Alternative Multi-Tenancy Approaches

While this implementation uses **Shared Schema with Tenant ID**, here are other approaches considered:

### 1. Schema-per-Tenant
- **Pros**: Better isolation, easier backup/restore per tenant
- **Cons**: More complex maintenance, higher resource usage
- **Use Case**: When tenants have very different data requirements

### 2. Database-per-Tenant
- **Pros**: Complete isolation, easier compliance, independent scaling
- **Cons**: Highest maintenance overhead, resource intensive
- **Use Case**: When strict compliance or complete isolation is required

### 3. Hybrid Approach
- **Pros**: Combines benefits of multiple approaches
- **Cons**: Most complex implementation
- **Use Case**: Large enterprise applications with varied tenant needs

## 🚀 Quick Start

### Prerequisites

- Node.js 16+ 
- MongoDB 4.4+
- npm or yarn

### Backend Setup

```bash
# Navigate to backend directory
cd notes-backend

# Install dependencies
npm install

# Create environment file
cp .env.example .env

# Configure environment variables
# MONGODB_URI=mongodb://localhost/notes-saas
# JWT_SECRET=your-secret-key
# NODE_ENV=development

# Seed the database with sample data
npm run seed

# Start development server
npm run dev
```

### Frontend Setup

```bash
# Navigate to frontend directory  
cd notes-frontend

# Install dependencies
npm install

# Start development server
npm start
```

## 📋 Test Accounts

The seeding script creates the following test accounts:

### Acme Corporation Tenant
- **Admin**: `admin@acme.test` (password: `password`)
- **Member**: `user@acme.test` (password: `password`)

### Globex Corporation Tenant  
- **Admin**: `admin@globex.test` (password: `password`)
- **Member**: `user@globex.test` (password: `password`)

### Additional Test Accounts
- `member1@example.com` (Acme tenant)
- `member2@example.com` (Globex tenant)

## 🛠️ Technology Stack

### Backend
- **Runtime**: Node.js 16+
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT (JSON Web Tokens)
- **Password Hashing**: bcryptjs
- **Validation**: Joi
- **Security**: Helmet, CORS, Rate Limiting

### Frontend
- **Framework**: React 18
- **Routing**: React Router v6
- **Styling**: Tailwind CSS
- **Icons**: Heroicons
- **HTTP Client**: Axios
- **UI Components**: Headless UI

### Development Tools
- **Backend**: Nodemon for development
- **Frontend**: Create React App
- **Version Control**: Git

## 📁 Project Structure

```
notes-saas-app/
├── notes-backend/              # Backend API server
│   ├── models/                 # Mongoose data models
│   │   ├── User.js            # User model with tenant association
│   │   ├── Tenant.js          # Tenant model
│   │   ├── Note.js            # Note model with tenant isolation
│   │   └── Invitation.js      # User invitation model
│   ├── routes/                # Express route handlers
│   │   ├── auth.js            # Authentication routes
│   │   ├── notes.js           # Notes CRUD operations
│   │   ├── users.js           # User management
│   │   └── tenants.js         # Tenant operations
│   ├── middleware/            # Custom middleware
│   │   ├── auth.js            # JWT authentication
│   │   └── subscription.js    # Plan limits enforcement
│   ├── scripts/               # Database utilities
│   │   ├── seedData.js        # Sample data creation
│   │   └── clearDatabase.js   # Database cleanup
│   └── index.js               # Application entry point
├── notes-frontend/            # React frontend application
│   ├── src/
│   │   ├── components/        # Reusable React components
│   │   ├── pages/             # Page components
│   │   ├── contexts/          # React contexts
│   │   └── services/          # API service layer
│   └── public/                # Static assets
└── README.md                  # This documentation file
```

## 🔐 API Endpoints

### Authentication
- `POST /auth/login` - User login
- `POST /auth/register` - User registration (admin only)
- `GET /auth/me` - Get current user info

### Notes Management
- `GET /notes` - List notes (with search & pagination)
- `GET /notes/:id` - Get specific note
- `POST /notes` - Create new note
- `PUT /notes/:id` - Update note
- `DELETE /notes/:id` - Delete note

### User Management
- `GET /users` - List tenant users (admin only)
- `POST /users/invite` - Invite user (admin only)
- `GET /users/invitations` - List invitations
- `DELETE /users/invitations/:id` - Cancel invitation

### Tenant Management
- `GET /tenants/current` - Get current tenant info
- `PUT /tenants/upgrade` - Upgrade tenant plan

## 🛡️ Security Measures

### Multi-Tenant Security
- **Tenant ID Validation**: All queries filtered by tenant ID
- **Request Context**: Tenant context injected via middleware
- **Access Control**: Role-based permissions within tenants
- **Data Isolation**: No cross-tenant data access possible

### General Security
- **JWT Authentication**: Secure token-based authentication
- **Password Hashing**: bcrypt with salt rounds
- **Input Validation**: Joi schema validation
- **Rate Limiting**: API call frequency limits
- **CORS Protection**: Cross-origin request filtering
- **Helmet Security**: HTTP security headers

## 📊 Database Schema

### Core Models

```javascript
// Tenant Model
{
  name: String,           // Tenant display name
  slug: String,           // URL-friendly identifier
  plan: Enum,            // 'free' | 'pro'
  createdAt: Date,
  updatedAt: Date
}

// User Model  
{
  email: String,          // Unique user email
  password: String,       // Hashed password
  role: Enum,            // 'admin' | 'member'
  tenantId: ObjectId,    // Reference to tenant
  createdAt: Date,
  updatedAt: Date
}

// Note Model
{
  title: String,          // Note title (max 200 chars)
  content: String,        // Note content (max 10k chars)
  userId: ObjectId,       // Note owner
  tenantId: ObjectId,     // Tenant isolation
  createdAt: Date,
  updatedAt: Date
}
```

## 🚀 Deployment

### Backend Deployment (Vercel)
```bash
# Deploy backend to Vercel
cd notes-backend
vercel --prod
```

### Frontend Deployment (Vercel)
```bash
# Build and deploy frontend
cd notes-frontend
npm run build
vercel --prod
```

### Environment Variables

```bash
# Backend Environment
MONGODB_URI=mongodb+srv://...
JWT_SECRET=your-secret-key
NODE_ENV=production

# Frontend Environment  
REACT_APP_API_URL=https://your-backend-url.vercel.app
```

## 📈 Scalability Considerations

### Current Capacity
- **Database**: MongoDB with indexed tenant queries
- **Application**: Stateless Express.js servers
- **Frontend**: Static React deployment

### Scaling Strategies
1. **Horizontal Scaling**: Add more application instances
2. **Database Optimization**: Implement read replicas
3. **Caching**: Add Redis for session management
4. **CDN**: Static asset distribution
5. **Load Balancing**: Distribute traffic across instances

## 🧪 Testing

### Manual Testing
1. Use provided test accounts to verify tenant isolation
2. Test cross-tenant data access (should be blocked)
3. Verify role-based permissions
4. Test invitation workflow

### Test Scenarios
- ✅ Tenant data isolation
- ✅ User authentication and authorization  
- ✅ CRUD operations with proper tenant filtering
- ✅ Invitation system functionality
- ✅ Subscription plan limits enforcement

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Developer

**Devanshu Pathak**
- GitHub: [@DannYX786](https://github.com/DannYX786)
- Project: Multi-Tenant Notes SaaS Application

## 🔮 Future Enhancements

### Planned Features
- 📧 Email notifications for invitations
- 🔄 Real-time collaborative editing
- 📎 File attachments for notes
- 🏷️ Note categorization and tags
- 📊 Usage analytics dashboard
- 🌍 Multi-language support
- 📱 Mobile application
- 🔌 Third-party integrations (Slack, Teams)

### Technical Improvements
- 🚀 GraphQL API implementation
- 🗄️ Database sharding for massive scale
- ⚡ Redis caching layer
- 🔍 Elasticsearch for advanced search
- 🔄 Event-driven architecture
- 📈 Monitoring and observability tools

---

<!-- Built with ❤️ by Devanshu Pathak -->
<!-- Multi-tenant architecture designed for scalability and security -->

# IT Asset Management System

A comprehensive web-based IT Asset Management System built with **Angular** (frontend) and **Node.js/Express** (backend), designed to streamline asset tracking, request management, and assignment workflows in organizations.

## 🚀 Features

### Core Functionality
- **Asset Management**: Complete CRUD operations for IT assets
- **User Management**: Multi-role user system (Admin, Department Head, Employee)
- **Request Workflow**: Employee asset requests with approval hierarchy
- **Assignment Tracking**: Asset assignment and return management
- **Inventory Tracking**: Real-time asset availability and categorization
- **Authentication**: Secure JWT-based authentication with refresh tokens
- **Email Notifications**: Automated password reset emails
- **API Documentation**: Swagger/OpenAPI documentation

### User Roles & Permissions

#### **Administrator**
- Full access to all system features
- Create, update, and delete assets
- Manage user accounts
- View all requests and assignments
- Final approval authority for asset assignments
- Access to complete system analytics

#### **Department Head**
- Review and approve/reject employee requests from their department
- View department-specific asset assignments
- Forward approved requests to admin for asset allocation
- Monitor department asset usage

#### **Employee**
- Submit asset requests with justification
- View personal request status and history
- Request asset returns
- View assigned assets

## 🛠️ Technology Stack

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT (Access + Refresh tokens)
- **Password Hashing**: bcryptjs
- **Email Service**: Nodemailer
- **API Documentation**: Swagger (swagger-jsdoc, swagger-ui-express)
- **Security**: CORS, cookie-parser
- **Environment**: dotenv

### Frontend
- **Framework**: Angular 18
- **UI Library**: Angular Material
- **Language**: TypeScript
- **Styling**: CSS3
- **Build Tool**: Angular CLI

## 📋 Prerequisites

Before running this application, ensure you have:

- **Node.js** (v16 or higher)
- **npm** (v8 or higher)
- **MongoDB** (local installation or MongoDB Atlas account)
- **Git** (for cloning the repository)

## ⚙️ Installation & Setup

### 1. Clone the Repository
```bash
git clone <repository-url>
cd it-asset-management
```

### 2. Backend Setup

#### Navigate to Backend Directory
```bash
cd Backend
```

#### Install Dependencies
```bash
npm install
```

#### Configure Environment Variables
Create a `.env` file in the `Backend` directory with the following variables:

```env
# Database Configuration
MONGO_URI=mongodb://localhost:27017/it-asset-management
# or for MongoDB Atlas:
# MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/it-asset-management

# Server Configuration
PORT=5000

# JWT Secrets (Use strong, unique values in production)
JWT_SECRET=your_super_secure_jwt_secret_here
REFRESH_TOKEN_SECRET=your_super_secure_refresh_secret_here

# Email Configuration (for password reset)
MAIL_USER=your-email@gmail.com
MAIL_PASS=your-app-password

# Frontend URL
FRONTEND_URL=http://localhost:4200
```

#### Start Backend Server
```bash
# Development mode with auto-restart
npm run dev

# or Production mode
npm start
```

The backend server will start on `http://localhost:5000`

### 3. Frontend Setup

#### Navigate to Frontend Directory
```bash
cd Frontend/it-asset-management
```

#### Install Dependencies
```bash
npm install
```

#### Start Development Server
```bash
ng serve
```

The frontend application will be available at `http://localhost:4200`

## 🏗️ Project Structure

```
it-asset-management/
├── Backend/
│   ├── config/
│   │   ├── db.js                 # MongoDB connection
│   │   └── swagger.js            # Swagger configuration
│   ├── controllers/
│   │   ├── assetController.js    # Asset CRUD operations
│   │   ├── authController.js     # Authentication logic
│   │   ├── userController.js     # User management
│   │   ├── requestController.js  # Request workflows
│   │   └── assignmentController.js # Assignment tracking
│   ├── middlewares/
│   │   └── authMiddleware.js     # JWT verification & role checking
│   ├── models/
│   │   ├── user.js              # User schema
│   │   ├── Asset.js             # Asset schema
│   │   ├── AssetCount.js        # Asset inventory tracking
│   │   ├── Request.js           # Request schema
│   │   └── Assignment.js        # Assignment schema
│   ├── routes/
│   │   ├── authRoutes.js        # Authentication routes
│   │   ├── userRoutes.js        # User management routes
│   │   ├── assetRoutes.js       # Asset management routes
│   │   ├── requestRoutes.js     # Request workflow routes
│   │   ├── assignmentRoutes.js  # Assignment routes
│   │   └── healthRoutes.js      # Health check endpoint
│   ├── services/
│   │   └── emailService.js      # Email notification service
│   ├── .env                     # Environment variables
│   ├── index.js                 # Server entry point
│   └── package.json
├── Frontend/
│   └── it-asset-management/
│       ├── src/
│       │   ├── app/
│       │   │   ├── components/   # Angular components
│       │   │   ├── services/     # Angular services
│       │   │   ├── guards/       # Route guards
│       │   │   └── models/       # TypeScript interfaces
│       │   ├── assets/          # Static assets
│       │   └── styles.css       # Global styles
│       ├── angular.json         # Angular configuration
│       └── package.json
└── README.md
```

## 🔧 Database Schema

### User Collection
```javascript
{
  name: String,
  email: String (unique),
  password: String (hashed),
  role: ['admin', 'depthead', 'employee'],
  department: String,
  isActive: Boolean,
  resetPasswordToken: String,
  resetPasswordExpires: Date,
  timestamps: true
}
```

### Asset Collection
```javascript
{
  asset_name: String,
  asset_id: String (unique),
  asset_category: String,
  condition: ['new', 'good', 'damaged'],
  description: String,
  isAssigned: Boolean,
  timestamps: true
}
```

### Request Collection
```javascript
{
  user: ObjectId (ref: User),
  asset_category: String,
  reason: String,
  status: ['pending', 'approved', 'rejected'],
  deptHeadRemarks: String,
  department: String,
  forwardedToAdmin: Boolean,
  adminRemarks: String,
  adminStatus: ['pending', 'approved', 'rejected'],
  completed: Boolean,
  timestamps: true
}
```

### Assignment Collection
```javascript
{
  request: ObjectId (ref: Request),
  asset_name: String,
  asset_id: ObjectId (ref: Asset),
  asset_category: String,
  assignedTo: ObjectId (ref: User),
  assignedBy: ObjectId (ref: User),
  assignedAt: Date,
  returnedAt: Date,
  returnRequested: Boolean,
  returnApprovedByDeptHead: Boolean,
  returnDeptHeadMessage: String,
  returnCondition: ['good', 'damaged'],
  returnFinalizedByAdmin: Boolean,
  timestamps: true
}
```

## 🌐 API Endpoints

### Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/refresh` - Refresh access token
- `POST /api/auth/logout` - User logout
- `POST /api/auth/forgot-password` - Request password reset
- `POST /api/auth/reset-password/:token` - Reset password

### User Management
- `POST /api/users` - Create user (Admin only)
- `GET /api/users` - Get all users (Admin only)
- `GET /api/users/:id` - Get user by ID
- `PUT /api/users/:id` - Update user
- `DELETE /api/users/:id` - Delete user (Admin only)

### Asset Management
- `POST /api/assets` - Create asset (Admin only)
- `GET /api/assets` - Get all assets
- `GET /api/assets/:id` - Get asset by ID
- `PUT /api/assets/:id` - Update asset (Admin only)
- `DELETE /api/assets/:id` - Delete asset (Admin only)
- `GET /api/assets/summary` - Get asset inventory summary
- `GET /api/assets/category/:category` - Get available assets by category

### Request Management
- `POST /api/requests` - Create request
- `GET /api/requests` - Get requests (filtered by role)
- `GET /api/requests/:id` - Get request by ID
- `PUT /api/requests/:id/approve` - Approve/reject request (Dept Head)
- `PUT /api/requests/:id/admin-action` - Admin action on request

### Assignment Management
- `POST /api/assignments` - Create assignment (Admin only)
- `GET /api/assignments` - Get assignments (filtered by role)
- `GET /api/assignments/:id` - Get assignment by ID
- `PUT /api/assignments/:id/return-request` - Request asset return
- `PUT /api/assignments/:id/approve-return` - Approve return (Dept Head)
- `PUT /api/assignments/:id/finalize-return` - Finalize return (Admin)

## 📚 API Documentation

The API documentation is automatically generated using Swagger and is available at:
```
http://localhost:5000/api-docs
```

## 🔒 Security Features

- **JWT Authentication**: Secure token-based authentication
- **Refresh Token**: Long-lived refresh tokens stored in HTTP-only cookies
- **Password Hashing**: bcrypt for secure password storage
- **Role-based Access Control**: Different permissions for different user roles
- **Input Validation**: Server-side validation for all inputs
- **CORS Protection**: Configured CORS for cross-origin requests
- **Environment Variables**: Sensitive data stored in environment variables

## 🚀 Deployment

### Backend Deployment (Node.js)

1. **Environment Setup**:
   ```bash
   # Set production environment variables
   NODE_ENV=production
   MONGO_URI=<production-mongodb-uri>
   JWT_SECRET=<strong-production-secret>
   REFRESH_TOKEN_SECRET=<strong-production-refresh-secret>
   ```

2. **Build & Start**:
   ```bash
   npm install --production
   npm start
   ```

### Frontend Deployment (Angular)

1. **Build for Production**:
   ```bash
   cd Frontend/it-asset-management
   ng build --configuration production
   ```

2. **Deploy**: The `dist/` folder contains the built application ready for deployment to any static hosting service.

### Deployment Platforms

- **Backend**: Heroku, Railway, DigitalOcean, AWS EC2
- **Frontend**: Netlify, Vercel, GitHub Pages
- **Database**: MongoDB Atlas (cloud)

## 🧪 Testing

### Backend Testing
```bash
cd Backend
npm test
```

### Frontend Testing
```bash
cd Frontend/it-asset-management
ng test
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🐛 Known Issues

- Email service requires proper SMTP configuration
- File upload functionality not yet implemented
- Real-time notifications pending implementation

## 🔮 Future Enhancements

- [ ] File upload for asset images
- [ ] Real-time notifications using WebSocket
- [ ] Advanced reporting and analytics
- [ ] Barcode/QR code scanning
- [ ] Mobile application
- [ ] Integration with external asset databases
- [ ] Automated asset depreciation calculation
- [ ] Multi-tenant support

## 💬 Support

If you encounter any issues or have questions:

1. Check the [API documentation](http://localhost:5000/api-docs)
2. Review the existing issues in the repository
3. Create a new issue with detailed information about the problem

## 👥 Authors

- **Frontend Developer**: Angular/TypeScript implementation
- **Backend Developer**: Node.js/Express API development
- **Database Designer**: MongoDB schema design

---

**Made with ❤️ for efficient IT asset management**

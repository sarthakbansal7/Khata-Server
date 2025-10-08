# Khata Server (Backend)

A comprehensive financial management backend API built with Node.js, Express.js, and MongoDB. This server provides secure authentication and transaction management capabilities for the Khata personal finance application.

## 🚀 Features

### Authentication & Security
- **JWT-based Authentication** - Secure token-based user authentication
- **Password Hashing** - Bcrypt encryption with salt rounds for password security
- **Protected Routes** - Middleware-based route protection
- **CORS Configuration** - Cross-origin resource sharing for frontend integration

### Transaction Management
- **Complete CRUD Operations** - Create, Read, Update, Delete transactions
- **Advanced Filtering** - Filter by date ranges, categories, transaction types
- **Pagination Support** - Efficient data loading with server-side pagination
- **Bulk Operations** - Import multiple transactions (CSV support)
- **Real-time Statistics** - Income, expense, and balance calculations

### Data Validation
- **Schema Validation** - Mongoose schema validation for data integrity
- **Input Sanitization** - Trim whitespace and validate input formats
- **Error Handling** - Comprehensive error responses with meaningful messages

## 📁 Project Structure

```
Khata-Server/
├── controllers/          # Business logic handlers
│   ├── authController.js    # Authentication logic
│   └── transactionController.js  # Transaction operations
├── middleware/           # Express middleware
│   └── authMiddleware.js    # JWT token verification
├── models/              # MongoDB schemas
│   ├── authSchema.js       # User model definition
│   └── transactionSchema.js # Transaction model definition
├── routes/              # API route definitions
│   ├── authRoutes.js       # Authentication endpoints
│   └── transactionRoutes.js # Transaction endpoints
├── app.js               # Express app configuration
├── server.js            # Server startup file
└── package.json         # Dependencies and scripts
```

## 🛠️ Tech Stack

- **Runtime:** Node.js
- **Framework:** Express.js
- **Database:** MongoDB with Mongoose ODM
- **Authentication:** JWT (JSON Web Tokens)
- **Security:** Bcryptjs for password hashing
- **Development:** Nodemon for auto-restart

## 📦 Dependencies

### Production Dependencies
```json
{
  "bcryptjs": "^3.0.2",      // Password hashing
  "cors": "^2.8.5",          // Cross-origin requests
  "dotenv": "^17.2.3",       // Environment variables
  "express": "^5.1.0",       // Web framework
  "jsonwebtoken": "^9.0.2",  // JWT authentication
  "mongoose": "^8.19.1"      // MongoDB ODM
}
```

### Development Dependencies
```json
{
  "nodemon": "^3.1.10"       // Development auto-restart
}
```

## ⚙️ Installation & Setup

### Prerequisites
- Node.js (v16 or higher)
- MongoDB (local or MongoDB Atlas)
- npm or yarn package manager

### Environment Variables
Create a `.env` file in the root directory:

```env
# Database Configuration
MONGODB_URI=mongodb://localhost:27017/khata
# or for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/khata

# JWT Configuration
JWT_SECRET=your-super-secret-jwt-key-here
JWT_EXPIRES_IN=7d

# Server Configuration
PORT=5000
NODE_ENV=development

# Frontend URL (for CORS)
FRONTEND_URL=http://localhost:3000
```

### Installation Steps

1. **Clone and navigate to server directory:**
   ```bash
   cd Khata-Server
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Set up environment variables:**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

4. **Start MongoDB:**
   ```bash
   # For local MongoDB
   mongod

   # Or ensure MongoDB Atlas connection is configured
   ```

5. **Run the server:**
   ```bash
   # Development mode (with auto-restart)
   npm run dev

   # Production mode
   npm start
   ```

## 🔧 Available Scripts

```bash
# Start server in production mode
npm start

# Start server in development mode with nodemon
npm run dev

# Run tests (if implemented)
npm test
```

## 🌐 API Endpoints

### Authentication Routes (`/api/auth`)

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "securepassword"
}
```

#### Login User
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "securepassword"
}
```

#### Get Current User
```http
GET /api/auth/me
Authorization: Bearer <jwt_token>
```

### Transaction Routes (`/api/transactions`)

#### Get All Transactions
```http
GET /api/transactions?page=1&limit=10&type=expense&category=Food
Authorization: Bearer <jwt_token>
```

**Query Parameters:**
- `page` - Page number for pagination (default: 1)
- `limit` - Items per page (default: 10)
- `type` - Filter by transaction type (`income` or `expense`)
- `category` - Filter by category
- `startDate` - Filter by start date (YYYY-MM-DD)
- `endDate` - Filter by end date (YYYY-MM-DD)
- `month` - Filter by specific month (1-12)
- `year` - Filter by specific year

#### Create Transaction
```http
POST /api/transactions
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "date": "2024-01-15",
  "description": "Grocery Shopping",
  "category": "Food & Dining",
  "type": "expense",
  "amount": 85.50
}
```

#### Update Transaction
```http
PUT /api/transactions/:id
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "description": "Updated description",
  "amount": 100.00
}
```

#### Delete Transaction
```http
DELETE /api/transactions/:id
Authorization: Bearer <jwt_token>
```

#### Bulk Create Transactions
```http
POST /api/transactions/bulk
Authorization: Bearer <jwt_token>
Content-Type: application/json

{
  "transactions": [
    {
      "date": "2024-01-15",
      "description": "Salary",
      "category": "Income",
      "type": "income",
      "amount": 5000
    }
  ]
}
```

#### Get Statistics
```http
GET /api/transactions/statistics?month=1&year=2024
Authorization: Bearer <jwt_token>
```

### Health Check
```http
GET /api/health
```

## 📊 Data Models

### User Schema
```javascript
{
  name: String (required, 2-50 chars),
  email: String (required, unique, valid email),
  password: String (required, min 6 chars, hashed),
  createdAt: Date,
  updatedAt: Date
}
```

### Transaction Schema
```javascript
{
  userId: ObjectId (required, ref: User),
  date: Date (required, default: now),
  description: String (required, max 200 chars),
  category: String (required, enum values),
  type: String (required, 'income' or 'expense'),
  amount: Number (required, positive),
  createdAt: Date,
  updatedAt: Date
}
```

### Categories
```javascript
[
  'Food & Dining',
  'Transportation', 
  'Shopping',
  'Entertainment',
  'Bills & Utilities',
  'Healthcare',
  'Education',
  'Travel',
  'Business',
  'Income',
  'Receipt',
  'Other'
]
```

## 🔒 Security Features

### Password Security
- Bcrypt hashing with salt rounds (cost factor: 12)
- Password length validation (minimum 6 characters)
- Secure password comparison methods

### JWT Authentication
- Token-based stateless authentication
- Configurable expiration times
- Protected route middleware
- Secure token verification

### Data Validation
- Mongoose schema validation
- Input sanitization and trimming
- Type checking and format validation
- Error message standardization

## 🚨 Error Handling

### Response Format
```javascript
// Success Response
{
  "success": true,
  "data": { /* response data */ },
  "message": "Operation completed successfully"
}

// Error Response
{
  "success": false,
  "message": "Error description",
  "errors": ["Detailed error messages"]
}
```

### HTTP Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request (validation errors)
- `401` - Unauthorized (authentication required)
- `404` - Not Found
- `500` - Internal Server Error

## 🧪 Testing

### Manual Testing with curl

```bash
# Register a new user
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","password":"password123"}'

# Login and get token
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'

# Create transaction (replace TOKEN with actual token)
curl -X POST http://localhost:5000/api/transactions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TOKEN" \
  -d '{"description":"Test Transaction","category":"Food & Dining","type":"expense","amount":50}'
```

## 📈 Performance Considerations

### Database Optimization
- Indexed fields for faster queries (userId, date, type)
- Efficient pagination with skip/limit
- Aggregation pipelines for statistics

### Memory Management
- Streaming for large datasets
- Proper connection pooling
- Garbage collection optimization

### Security Best Practices
- Rate limiting (recommended for production)
- Input validation and sanitization
- Secure headers configuration
- Environment-based configuration

## 🚀 Deployment

### Production Checklist
- [ ] Set `NODE_ENV=production`
- [ ] Configure production MongoDB URI
- [ ] Set secure JWT secret
- [ ] Enable HTTPS
- [ ] Configure proper CORS origins
- [ ] Set up process manager (PM2)
- [ ] Configure logging
- [ ] Set up monitoring
- [ ] Configure backup strategy

### Docker Deployment (Optional)
```dockerfile
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the ISC License.

## 🆘 Support & Troubleshooting

### Common Issues

1. **MongoDB Connection Error**
   ```
   Solution: Check MONGODB_URI in .env file
   Ensure MongoDB is running (local) or accessible (Atlas)
   ```

2. **JWT Token Invalid**
   ```
   Solution: Check JWT_SECRET configuration
   Ensure token is included in Authorization header
   ```

3. **CORS Issues**
   ```
   Solution: Verify FRONTEND_URL matches your client application
   Update CORS configuration in app.js
   ```

### Development Tips
- Use Postman or Insomnia for API testing
- Enable MongoDB logging for database debugging
- Check server logs for detailed error information
- Use nodemon for faster development cycles

---

**Made with ❤️ for personal finance management**
# Tenant Rent Payment System

A comprehensive platform for tenants to pay rent and receive automated receipts, with support for M-Pesa and bank transfers. Built with Node.js, React, and MySQL.

## 📋 Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Deployment](#deployment)
- [Contributing](#contributing)

## ✨ Features

### Tenant Features
- 🏠 **Self-Service Portal**: Easy-to-use interface to pay rent
- 💰 **Multiple Payment Methods**: M-Pesa and bank transfers
- 📄 **Automatic Receipts**: PDF receipts generated and emailed automatically
- 📊 **Payment History**: Track all payments and receipts
- 🔐 **Secure Authentication**: JWT-based login system
- ⏰ **Payment Reminders**: Email notifications for upcoming due dates

### Landlord Features
- 🏢 **Multi-Property Management**: Manage multiple properties
- 👥 **Tenant Management**: Assign and track tenants
- 💳 **Payment Monitoring**: View all tenant payments
- 📈 **Analytics Dashboard**: Payment trends and statistics
- 🧾 **Receipt Management**: Access all generated receipts

### Admin Features
- 🔧 **System Configuration**: Manage payment methods and settings
- 📊 **Platform Analytics**: Overall usage and revenue tracking
- 👤 **User Management**: Manage users and permissions

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Node.js 18+, Express.js 4+ |
| **Database** | MySQL 8+, Sequelize ORM |
| **Frontend** | React 18+, Redux Toolkit, Tailwind CSS |
| **Authentication** | JWT (jsonwebtoken) |
| **Payments** | M-Pesa API, Bank API (stub) |
| **Receipts** | PDFKit |
| **Email** | Nodemailer |
| **Security** | bcryptjs, helmet, cors |
| **Validation** | express-validator, joi |
| **Testing** | Jest, Supertest |
| **Deployment** | Railway/Heroku (backend), Vercel/Netlify (frontend) |

## 🚀 Quick Start

### Prerequisites
- Node.js 18+ and npm
- MySQL 8+
- Git

### Backend Setup

```bash
# Navigate to backend directory
cd backend

# Copy environment file
cp .env.example .env

# Edit .env with your configuration
nano .env  # or use your preferred editor

# Install dependencies
npm install

# Run migrations (creates database tables)
npm run migrate

# Seed sample data (optional)
npm run seed

# Start development server
npm run dev
```

Backend will run on `http://localhost:5000`

### Frontend Setup

```bash
# Navigate to frontend directory
cd frontend

# Copy environment file
cp .env.example .env

# Edit .env with backend URL
nano .env

# Install dependencies
npm install

# Start development server
npm start
```

Frontend will run on `http://localhost:3000`

### Test Credentials

**Tenant Login**
- Email: `tenant@example.com`
- Password: `password123`

**Landlord Login**
- Email: `landlord@example.com`
- Password: `password123`

## 📁 Project Structure

```
tenant-rent-payment-system/
├── backend/
│   ├── config/
│   │   ├── database.js          # Database configuration
│   │   └── constants.js         # App constants
│   ├── models/
│   │   ├── user.js
│   │   ├── property.js
│   │   ├── tenant.js
│   │   ├── payment.js
│   │   └── receipt.js
│   ├── routes/
│   │   ├── auth.js              # Authentication routes
│   │   ├── users.js             # User routes
│   │   ├── properties.js        # Property routes
│   │   ├── tenants.js           # Tenant routes
│   │   ├── payments.js          # Payment routes
│   │   └── receipts.js          # Receipt routes
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── paymentController.js
│   │   ├── receiptController.js
│   │   └── ... (other controllers)
│   ├── middleware/
│   │   ├── auth.js              # JWT verification
│   │   ├── errorHandler.js
│   │   └── validators.js
│   ├── utils/
│   │   ├── mpesa.js             # M-Pesa integration
│   │   ├── bank.js              # Bank transfer integration
│   │   ├── pdf.js               # Receipt PDF generation
│   │   ├── email.js             # Email service
│   │   └── logger.js            # Logging utility
│   ├── migrations/              # Database migrations
│   ├── seeders/                 # Database seeders
│   ├── server.js                # Express app entry point
│   ├── package.json
│   └── .env.example
│
├── frontend/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navigation.jsx
│   │   │   ├── ProtectedRoute.jsx
│   │   │   └── ... (other components)
│   │   ├── pages/
│   │   │   ├── Login.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── PayRent.jsx
│   │   │   ├── PaymentHistory.jsx
│   │   │   └── ... (other pages)
│   │   ├── redux/
│   │   │   ├── slices/
│   │   │   │   ├── authSlice.js
│   │   │   │   └── paymentSlice.js
│   │   │   └── store.js
│   │   ├── services/
│   │   │   └── api.js           # API client
│   │   ├── styles/
│   │   │   └── tailwind.css
│   │   ├── App.jsx
│   │   └── index.jsx
│   ├── package.json
│   └── .env.example
│
├── docs/
│   ├── API.md                   # API documentation
│   ├── DATABASE.md              # Database schema
│   ├── SETUP.md                 # Setup instructions
│   └── DEPLOYMENT.md            # Deployment guide
│
└── README.md
```

## 📚 API Documentation

See [docs/API.md](docs/API.md) for complete API endpoint documentation.

### Key Endpoints

**Authentication**
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - User login
- `POST /api/auth/verify` - Verify token

**Payments**
- `POST /api/payments/mpesa` - Initiate M-Pesa payment
- `POST /api/payments/bank` - Initiate bank transfer
- `GET /api/payments/history` - Get payment history
- `POST /api/payments/callback` - M-Pesa callback

**Receipts**
- `GET /api/receipts/:id` - Get receipt details
- `GET /api/receipts/:id/download` - Download receipt PDF
- `GET /api/receipts/user/:userId` - Get user's receipts

## 🗄️ Database Schema

See [docs/DATABASE.md](docs/DATABASE.md) for complete database schema and relationships.

### Main Tables
- `users` - User accounts (tenants, landlords, admins)
- `properties` - Rental properties
- `tenants` - Tenant assignments
- `payments` - Payment records
- `receipts` - Receipt documents

## 🔐 Environment Variables

### Backend (.env)

```env
# Server
NODE_ENV=development
PORT=5000
API_URL=http://localhost:5000

# Database
DB_HOST=localhost
DB_PORT=3306
DB_NAME=rent_payment_db
DB_USER=root
DB_PASSWORD=your_password

# Authentication
JWT_SECRET=your-super-secret-jwt-key-here
JWT_EXPIRE=7d

# M-Pesa
MPESA_API_KEY=your-mpesa-api-key
MPESA_API_SECRET=your-mpesa-api-secret
MPESA_SHORTCODE=your-shortcode
MPESA_PASSKEY=your-passkey
MPESA_CALLBACK_URL=https://your-domain.com/api/payments/callback

# Bank (example)
BANK_API_KEY=your-bank-api-key
BANK_API_SECRET=your-bank-api-secret

# Email
EMAIL_HOST=smtp.gmail.com
EMAIL_PORT=587
EMAIL_USER=your-email@gmail.com
EMAIL_PASSWORD=your-app-password
EMAIL_FROM=noreply@rentpayment.com

# Frontend
FRONTEND_URL=http://localhost:3000

# Logging
LOG_LEVEL=debug
```

### Frontend (.env)

```env
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_ENV=development
```

## 🚀 Deployment

See [docs/DEPLOYMENT.md](docs/DEPLOYMENT.md) for comprehensive deployment instructions.

### Quick Deployment Summary

**Backend** (Node.js/Express)
- Railway: Recommended for free tier
- Heroku: Traditional option
- AWS/GCP: For scale

**Frontend** (React)
- Vercel: Recommended
- Netlify: Good alternative
- AWS S3 + CloudFront: For scale

**Database** (MySQL)
- AWS RDS
- Railway Managed MySQL
- DigitalOcean Managed MySQL

## 🧪 Testing

```bash
# Backend tests
cd backend
npm test

# Frontend tests
cd frontend
npm test

# Coverage
npm run test:coverage
```

## 📖 Setup Instructions

See [docs/SETUP.md](docs/SETUP.md) for detailed setup instructions including:
- Database setup and migrations
- M-Pesa integration
- Email configuration
- Development vs production setup
- Troubleshooting guide

## 🔧 Development Workflow

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Make changes and commit
git add .
git commit -m "feat: description of changes"

# Push to remote
git push origin feature/your-feature-name

# Create Pull Request on GitHub
```

## 📝 Commit Message Format

```
feat: add new feature
fix: fix bug
docs: update documentation
style: code style changes
refactor: refactor code
test: add tests
chore: maintenance tasks
```

## 🐛 Troubleshooting

### Database Connection Failed
```bash
# Check MySQL is running
mysql -u root -p

# Verify .env database credentials
# Check database exists
SHOW DATABASES;
```

### M-Pesa Payment Not Working
- Verify API credentials in `.env`
- Check callback URL is accessible
- Ensure test number is registered

### Emails Not Sending
- Verify Gmail App Password (not regular password)
- Enable "Less secure apps" if needed
- Check email logs

### Port Already in Use
```bash
# Find and kill process on port 5000
lsof -i :5000
kill -9 <PID>
```

## 📞 Support

For issues and questions:
1. Check [docs/SETUP.md](docs/SETUP.md) for troubleshooting
2. Review API errors and logs
3. Create GitHub issue with detailed description

## 📄 License

This project is licensed under the MIT License - see LICENSE file for details.

## 🙋 Contributing

Contributions are welcome! Please follow the guidelines in CONTRIBUTING.md

## 📅 Roadmap

- [ ] SMS notifications
- [ ] Payment analytics
- [ ] Late payment penalties
- [ ] Automated payment reminders
- [ ] Mobile app (React Native)
- [ ] Multi-currency support
- [ ] Bulk payment uploads
- [ ] Ledger reports

## 🎉 Get Started

1. Clone the repository
2. Follow [Quick Start](#quick-start)
3. Update environment variables
4. Run migrations
5. Start development servers
6. Access frontend at http://localhost:3000

Happy coding! 🚀

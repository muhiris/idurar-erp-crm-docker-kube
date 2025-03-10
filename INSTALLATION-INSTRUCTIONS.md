# IDURAR ERP/CRM Setup Guide

This guide will walk you through setting up the IDURAR ERP/CRM application, with detailed instructions for MongoDB Atlas configuration.

## Getting Started

### Step 1: Clone the Repository
```bash
git clone https://github.com/idurar/idurar-erp-crm.git
cd idurar-erp-crm
```

### Step 2: Set Up MongoDB Atlas

1. **Create a MongoDB Atlas Account**
   - Visit [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register) and sign up or log in
   - Click "Create" to start a new project
   - Name your project (e.g., "IDURAR-ERP") and click "Next"
   - You can skip adding members and click "Create Project"

2. **Create a Database Cluster**
   - Click "Build a Database"
   - Select the free tier option (M0)
   - Choose your preferred cloud provider and region closest to you
   - Name your cluster (e.g., "idurar-mongo")
   - Click "Create"

3. **Set Up Database Security**
   - Under "Security Quickstart", create a database user:
     - Username: Choose a username (e.g., "idurar-admin")
     - Password: Create a strong password (save this for later)
     - Click "Create User"
   
   - Add your IP address to the allowlist:
     - Click "Add My Current IP Address" 
     - Or for development, you can add "0.0.0.0/0" to allow access from anywhere (not recommended for production)
     - Click "Finish and Close"

4. **Get Your Connection String**
   - Click "Connect" on your cluster dashboard
   - Select "Connect your application"
   - Copy the connection string that looks like:
     ```
     mongodb+srv://<username>:<password>@idurar-mongo.xxxxx.mongodb.net/?retryWrites=true&w=majority
     ```
   - Replace `<username>` with your database username
   - Replace `<password>` with your database password

### Step 3: Configure Environment Variables

1. **Set Up Backend Environment**
   - Navigate to the backend directory: `cd backend`
   - Create or edit the `.env` file:
   ```
   DATABASE="mongodb+srv://your-username:your-password@idurar-mongo.xxxxx.mongodb.net/?retryWrites=true&w=majority"
   JWT_SECRET="your_private_jwt_secret_key"
   NODE_ENV="development"
   OPENSSL_CONF='/dev/null'
   ```
   - Replace the DATABASE value with your actual MongoDB connection string

### Step 4: Install and Run Backend

1. **Install Dependencies**
   ```bash
   cd backend
   npm install
   ```

2. **Run Setup Script**
   ```bash
   npm run setup
   ```
   This creates necessary database collections and initial data.

3. **Start Backend Server**
   ```bash
   npm run dev
   ```
   The server should start at http://localhost:8888

   > **Note:** If you see an error about Node.js version, upgrade to Node.js v20+ using:
   > ```bash
   > # Using nvm (recommended)
   > nvm install 20
   > nvm use 20
   > # Or download from nodejs.org
   > ```

### Step 5: Install and Run Frontend

1. **Open a New Terminal Window** and navigate to the project root directory

2. **Install Frontend Dependencies**
   ```bash
   cd frontend
   npm install
   ```

3. **Start Frontend Server**
   ```bash
   npm run dev
   ```
   The frontend will be available at http://localhost:3000

   > **If you encounter OpenSSL errors:**
   > 
   > This is due to Node.js v17+ compatibility issues with OpenSSL. Try:
   > 
   > - **Option 1:** Set OpenSSL legacy provider before running:
   >   ```bash
   >   # On Linux/macOS
   >   export NODE_OPTIONS=--openssl-legacy-provider
   >   
   >   # On Windows Command Prompt
   >   set NODE_OPTIONS=--openssl-legacy-provider
   >   
   >   # On Windows PowerShell
   >   $env:NODE_OPTIONS = "--openssl-legacy-provider"
   >   ```
   >
   > - **Option 2:** Upgrade to Node.js v20+ (recommended)

## Troubleshooting MongoDB Connection

### Common MongoDB Atlas Issues:

1. **Connection Refused Error**
   - Verify your IP is in the Network Access allowlist on MongoDB Atlas
   - Navigate to: Atlas Dashboard → Network Access → Add IP Address

2. **Authentication Failed**
   - Check that username and password in your connection string are correct
   - In MongoDB Atlas: Database Access → Edit → Update Password

3. **Cluster Status Shows "Inactive"**
   - Check if your cluster is paused (free tier clusters pause after inactivity)
   - Resume your cluster from the Atlas dashboard

4. **Wrong Connection String Format**
   - Make sure you've copied the entire string from MongoDB Atlas
   - Double-check that you've replaced placeholders with actual values

### Testing Your MongoDB Connection

You can test your connection with this simple script before proceeding:

```javascript
// Save as test-mongo.js in your backend directory
const mongoose = require('mongoose');
require('dotenv').config();

mongoose.connect(process.env.DATABASE)
  .then(() => {
    console.log('MongoDB connection successful!');
    mongoose.connection.close();
  })
  .catch(err => {
    console.error('MongoDB connection error:', err);
  });
```

Run with: `node test-mongo.js`

## Logging In After Setup

- Default admin credentials:
  - Email: admin@demo.com
  - Password: admin123

## Additional Resources

- [MongoDB Atlas Documentation](https://docs.atlas.mongodb.com/)
- [IDURAR GitHub Repository](https://github.com/idurar/idurar-erp-crm)
- For more help, visit the IDURAR community forum or GitHub issues page
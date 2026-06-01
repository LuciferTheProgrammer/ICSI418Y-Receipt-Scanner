# Receipt Scanner

Applications are contained in the src and server folders.

## Receipt Scanner Web Application

This repository contains a full-stack receipt scanner and shopping management web application. The project uses a React frontend, an Express/Node.js backend, MongoDB database models, password hashing, receipt storage, shopping list management, spending analytics, and receipt scanning support through Azure Document Intelligence.

The application allows users to create accounts, log in, manually enter receipt information, upload scanned receipts for OCR processing, save receipt records, view weekly spending, analyze spending by date or item, manage shopping lists, configure user preferences, and compare shopping options through price search functionality.

## Features

* User account creation
* User login verification
* Secure password hashing with bcrypt
* React frontend routing
* Express backend API
* MongoDB database integration
* Mongoose schemas for users, receipts, and shopping lists
* Manual receipt entry
* Scanned receipt upload
* Azure Document Intelligence receipt OCR support
* Receipt record generation
* Weekly spending summary
* Spending analytics by date range
* Spending analytics by item
* Shopping list creation and editing
* Import latest receipt items into a shopping list
* User settings for preferred brands, banned brands, allergens, privacy, and search radius
* Price shopping/search support
* Favorite store handling
* Distance and transportation filtering
* Reusable sidebar navigation
* Reusable data table component
* Plus/minus number input component

## Project Files

### Frontend Files

* `index.js` - main React entry point that defines the application routes
* `reportWebVitals.js` - optional React performance measurement helper
* `setupTests.js` - Jest DOM testing setup
* `constants.js` - shared frontend constants for distance units, transportation types, and search defaults
* `Sidebar.jsx` - reusable sidebar navigation component
* `DataTable.jsx` - reusable table component for rendering dynamic rows and columns
* `PlusMinusNumberBox.jsx` - reusable plus/minus number input component

### Frontend Pages

* `Login.jsx` - login page for existing users
* `Signup.jsx` - account creation page for new users
* `Home.jsx` - landing page after login that shows recent weekly purchases and spending
* `ScannerOptions.jsx` - receipt entry and receipt scanning page
* `ShoppingListEditor.jsx` - shopping list creation and editing page
* `PriceShop.jsx` - price comparison and store search page
* `SpendingAnalytics.jsx` - spending analytics page with charts
* `Settings.jsx` - user preference and filter settings page
* `FAQ.jsx` - frequently asked questions page

### Backend Files

* `server.js` - main Express backend server with API routes for users, receipts, analytics, settings, shopping lists, and scanning
* `Hash.js` - helper file for hashing and verifying user passwords
* `PriceSearch.js` - helper file for processing price search inputs, store locations, distance filtering, and receipt matching
* `UserSchema.js` - Mongoose schema for user account records
* `ReceiptSchema.js` - Mongoose schema for receipt records and receipt items
* `ShoppingListSchema.js` - Mongoose schema for shopping list records

## User Authentication

The application supports user account creation and login.

When a user creates an account, the backend checks whether the username already exists. If the username is available, the password is hashed with bcrypt before being saved in the database.

When a user logs in, the backend retrieves the stored password hash and compares it against the provided password. This allows the application to verify login credentials without storing plaintext passwords.

## Receipt Records

Receipt records are stored in MongoDB using the `ReceiptSchema`.

A receipt record can contain:

* Store name
* Store location
* Store phone number
* Store geolocation
* Purchase date
* Purchase time
* Total price
* Tax rate
* Receipt items
* User who generated the receipt
* Generated timestamp
* Public/private flag

Each receipt item can contain:

* Item name
* Quantity
* Price
* Unit type
* Discount type
* Discount amount

## Receipt Scanning

The application supports scanned receipt uploads.

A user can upload a scanned receipt file, and the backend sends the file to Azure Document Intelligence. The service analyzes the receipt and returns extracted receipt fields. The frontend can then use that extracted data to help create a structured receipt record.

Supported scanning workflow:

1. User uploads a scanned receipt
2. Backend sends the file to Azure Document Intelligence
3. Backend waits for OCR processing to complete
4. Extracted receipt fields are returned to the frontend
5. User can review or use the data to create a receipt record

## Manual Receipt Entry

The application also supports manual receipt entry.

Users can manually enter receipt details such as store name, location, phone number, date, time, total price, tax rate, and item rows. Each item can include a name, quantity, price, unit type, discount type, and discount value.

After submission, the backend creates and stores a receipt record in MongoDB.

## Spending Analytics

The spending analytics page allows users to view spending totals over a selected date range.

The backend supports:

* Spending grouped by day
* Spending grouped by item

The frontend uses this data to display charts that help users understand their spending patterns.

## Shopping Lists

The shopping list editor allows users to create and manage shopping lists.

Users can:

* Create a new shopping list
* Add items to a list
* Edit item names, quantities, prices, and unit types
* Remove items from a list
* Import items from the latest saved receipt

Shopping lists are represented using a MongoDB schema with an owner ID, list name, and item entries.

## Price Search

The price search functionality is designed to help compare shopping options based on available receipt data, favorite stores, distance, transportation type, and user search preferences.

The price search helper processes input values such as:

* Username
* Shopping list name
* Current location
* Distance
* Distance unit
* Transportation type
* Whether to prioritize favorite stores
* Maximum receipt age
* Maximum number of stores
* Favorite store list

It can also collect available receipts, extract available store locations, insert favorite stores into the search order, filter stores by distance, and attach applicable receipts to store locations.

## User Settings

The settings page allows users to configure shopping and filtering preferences.

Settings can include:

* Preferred brands
* Banned brands
* Banned allergens
* Privacy flag
* Default distance/radius
* Search preferences

The backend provides API routes to get and update user settings.

## Navigation

The application uses a reusable sidebar component for navigation.

The sidebar includes links for:

* Home
* Record Receipt
* Shopping List
* Price Shop
* Spending Analytics
* Settings
* FAQ
* Sign Out

## Technologies Used

* JavaScript
* React
* React Router
* Node.js
* Express
* MongoDB
* Mongoose
* bcryptjs
* Axios
* Multer
* CORS
* dotenv
* Bootstrap
* Recharts
* Azure Document Intelligence
* HTML
* CSS

## Backend API Routes

The Express backend includes routes such as:

* `POST /createUser` - creates a new user account
* `GET /getUser` - verifies login credentials
* `POST /scanAzureAPI` - uploads a receipt file for Azure OCR processing
* `POST /generateReceiptRecord` - creates a receipt record
* `GET /getSpendingByDay` - returns spending totals grouped by day
* `GET /getSpendingByItem` - returns spending totals grouped by item
* `GET /getLatestReceiptForUser` - retrieves the latest receipt for a user
* `GET /getUserFilterSettings` - loads user filter settings
* `POST /updateUserFilterSettings` - updates user filter settings
* `GET /getUserSettings` - retrieves user settings
* `POST /updateUserSettings` - updates user settings
* `GET /getUserSearchPreferences` - retrieves user price search preferences
* `GET /priceSearch` - performs price search logic
* `GET /getShoppingListNames` - retrieves shopping list names for a user
* `GET /getThisWeeksItems` - retrieves items purchased within the last week

## How It Works

The project starts with a React frontend that provides pages for login, signup, receipt scanning, shopping lists, price search, spending analytics, settings, and FAQ.

The React app communicates with the Express backend through HTTP requests. The backend handles user authentication, receipt storage, receipt scanning, analytics queries, shopping list data, settings updates, and price search logic.

MongoDB stores the user records, receipt records, and shopping list records. Mongoose schemas define the structure of the stored documents.

When a user signs up, the backend hashes the password and saves the new user record. When a user logs in, the backend compares the submitted password with the stored hash.

When a user enters or scans a receipt, the backend creates a receipt record and saves it in MongoDB. The receipt data can later be used for spending analytics, shopping list imports, and price search calculations.

The spending analytics page requests receipt totals from the backend and displays the results with charts. The shopping list editor allows users to create lists manually or import items from the latest receipt. The settings page stores user preferences that can be used for filtering and search behavior.

## Running the Project

Install project dependencies:

```bash
npm install
```

Start the React frontend:

```bash
npm start
```

Start the backend server from the server folder or backend entry location:

```bash
node server.js
```

The React app runs on the frontend development server, and the Express backend listens on port `9000`.

## Environment Variables

The backend uses environment variables for external services and database access.

Recommended `.env` values include:

```text
AZURE_DI_ENDPOINT=your_azure_document_intelligence_endpoint
AZURE_DI_KEY=your_azure_document_intelligence_key
DB_KEY=your_mongodb_connection_string
```

Sensitive values such as API keys, database connection strings, and service credentials should be stored in `.env` and should not be committed to GitHub.

## Files That Should Not Be Committed

The following files and values should not be committed to a public repository:

```text
.env
node_modules/
build/
coverage/
*.log
.DS_Store
database connection strings
API keys
Azure keys
MongoDB usernames/passwords
uploaded receipt files containing personal data
```

## Security Notes

This project handles user accounts, passwords, receipt data, and external service credentials.

Passwords should always be hashed before being stored. API keys and database connection strings should be stored in environment variables instead of being hardcoded in source files. Receipt images and receipt records may contain private purchase information, so they should be handled carefully.

Before making the repository public, remove hardcoded credentials, API keys, database URLs, and any test files containing personal or sensitive data.

## Purpose

This project was created as a full-stack web application for managing receipt data, shopping lists, and spending analytics.

It demonstrates how a React frontend, Express backend, MongoDB database, password hashing, OCR receipt scanning, and chart-based analytics can work together in a practical web application.

The project helped practice full-stack development concepts including frontend routing, reusable React components, backend API design, database schemas, authentication logic, file upload handling, external API integration, and data visualization.

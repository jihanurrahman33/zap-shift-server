# Zap Shift Server

A full-stack Express.js backend server for a parcel delivery management system. This server handles parcel tracking, delivery assignments, rider management, user authentication, and payment processing.

## Features

- **User Management**: Create and manage users with role-based access control (admin, rider, customer)
- **Parcel Management**: Create, update, delete, and track parcels with real-time status updates
- **Rider Management**: Manage delivery riders and assign parcels to them
- **Payment Processing**: Integration with Stripe for secure payment handling
- **Authentication**: Firebase-based authentication with JWT token verification
- **Tracking System**: Generate unique tracking IDs for parcels and monitor delivery status
- **Statistics Dashboard**: Get delivery statistics and performance metrics
- **CORS Support**: Cross-origin requests enabled for frontend integration

## Tech Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB
- **Authentication**: Firebase Admin SDK
- **Payments**: Stripe API
- **Deployment**: Vercel

## Prerequisites

- Node.js (v14 or higher)
- MongoDB account and connection string
- Firebase project with service account credentials
- Stripe API keys
- Environment variables configured

## Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd zap-shift-server
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file in the root directory and add your environment variables:
```
PORT=3000
DB_USER=your_mongodb_user
DB_PASS=your_mongodb_password
FB_SERVICE_KEY=your_base64_firebase_key
STRIPE_SECRET=your_stripe_secret_key
```

## Running the Server

### Development Mode
```bash
npm run dev
```
This uses Nodemon to automatically restart the server on file changes.

### Production Mode
```bash
node index.js
```

The server will start on the port specified in the `.env` file (default: 3000).

## API Endpoints

### Users
- `POST /users` - Create a new user
- `GET /users` - Get all users (requires authentication)
- `GET /users/:id` - Get user by ID
- `GET /users/:email/role` - Get user role by email
- `PATCH /users/:id` - Update user information

### Parcels
- `POST /parcels` - Create a new parcel
- `GET /parcels` - Get all parcels
- `GET /parcels/:id` - Get parcel by ID
- `GET /parcels/rider` - Get parcels assigned to a rider
- `PATCH /parcels/:id` - Update parcel details
- `PATCH /parcels/:id/status` - Update parcel delivery status
- `DELETE /parcels/:id` - Delete a parcel
- `GET /parcels/delivery-status/stats` - Get delivery statistics

### Riders
- `POST /riders` - Create a new rider
- `GET /riders` - Get all riders
- `PATCH /riders/:id` - Update rider information (admin only)
- `GET /riders/delivery-per-day` - Get daily delivery statistics

### Payments
- `POST /create-checkout-session` - Create Stripe checkout session
- `PATCH /payment-success` - Update payment status after successful transaction
- `GET /payments` - Get all payments (requires authentication)

## Database Collections

- **users** - User accounts with roles and information
- **riders** - Delivery rider profiles
- **parcels** - Parcel information and delivery details
- **payments** - Payment transaction records
- **trackings** - Tracking history and status updates

## Authentication

The server uses Firebase for authentication. Protected routes require a valid Firebase ID token in the `Authorization` header:

```
Authorization: Bearer <firebase_id_token>
```

## Role-Based Access Control

- **Admin**: Full access to all resources, can manage riders and view analytics
- **Rider**: Can view assigned parcels and update delivery status
- **Customer**: Can create parcels and track orders

## Environment Variables

| Variable | Description |
|----------|-------------|
| `PORT` | Server port (default: 3000) |
| `DB_USER` | MongoDB username |
| `DB_PASS` | MongoDB password |
| `FB_SERVICE_KEY` | Firebase service account key (base64 encoded) |
| `STRIPE_SECRET` | Stripe secret API key |

## File Structure

```
├── index.js           # Main server file with all API endpoints
├── keyConvert.js      # Utility for converting Firebase keys to base64
├── package.json       # Project dependencies and scripts
├── vercel.json        # Vercel deployment configuration
└── README.md          # This file
```

## Deployment

The project is configured for deployment on Vercel. Ensure all environment variables are set in your Vercel project settings.

```bash
# Vercel automatically deploys when you push to your repository
git push origin main
```

## Key Utilities

### Tracking ID Generation
The server automatically generates unique tracking IDs for parcels using the format:
```
PRCL-YYYYMMDD-XXXXXX
```

### Firebase Key Conversion
The `keyConvert.js` utility helps convert Firebase service account keys to base64 format for the `FB_SERVICE_KEY` environment variable.

## Error Handling

The server includes comprehensive error handling with appropriate HTTP status codes:
- `400` - Bad Request
- `401` - Unauthorized (missing or invalid token)
- `403` - Forbidden (insufficient permissions)
- `500` - Internal Server Error

## CORS Configuration

CORS is enabled for all origins. You can configure allowed origins in the `cors()` middleware:

```javascript
app.use(cors());
```

## Development

- Use `npm run dev` for local development with hot reload
- Make sure MongoDB and Firebase are properly configured
- Test endpoints using Postman or any REST client

## License

ISC

## Author

Created by the Zap Shift team.

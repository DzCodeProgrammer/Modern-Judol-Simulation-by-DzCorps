# API Endpoints Specification

## Authentication Module

### POST /api/auth/register
**Description**: Register a new user account
**Request Body**:
```json
{
  "username": "string",
  "email": "string",
  "password": "string",
  "confirmPassword": "string",
  "fullName": "string",
  "dateOfBirth": "YYYY-MM-DD",
  "phoneNumber": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Registration successful",
  "data": {
    "userId": "integer",
    "email": "string",
    "requiresEmailVerification": "boolean"
  }
}
```

### POST /api/auth/login
**Description**: Authenticate user credentials
**Request Body**:
```json
{
  "email": "string",
  "password": "string",
  "rememberMe": "boolean"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "JWT token",
    "user": {
      "id": "integer",
      "username": "string",
      "email": "string",
      "fullName": "string"
    }
  }
}
```

### POST /api/auth/forgot-password
**Description**: Initiate password reset process
**Request Body**:
```json
{
  "email": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Password reset email sent"
}
```

### POST /api/auth/reset-password
**Description**: Reset user password
**Request Body**:
```json
{
  "token": "string",
  "newPassword": "string",
  "confirmPassword": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Password reset successful"
}
```

## User Profile Module

### GET /api/user/profile
**Description**: Get authenticated user's profile
**Headers**: Authorization: Bearer <token>
**Response**:
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "username": "string",
    "email": "string",
    "fullName": "string",
    "dateOfBirth": "YYYY-MM-DD",
    "phoneNumber": "string",
    "avatarUrl": "string",
    "bio": "string",
    "country": "string",
    "city": "string"
  }
}
```

### PUT /api/user/profile
**Description**: Update user profile
**Headers**: Authorization: Bearer <token>
**Request Body**:
```json
{
  "fullName": "string",
  "phoneNumber": "string",
  "bio": "string",
  "country": "string",
  "city": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Profile updated successfully",
  "data": {
    "profile": { /* updated profile data */ }
  }
}
```

### PUT /api/user/password
**Description**: Change user password
**Headers**: Authorization: Bearer <token>
**Request Body**:
```json
{
  "currentPassword": "string",
  "newPassword": "string",
  "confirmPassword": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Password changed successfully"
}
```

## Wallet Module

### GET /api/wallet/balance
**Description**: Get user's wallet balance
**Headers**: Authorization: Bearer <token>
**Response**:
```json
{
  "success": true,
  "data": {
    "balance": "decimal",
    "currency": "string"
  }
}
```

### GET /api/wallet/transactions
**Description**: Get user's transaction history
**Headers**: Authorization: Bearer <token>
**Query Parameters**: 
- page (integer, optional)
- limit (integer, optional)
- type (string, optional - deposit|withdrawal|bonus)
**Response**:
```json
{
  "success": true,
  "data": {
    "transactions": [
      {
        "id": "integer",
        "type": "string",
        "amount": "decimal",
        "balanceAfter": "decimal",
        "description": "string",
        "status": "string",
        "createdAt": "datetime"
      }
    ],
    "pagination": {
      "page": "integer",
      "limit": "integer",
      "total": "integer"
    }
  }
}
```

### POST /api/wallet/deposit
**Description**: Initiate a deposit transaction
**Headers**: Authorization: Bearer <token>
**Request Body**:
```json
{
  "amount": "decimal",
  "paymentMethod": "string",
  "currency": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Deposit initiated",
  "data": {
    "transactionId": "integer",
    "redirectUrl": "string" // for payment gateway redirect
  }
}
```

### POST /api/wallet/withdraw
**Description**: Initiate a withdrawal request
**Headers**: Authorization: Bearer <token>
**Request Body**:
```json
{
  "amount": "decimal",
  "paymentMethod": "string",
  "bankAccount": "string" // if applicable
}
```
**Response**:
```json
{
  "success": true,
  "message": "Withdrawal request submitted",
  "data": {
    "transactionId": "integer",
    "status": "pending"
  }
}
```

## Games Module

### GET /api/games
**Description**: Get list of available games
**Query Parameters**: 
- category (string, optional)
- page (integer, optional)
- limit (integer, optional)
**Response**:
```json
{
  "success": true,
  "data": {
    "games": [
      {
        "id": "integer",
        "name": "string",
        "description": "string",
        "provider": "string",
        "thumbnailUrl": "string",
        "minBet": "decimal",
        "maxBet": "decimal",
        "category": "string"
      }
    ],
    "pagination": {
      "page": "integer",
      "limit": "integer",
      "total": "integer"
    }
  }
}
```

### GET /api/games/{id}
**Description**: Get detailed information about a specific game
**Response**:
```json
{
  "success": true,
  "data": {
    "id": "integer",
    "name": "string",
    "description": "string",
    "provider": "string",
    "thumbnailUrl": "string",
    "minBet": "decimal",
    "maxBet": "decimal",
    "houseEdge": "decimal",
    "gameConfig": "json"
  }
}
```

### POST /api/games/{id}/play
**Description**: Start a game session
**Headers**: Authorization: Bearer <token>
**Request Body**:
```json
{
  "betAmount": "decimal"
}
```
**Response**:
```json
{
  "success": true,
  "data": {
    "sessionId": "integer",
    "gameState": "json"
  }
}
```

## Promotions Module

### GET /api/promotions
**Description**: Get available promotions
**Headers**: Authorization: Bearer <token>
**Response**:
```json
{
  "success": true,
  "data": {
    "promotions": [
      {
        "id": "integer",
        "name": "string",
        "description": "string",
        "type": "string",
        "configuration": "json",
        "startDate": "datetime",
        "endDate": "datetime"
      }
    ]
  }
}
```

### POST /api/promotions/{id}/claim
**Description**: Claim a promotion
**Headers**: Authorization: Bearer <token>
**Response**:
```json
{
  "success": true,
  "message": "Promotion claimed successfully",
  "data": {
    "promotionId": "integer",
    "status": "claimed"
  }
}
```

## Support Module

### GET /api/support/tickets
**Description**: Get user's support tickets
**Headers**: Authorization: Bearer <token>
**Response**:
```json
{
  "success": true,
  "data": {
    "tickets": [
      {
        "id": "integer",
        "subject": "string",
        "category": "string",
        "priority": "string",
        "status": "string",
        "createdAt": "datetime",
        "updatedAt": "datetime"
      }
    ]
  }
}
```

### POST /api/support/tickets
**Description**: Create a new support ticket
**Headers**: Authorization: Bearer <token>
**Request Body**:
```json
{
  "subject": "string",
  "category": "string",
  "priority": "string",
  "message": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Ticket created successfully",
  "data": {
    "ticketId": "integer"
  }
}
```

### GET /api/support/tickets/{id}
**Description**: Get details of a specific support ticket
**Headers**: Authorization: Bearer <token>
**Response**:
```json
{
  "success": true,
  "data": {
    "ticket": {
      "id": "integer",
      "subject": "string",
      "category": "string",
      "priority": "string",
      "status": "string",
      "createdAt": "datetime",
      "updatedAt": "datetime",
      "messages": [
        {
          "id": "integer",
          "senderType": "string",
          "message": "string",
          "sentAt": "datetime"
        }
      ]
    }
  }
}
```

### POST /api/support/tickets/{id}/messages
**Description**: Add a message to a support ticket
**Headers**: Authorization: Bearer <token>
**Request Body**:
```json
{
  "message": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Message added successfully"
}
```

## Admin API Endpoints

### POST /api/admin/auth/login
**Description**: Authenticate admin user
**Request Body**:
```json
{
  "email": "string",
  "password": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "token": "JWT token",
    "admin": {
      "id": "integer",
      "username": "string",
      "email": "string",
      "fullName": "string",
      "isSuperAdmin": "boolean"
    }
  }
}
```

### GET /api/admin/users
**Description**: Get list of users
**Headers**: Authorization: Bearer <admin_token>
**Query Parameters**: 
- page (integer, optional)
- limit (integer, optional)
- status (string, optional - active|suspended)
**Response**:
```json
{
  "success": true,
  "data": {
    "users": [
      {
        "id": "integer",
        "username": "string",
        "email": "string",
        "fullName": "string",
        "isActive": "boolean",
        "isSuspended": "boolean",
        "createdAt": "datetime"
      }
    ],
    "pagination": {
      "page": "integer",
      "limit": "integer",
      "total": "integer"
    }
  }
}
```

### PUT /api/admin/users/{id}/status
**Description**: Update user status (activate/suspend)
**Headers**: Authorization: Bearer <admin_token>
**Request Body**:
```json
{
  "isActive": "boolean",
  "isSuspended": "boolean",
  "reason": "string"
}
```
**Response**:
```json
{
  "success": true,
  "message": "User status updated successfully"
}
```

### GET /api/admin/transactions
**Description**: Get financial transactions
**Headers**: Authorization: Bearer <admin_token>
**Query Parameters**: 
- page (integer, optional)
- limit (integer, optional)
- type (string, optional)
- status (string, optional)
**Response**:
```json
{
  "success": true,
  "data": {
    "transactions": [
      {
        "id": "integer",
        "userId": "integer",
        "username": "string",
        "type": "string",
        "amount": "decimal",
        "balanceAfter": "decimal",
        "status": "string",
        "createdAt": "datetime"
      }
    ],
    "pagination": {
      "page": "integer",
      "limit": "integer",
      "total": "integer"
    }
  }
}
```

### GET /api/admin/reports/financial
**Description**: Get financial reports
**Headers**: Authorization: Bearer <admin_token>
**Query Parameters**: 
- startDate (date, optional)
- endDate (date, optional)
**Response**:
```json
{
  "success": true,
  "data": {
    "totalDeposits": "decimal",
    "totalWithdrawals": "decimal",
    "netRevenue": "decimal",
    "reportData": [
      {
        "date": "date",
        "deposits": "decimal",
        "withdrawals": "decimal",
        "revenue": "decimal"
      }
    ]
  }
}
```
# Pub Network API Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [User Roles](#user-roles)
3. [Authentication](#authentication)
4. [Customer APIs](#customer-apis)
5. [Venue APIs](#venue-apis)
6. [Chat System](#chat-system)
7. [Ice Breakers](#ice-breakers)
8. [WebSocket Integration](#websocket-integration)
9. [Superuser Features](#superuser-features)
10. [Admin Panel](#admin-panel)

---

## Introduction

Pub Network is a platform that connects venues (pubs, bars, restaurants) with customers through a digital ecosystem. The system enables real-time chat, offers, ice breakers for social interaction, and provides venue owners with analytics dashboards.

**Base URL:** https://api.netwok.app/

---

## User Roles

The platform supports three distinct user roles, each with specific permissions and capabilities:

1. **Superuser**: Platform administrator with complete access to all features and data
2. **Venue Owner**: Manages venue details, offers, and can view venue-specific analytics
3. **Customer**: End user who interacts with venues, redeems offers, and participates in chats

---

## Authentication

All protected endpoints require a JWT Bearer token in the Authorization header:
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6Ik...
```

### Authentication Endpoints

#### 1. Customer Login (Password-based)
- **Endpoint**: `/login/customer/`
- **Method**: POST
- **Authentication**: None
- **Body**:
  ```json
  {
    "username": "c1",
    "password": "abcd@123",
    "venue": 3
  }
  ```
- **Response**: Returns access and refresh tokens along with user details
- **Notes**: The `venue` parameter indicates which venue the customer is currently at

#### 2. Customer Registration
- **Endpoint**: `/register/customer/`
- **Method**: POST
- **Authentication**: None
- **Body**:
  ```json
  {
    "username": "john",
    "password": "secret",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "gender": "male",
    "customer_profile": {
      "name": "John D.",
      "one_line_desc": "I love music!",
      "interests": ["Music", "Food"]
    }
  }
  ```
- **Response**: Success message and user details including profile
- **Notes**: `customer_profile` is optional but recommended

#### 3. Passwordless Login (OTP)
- **Endpoint**: `/customers/login/passwordless/`
- **Method**: POST
- **Authentication**: None
- **Body**:
  ```json
  {
    "username": "c2",
    "email": "c2@example.com",
    "var_id": "v2"
  }
  ```
- **Response**: `{"message": "OTP sent, please check your email."}`
- **Notes**: Requires valid username, email, and venue QR code ID (var_id)

#### 4. OTP Validation
- **Endpoint**: `/customers/login/validate-otp/`
- **Method**: POST
- **Authentication**: None
- **Body**:
  ```json
  {
    "email": "c2@example.com",
    "otp": "624469"
  }
  ```
- **Response**: Access and refresh tokens upon successful validation
- **Notes**: OTP expires after 10 minutes

#### 5. Logout
- **Endpoint**: `/logout/`
- **Method**: POST
- **Authentication**: Required
- **Body**:
  ```json
  {
    "refresh": "<refresh_token>"
  }
  ```
- **Response**: Success message
- **Notes**: Invalidates the provided refresh token

#### 6. Google OAuth Login
- **Endpoint**: `/google/callback/`
- **Method**: GET
- **Authentication**: None
- **Parameters**: `code` (from Google OAuth)
- **Response**: Redirects with tokens or error message
- **Notes**: Used as callback URL for Google OAuth flow

#### 7. Token Details
- **Endpoint**: `/customer-tokens/`
- **Method**: GET
- **Authentication**: Required
- **Response**: Token details including expiration
- **Notes**: Useful for checking token validity and expiration

---

## Customer APIs

#### 1. Get Customer Details
- **Endpoint**: `/customers/`
- **Method**: GET
- **Authentication**: Required
- **Response**: Customer profile and account details
- **Notes**: Returns the currently authenticated customer's information

#### 2. Update Customer
- **Endpoint**: `/customers/1/`
- **Method**: PUT/PATCH
- **Authentication**: Required
- **Body**: Fields to update
- **Response**: Updated customer details
- **Notes**: Customers can only update their own profiles

#### 3. Delete Customer
- **Endpoint**: `/customers/1/`
- **Method**: DELETE
- **Authentication**: Required (Superuser only)
- **Response**: Success confirmation
- **Notes**: Only superusers can delete customer accounts

---

## Venue APIs

#### 1. Create Venue
- **Endpoint**: `/venues/`
- **Method**: POST
- **Authentication**: Required (Venue owner or Superuser)
- **Body**:
  ```json
  {
    "name": "Venue XYZ",
    "qr_settings": {"qr_gen_frequency_hours": 24}
  }
  ```
- **Response**: Created venue details
- **Notes**: Minimal required fields shown; can include address and business_details

#### 2. List Venues
- **Endpoint**: `/venues/`
- **Method**: GET
- **Authentication**: Required
- **Response**: List of venues (filtered by permissions)
- **Notes**: Venue owners see only their venues; superusers see all

#### 3. Update Venue
- **Endpoint**: `/venues/5/`
- **Method**: PUT/PATCH
- **Authentication**: Required (Venue owner or Superuser)
- **Body**: Fields to update
- **Response**: Updated venue details
- **Notes**: Venue owners can only update their own venues

#### 4. Delete Venue
- **Endpoint**: `/venues/5/`
- **Method**: DELETE
- **Authentication**: Required (Venue owner or Superuser)
- **Response**: Success confirmation
- **Notes**: Venue owners can only delete their own venues

#### 5. Generate New var_id
- **Endpoint**: `/venues/4/generate-var-id/`
- **Method**: POST
- **Authentication**: Required (Venue owner or Superuser)
- **Response**: New var_id for QR code generation
- **Notes**: Creates a new random 5-character hexadecimal ID

#### 6. Delete Venue Offer
- **Endpoint**: `/venues/delete-offer/`
- **Method**: POST
- **Authentication**: Required (Venue owner or Superuser)
- **Body**:
  ```json
  {
    "id": "257874ea-05a3-485c-b627-42b2c24aac0c"
  }
  ```
- **Response**: Success confirmation
- **Notes**: Permanently removes the offer

#### 7. Venue Dashboard Overview
- **Endpoint**: `/overview/3/`
- **Method**: GET
- **Authentication**: Required (Venue owner or Superuser)
- **Response**: Venue statistics and performance metrics
- **Notes**: `3` is the venue ID

#### 8. QR Analytics
- **Endpoint**: `/qr-analytics/3/`
- **Method**: GET
- **Authentication**: Required (Venue owner or Superuser)
- **Response**: QR code scan statistics
- **Notes**: `3` is the venue ID

#### 9. Redeem Offer
- **Endpoint**: `/venues/redeem-offer/`
- **Method**: POST
- **Authentication**: Required (Customer)
- **Body**:
  ```json
  {
    "offer_id": "535f04bd-8a11-4728-b87b-909912a8749a"
  }
  ```
- **Response**: Redemption details with token
- **Notes**: Customer must be physically present at the venue (current_venue must match)

---

## Chat System

### Private Chat

#### 1. Create Private Chat
- **Endpoint**: `/private/create/`
- **Method**: POST
- **Authentication**: Required
- **Body**:
  ```json
  {
    "username": "c1"
  }
  ```
- **Response**: Chat details with unique chat_id
- **Notes**: Creates a private chat room between current user and specified user

#### 2. List Private Chats
- **Endpoint**: `/private/list/`
- **Method**: GET
- **Authentication**: Required
- **Response**: List of all private chats for current user
- **Notes**: Includes chat_id and other participant information

#### 3. Private Chat History
- **Endpoint**: `/private/<chat_id>/history/`
- **Method**: GET
- **Authentication**: Required
- **Response**: Chat message history
- **Notes**: Only accessible to participants of the chat

### Group Chat

#### 1. Group Chat History
- **Endpoint**: `/group/<var_id>/history/`
- **Method**: GET
- **Authentication**: Required
- **Response**: Group chat message history
- **Notes**: `var_id` is the unique venue variable ID

---

## Ice Breakers

#### 1. Get Ice Breakers
- **Endpoint**: `/icebreakers`
- **Method**: GET
- **Authentication**: None
- **Parameters**: 
  - `level` (1-5)
  - `gender` (male, female, or omit for any)
- **Response**: List of ice breakers matching criteria
- **Notes**: Filters by level and optional gender target

#### 2. Random Ice Breaker
- **Endpoint**: `/icebreakers/random/`
- **Method**: GET
- **Authentication**: None
- **Parameters**: Same as above (optional)
- **Response**: Single random ice breaker
- **Notes**: Great for quick social interaction prompts

---

## WebSocket Integration

Real-time messaging is implemented via WebSockets:

### Group Chat WebSocket
- **Endpoint**: `ws://localhost:5000/ws/group/v1/`
- **Messages**:
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "message": "Hello everyone!"
  }
  ```
- **Notes**: 
  - `v1` is the venue's var_id
  - Messages are broadcast to all connected users in the venue
  - Authentication via JWT token in each message

### Private Chat WebSocket
- **Endpoint**: `ws://localhost:5000/ws/private/31933/`
- **Messages**:
  ```json
  {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "message": "Hello there!"
  }
  ```
- **Notes**:
  - `31933` is the unique chat_id
  - Messages are delivered only to the two participants
  - Authentication via JWT token in each message

---

## Superuser Features

These features are planned for implementation:

### Dashboard
- **Dashboard Counts**: Total venues, active users, monthly revenue, active subscriptions
- **Overview**: Recent signups, subscription distribution, recent coupons, recent activity
- **Analytics**: 12-month analytics for customers, revenue, chats, messages
- **Reports**: Detailed 12-month performance reports

### Venue Management
- Comprehensive venue listing with status, subscription, payment details
- New venue creation with detailed information collection
- Activity logging of all platform events

---

## Admin Panel

- **URL**: https://api.netwok.app/admin
- **Credentials**: admin / admin
- **Features**:
  - User management
  - Venue management
  - Offer management
  - Chat moderation
  - Ice breaker content management
  - System configuration

---

## Sample Flows

### Customer Journey
1. Customer scans venue QR code
2. Registers account or logs in (password or passwordless)
3. Browses ice breakers for social interaction
4. Participates in venue group chat
5. Initiates private chats with other customers
6. Redeems venue offers

### Venue Owner Journey
1. Logs in to dashboard
2. Views analytics and customer engagement metrics
3. Creates special offers for customers
4. Regenerates QR code if needed
5. Monitors group chat activity

### Superuser Journey
1. Manages all venues and users
2. Views platform-wide analytics
3. Monitors subscription status
4. Adds new ice breakers
5. Reviews activity logs
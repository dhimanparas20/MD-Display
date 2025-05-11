
---

# Pub Network API & Feature Guide

## Table of Contents

1. [Project Overview](#project-overview)
2. [User Roles](#user-roles)
3. [Authentication & Registration](#authentication--registration)
    - Customer Login (Password & Passwordless)
    - Customer Registration
    - Token Management
    - Logout
    - Google Login
4. [Customer APIs](#customer-apis)
5. [Venue APIs](#venue-apis)
6. [Offer Redemption](#offer-redemption)
7. [Ice Breakers](#ice-breakers)
8. [Chatting APIs](#chatting-apis)
    - Private Chat
    - Group Chat
    - WebSocket Messaging
9. [Superuser Features](#superuser-features)
10. [Admin Panel](#admin-panel)
11. [Appendix: Example Requests & Responses](#appendix-example-requests--responses)

---

## Project Overview

**Pub Network** is a platform connecting venues (like pubs, bars, restaurants) with customers, managed by a superuser (platform owner).  
It supports:
- Venue management
- Customer engagement (offers, ice breakers)
- Real-time chat (private & group)
- Analytics and dashboards for venues and superusers

---

## User Roles

- **Superuser**: Platform owner, manages everything.
- **Venue Owner**: Manages their own venue(s), offers, and analytics.
- **Customer**: End-user, can register, login, redeem offers, chat, and interact with venues.

---

## Authentication & Registration

### 1. Customer Login

#### a. Password Login
**POST** `/login/customer/`  
**Body:**
```json
{
  "username": "c1",
  "password": "abcd@123",
  "venue": 3
}
```
**Returns:** JWT tokens and user info.

#### b. Passwordless Login (OTP)
**POST** `/customers/login/passwordless/`  
**Body:**
```json
{
  "username": "c2",
  "email": "c2@example.com",
  "var_id": "v2"
}
```
- Checks user and venue.
- Sends OTP (printed to terminal for now).
- Returns: `{"message": "OTP sent, please check your email."}`

**POST** `/customers/login/validate-otp/`  
**Body:**
```json
{
  "email": "c2@example.com",
  "otp": "624469"
}
```
- Validates OTP, returns JWT tokens and user info.

### 2. Customer Registration

**POST** `/register/customer/`  
**Body:**
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
- Registers a new customer and profile.

### 3. Token Management

**GET** `/customer-tokens/`  
- Requires Bearer token.
- Returns token details, expiry, etc.

### 4. Logout

**POST** `/logout/`  
**Body:** `{ "refresh": "<refresh_token>" }`  
- Invalidates the token.

### 5. Google Login

**GET** `/google/callback/?code=...`  
- Handles Google OAuth login.

---

## Customer APIs

- **GET** `/customers/`  
  - Get your customer details (token required).
- **PUT/PATCH** `/customers/1/`  
  - Update your details (token required).
- **DELETE** `/customers/1/`  
  - Delete customer (superuser only).
- **GET** `/customer-tokens/`  
  - Get token info (token required).

---

## Venue APIs

- **CRUD** `/venues/`  
  - Create, update, delete, list venues (token required).
  - **POST** body (minimum):
    ```json
    {
      "name": "Venue xyc",
      "qr_settings": {"qr_gen_frequency_hours": 24}
    }
    ```
- **POST** `/venues/4/generate-var-id/`  
  - Generate a new QR var_id for venue 4 (token required).
- **POST** `/venues/delete-offer/`  
  - Delete an offer (token required).
  - **Body:** `{ "id": "offer-uuid" }`
- **GET** `/overview/3/`  
  - Venue dashboard overview (token required).
- **GET** `/qr-analytics/3/`  
  - Venue QR analytics (token required).

---

## Offer Redemption

- **POST** `/venues/redeem-offer/`  
  - Redeem an offer (token required).
  - **Body:** `{ "offer_id": "offer-uuid" }`
  - Checks if customer is at the venue, offer is valid, not expired, and not already redeemed.

---

## Ice Breakers

- **GET** `/icebreakers?level=3&gender=male`  
  - Fetch all ice breakers for a level/gender (no token needed).
- **GET** `/icebreakers/random/`  
  - Fetch a random ice breaker (no token needed).

---

## Chatting APIs

### Private Chat

- **POST** `/private/create/`  
  - Create a private chat with another user (token required).
  - **Body:** `{ "username": "c1" }`
- **GET** `/private/list/`  
  - List all private chat IDs for the user (token required).
- **GET** `/private/<chat_id>/history/`  
  - Get chat history (token required).

### Group Chat

- **GET** `/group/<var_id>/history/`  
  - Get group chat history for a venue (token required).

---

## WebSocket Messaging

### Group Chat

- **Connect:** `ws://localhost:5000/ws/group/v1/`
- **Send Message:**  
  ```json
  {
    "token": "<JWT access token>",
    "message": "Hello group!"
  }
  ```
- **Receive:**  
  - Broadcasts messages to all connected users in the group.

### Private Chat

- **Connect:** `ws://localhost:5000/ws/private/<chat_id>/`
- **Send Message:**  
  ```json
  {
    "token": "<JWT access token>",
    "message": "Hello!"
  }
  ```
- **Receive:**  
  - Broadcasts messages to both users in the chat.

---

## Superuser Features

- **Dashboard:**  
  - Counts for total venues, active users, monthly revenue, active subscriptions.
- **Overview:**  
  - Recent signups, subscription distribution, recent coupons, recent activity.
- **Analytics:**  
  - 12-month analytics for customers, revenue, chats, messages, etc.
- **Reports:**  
  - Detailed 12-month reports.
- **Venues:**  
  - List all venues with name, address, subscription, last payment, last QR scanned, status.
- **New Venue:**  
  - Create new venue with details and contact info.
- **Activity Log:**  
  - Logs for venue creation, payments, subscriptions, coupons, logins, venue status changes, etc.

---

## Admin Panel

- **URL:** `https://api.netwok.app/admin`
- **Login:** `admin` / `admin`
- Manage all users, venues, offers, redemptions, chats, ice breakers, etc.

---

## Appendix: Example Requests & Responses

### 1. Customer Registration

**Request:**
```http
POST /register/customer/
Content-Type: application/json

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

**Response:**
```json
{
  "message": "Customer registered successfully.",
  "user": {
    "id": 26,
    "username": "john",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "gender": "male",
    "user_type": "customer",
    "customer_profile": {
      "name": "John D.",
      "one_line_desc": "I love music!",
      "interests": ["Music", "Food"],
      "current_venue": null,
      "doj": "2024-06-01T12:00:00Z",
      "is_verified": false,
      "otp_validated": false
    }
  }
}
```

### 2. Passwordless Login

**Request:**
```http
POST /customers/login/passwordless/
Content-Type: application/json

{
  "username": "c2",
  "email": "c2@example.com",
  "var_id": "v2"
}
```

**Response:**
```json
{
  "message": "OTP sent, please check your email."

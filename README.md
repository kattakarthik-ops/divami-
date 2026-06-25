# divami-
creating a web application
# Archery Shop — Project Document

---

## Project Overview

An e-commerce web application for selling imported archery equipment. Users can browse products, add them to a cart, and pay via UPI by uploading a payment screenshot. An admin manually verifies payments, manages products, updates order statuses, and can upload promotional videos tagged to a social media platform. Both users and admins can communicate through a comment thread below each order.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React + Vite + Tailwind CSS |
| Backend | Spring Boot (Java 17) |
| Database | MySQL 8 |
| ORM | Spring Data JPA + Hibernate |
| Authentication | Spring Security + JWT |
| File Uploads | Spring Multipart |
| HTTP Client | Axios |
| Frontend Hosting | Vercel |
| Backend Hosting | Railway |

---

## Features

### User
- Register and log in with email and password
- Browse product catalog with search and category filter
- Add products to cart, update quantities, remove items
- Pay via UPI — view UPI ID, QR code, and phone number
- Upload payment screenshot to generate an Order ID
- View personal orders and their statuses
- Comment on their own orders

### Admin
- Log in via admin credentials
- Add, edit, and delete products with image upload
- View all orders with payment proof screenshots
- Verify or reject payments and update order status
- Comment on any order
- Upload promotional videos tagged as Instagram / Facebook / YouTube

---

## Authentication Flow

```
1. User registers → password hashed with BCrypt → saved in MySQL
2. User logs in → Spring Security validates credentials
3. JWT token generated and returned to the frontend
4. Token stored in localStorage
5. Every API request sends: Authorization: Bearer <token>
6. JwtFilter on the backend validates the token on every request
7. Role-based access enforced: USER and ADMIN routes protected separately
```

---

## Video Upload Flow

```
1. Admin navigates to Admin → Videos
2. Clicks "Upload Video"
3. Selects a video file from their device
4. Chooses a platform: Instagram / Facebook / YouTube
5. Enters a title and clicks Upload
6. Backend saves the file to the /uploads/videos/ folder
7. A record is saved in the videos table with the platform tag
8. Admin can view and delete uploaded videos from the dashboard
```

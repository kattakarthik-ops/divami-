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
| Frontend | React +  CSS |
| Backend | Spring Boot (Java 17) |
| Database | MySQL 8 |
| File Uploads | Spring Multipart |

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

---

# Archery Shop — Feature Flows

> Visual reference for the user purchase journey and admin workflow.

---

## 1. User purchase flow

```
Register / Log in
       ↓
Browse catalog
       ↓
Add items to cart
       ↓
Payment page
       ↓
Upload payment proof
       ↓
Order ID generated
```

| Step | Action | Detail |
|---|---|---|
| Auth | Register / log in | JWT token issued on success |
| Shop | Browse catalog | Search and filter by category |
| Cart | Add items to cart | Update quantity, remove items |
| Pay | Payment page | View UPI ID and QR code |
| Proof | Upload payment proof | Screenshot or receipt |
| Order | Order ID generated | Track status and add comments |

---

## 2. Admin workflow

```
Admin login (role-protected)
           ↓
     Admin dashboard
    ↙    ↙    ↘    ↘
Products  Orders  Verify  Upload
                payment  video
           ↓        ↓
     Order detail + comment thread
     (admin and user communicate)
```

| Action | Description |
|---|---|
| Manage products | Add, edit, delete products with image upload |
| View orders | See all incoming orders with payment proof |
| Verify payment | Approve or reject each payment manually |
| Upload video | Tag video as Instagram / Facebook / YouTube |
| Comment thread | Admin and user communicate below each order |

---

## Flow summary

- **User** registers → browses → adds to cart → pays via UPI → uploads proof → gets Order ID → tracks and comments
- **Admin** logs in → manages products → verifies payments → communicates via comments → uploads promotional videos with platform tags


  ---

### In Scope
- User registration and login with role-based access
- Responsive product catalog with search and category filter
- Shopping cart with quantity management
- UPI-based manual payment with proof upload
- Auto-generated Order ID on payment submission
- Admin payment verification workflow
- Comment thread below each order for user-admin communication
- Admin product management with image upload
- Video upload with social media platform tagging

### Out of Scope
- Automated payment gateway integration (Razorpay, Stripe, etc.)
- Real-time notifications or live chat
- Social media API publishing (Instagram, Facebook, YouTube)
- Mobile application (iOS / Android)
- Multi-language support
- Discount codes or coupon system

---

## 2. Functional Specifications

### 2.1 Authentication

| ID | Requirement |
|---|---|
| AUTH-01 | User can register with name, email, and password |
| AUTH-02 | Passwords must be minimum 6 characters |
| AUTH-03 | Email must be unique across all users |
| AUTH-04 | Passwords are stored as BCrypt hashes — never plain text |
| AUTH-05 | Login returns a JWT token valid for 24 hours |
| AUTH-06 | Admin account is pre-seeded or created manually in the database |
| AUTH-07 | Role-based route protection — USER and ADMIN routes are separate |
| AUTH-08 | Expired or invalid tokens redirect to the login page |

---

### 2.2 Product Catalog

| ID | Requirement |
|---|---|
| CAT-01 | All products are publicly visible without login |
| CAT-02 | Products display name, image, price, category, and stock status |
| CAT-03 | Catalog supports keyword search across name and description |
| CAT-04 | Catalog supports filtering by product category |
| CAT-05 | Out-of-stock products are shown but cannot be added to cart |
| CAT-06 | Product detail page shows full description, image, and quantity selector |
| CAT-07 | Catalog layout is responsive — 1 column (mobile), 2 (tablet), 4 (desktop) |

---

### 2.3 Shopping Cart

| ID | Requirement |
|---|---|
| CART-01 | Only logged-in users can add items to cart |
| CART-02 | Adding the same product again increments quantity |
| CART-03 | User can increase or decrease item quantity |
| CART-04 | User can remove individual items from cart |
| CART-05 | User can clear the entire cart |
| CART-06 | Cart persists in the database — survives page refresh |
| CART-07 | Cart displays subtotal per item and total order value |
| CART-08 | Navbar shows live cart item count badge |

---

### 2.4 Payment

| ID | Requirement |
|---|---|
| PAY-01 | Payment page shows UPI ID, QR code image, and phone number |
| PAY-02 | User uploads a payment screenshot or receipt |
| PAY-03 | Accepted upload formats: JPG, PNG, PDF |
| PAY-04 | Maximum upload file size: 5 MB |
| PAY-05 | System auto-generates a unique Order ID on upload (e.g. ORD-20240625-00123) |
| PAY-06 | Cart is cleared automatically after order is placed |
| PAY-07 | Order is created with status PENDING by default |

---

### 2.5 Order Management

| ID | Requirement |
|---|---|
| ORD-01 | User can view all their past orders with status |
| ORD-02 | Each order shows Order ID, items, total, date, and current status |
| ORD-03 | Admin can view all orders from all users |
| ORD-04 | Admin can view the uploaded payment proof for each order |
| ORD-05 | Admin can update order status: PENDING → VERIFIED / REJECTED / SHIPPED / DELIVERED |
| ORD-06 | Order statuses are: PENDING, VERIFIED, REJECTED, SHIPPED, DELIVERED |

---

### 2.6 Comment Thread

| ID | Requirement |
|---|---|
| CMT-01 | Each order has a comment thread visible to both user and admin |
| CMT-02 | Both user and admin can post comments on any order |
| CMT-03 | Comments display author name, message, and timestamp |
| CMT-04 | Comments are stored in the database and shown in chronological order |
| CMT-05 | Comment thread is accessible from the Order Detail page |

---

### 2.7 Admin — Product Management

| ID | Requirement |
|---|---|
| ADM-01 | Admin can add a new product with name, description, price, category, stock, and image |
| ADM-02 | Admin can edit any existing product |
| ADM-03 | Admin can delete a product (image is also removed from storage) |
| ADM-04 | Product image upload is optional — a placeholder is shown if no image |
| ADM-05 | Accepted image formats: JPG, PNG, WEBP |
| ADM-06 | Maximum image size: 10 MB |

---

### 2.8 Admin — Video Upload

| ID | Requirement |
|---|---|
| VID-01 | Admin can upload a promotional video file |
| VID-02 | Admin must select a platform tag: Instagram, Facebook, or YouTube |
| VID-03 | Admin must provide a title for each video |
| VID-04 | Accepted video formats: MP4, MOV, AVI |
| VID-05 | Maximum video file size: 20 MB |
| VID-06 | Uploaded videos are stored locally — not published to any platform |
| VID-07 | Admin can view and delete uploaded videos |

---

## 3. File Upload Specifications

| Type | Folder | Accepted Formats | Max Size |
|---|---|---|---|
| Product image | `/uploads/products/` | JPG, PNG, WEBP | 10 MB |
| Payment proof | `/uploads/payments/` | JPG, PNG, PDF | 5 MB |
| Promotional video | `/uploads/videos/` | MP4, MOV, AVI | 20 MB |

- Files are renamed to a UUID on upload to prevent conflicts
- Deleted products also remove their associated image file
- Files are served publicly at `/uploads/**`

---

## 4. Deliverables

### Phase 1 — Project setup
- [ ] Spring Boot project with MySQL + JPA + Security configured
- [ ] React + Vite + Tailwind project scaffolded
- [ ] MySQL schema created and connected
- [ ] `application.properties` and `.env` files configured

### Phase 2 — Authentication
- [ ] User register API (`POST /api/auth/register`)
- [ ] User login API (`POST /api/auth/login`)
- [ ] JWT generation and validation
- [ ] Spring Security config with role-based protection
- [ ] React login page
- [ ] React register page
- [ ] Auth context with persistent login state
- [ ] Protected and admin route guards

### Phase 3 — Catalog & cart
- [ ] Product CRUD APIs (admin only for write operations)
- [ ] Product search and category filter APIs
- [ ] Image upload and file storage service
- [ ] Cart APIs (add, update, remove, clear)
- [ ] Responsive catalog page with search and filter
- [ ] Product detail page with quantity selector
- [ ] Cart page with order summary
- [ ] Navbar with cart item count badge

### Phase 4 — Payment & orders
- [ ] Order creation API with payment proof upload
- [ ] Auto-generated Order ID
- [ ] Cart cleared on order placement
- [ ] My Orders page (user view)
- [ ] Admin orders list with payment proof viewer
- [ ] Order status update API (admin)
- [ ] Payment page with UPI ID, QR code, phone number

### Phase 5 — Admin dashboard
- [ ] Admin product management UI (add, edit, delete)
- [ ] Admin orders management UI
- [ ] Payment verification UI (approve / reject)
- [ ] Video upload UI with platform selector
- [ ] Video management list

### Phase 6 — Comments
- [ ] Comment API (post and fetch per order)
- [ ] Comment thread UI below Order Detail page
- [ ] Author name and timestamp displayed per comment

### Phase 7 — Testing & deployment
- [ ] Frontend deployed to Vercel
- [ ] Backend deployed to Railway
- [ ] MySQL database hosted on Railway
- [ ] Environment variables configured on both platforms
- [ ] End-to-end flow tested: register → browse → cart → pay → verify → comment

---



*Arrow & Arc — Archery Shop · Specifications v1.0*

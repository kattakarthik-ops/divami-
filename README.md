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
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Archery Shop — Flows</title>
<style>
  body { font-family: system-ui, sans-serif; max-width: 860px; margin: 40px auto; padding: 0 24px; background: #fff; color: #1a1a1a; }
  h1 { font-size: 22px; font-weight: 600; margin-bottom: 4px; }
  p.sub { color: #666; font-size: 14px; margin-bottom: 40px; }
  h2 { font-size: 16px; font-weight: 600; margin: 40px 0 12px; color: #333; }
  .diagram { border: 1px solid #e5e5e5; border-radius: 12px; padding: 24px; margin-bottom: 32px; }
  svg text { font-family: system-ui, sans-serif; }
  .th { font-size: 14px; font-weight: 500; fill: #1a1a1a; }
  .ts { font-size: 12px; fill: #555; }
  .c-purple > rect { fill: #EEEDFE; stroke: #534AB7; }
  .c-purple .th { fill: #3C3489; }
  .c-purple .ts { fill: #534AB7; }
  .c-teal > rect   { fill: #E1F5EE; stroke: #0F6E56; }
  .c-teal .th { fill: #085041; }
  .c-teal .ts { fill: #0F6E56; }
  .c-amber > rect { fill: #FAEEDA; stroke: #854F0B; }
  .c-amber .th { fill: #633806; }
  .c-amber .ts { fill: #854F0B; }
  .c-green > rect { fill: #EAF3DE; stroke: #3B6D11; }
  .c-green .th { fill: #27500A; }
  .c-green .ts { fill: #3B6D11; }
  .c-coral > rect { fill: #FAECE7; stroke: #993C1D; }
  .c-coral .th { fill: #712B13; }
  .c-coral .ts { fill: #993C1D; }
  .c-pink > rect   { fill: #FBEAF0; stroke: #993556; }
  .c-pink .th { fill: #72243E; }
  .c-pink .ts { fill: #993556; }
  .c-gray > rect   { fill: #F1EFE8; stroke: #5F5E5A; }
  .c-gray .th { fill: #444441; }
  .c-gray .ts { fill: #5F5E5A; }
</style>
</head>
<body>

<h1>Archery Shop — Feature Flows</h1>
<p class="sub">User purchase flow and admin workflow</p>

<!-- DIAGRAM 1: User Purchase Flow -->
<h2>1. User purchase flow</h2>
<div class="diagram">
<svg width="100%" viewBox="0 0 680 530">
  <defs>
    <marker id="a1" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#888" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <g class="c-purple"><rect x="220" y="20" width="240" height="56" rx="10" stroke-width="0.5"/><text class="th" x="340" y="42" text-anchor="middle" dominant-baseline="central">Register / log in</text><text class="ts" x="340" y="62" text-anchor="middle" dominant-baseline="central">JWT token issued on success</text></g>
  <line x1="340" y1="76" x2="340" y2="106" stroke="#888" stroke-width="1.5" marker-end="url(#a1)"/>

  <g class="c-teal"><rect x="220" y="108" width="240" height="56" rx="10" stroke-width="0.5"/><text class="th" x="340" y="130" text-anchor="middle" dominant-baseline="central">Browse catalog</text><text class="ts" x="340" y="150" text-anchor="middle" dominant-baseline="central">Search, filter by category</text></g>
  <line x1="340" y1="164" x2="340" y2="194" stroke="#888" stroke-width="1.5" marker-end="url(#a1)"/>

  <g class="c-teal"><rect x="220" y="196" width="240" height="56" rx="10" stroke-width="0.5"/><text class="th" x="340" y="218" text-anchor="middle" dominant-baseline="central">Add items to cart</text><text class="ts" x="340" y="238" text-anchor="middle" dominant-baseline="central">Update qty, remove items</text></g>
  <line x1="340" y1="252" x2="340" y2="282" stroke="#888" stroke-width="1.5" marker-end="url(#a1)"/>

  <g class="c-amber"><rect x="220" y="284" width="240" height="56" rx="10" stroke-width="0.5"/><text class="th" x="340" y="306" text-anchor="middle" dominant-baseline="central">Payment page</text><text class="ts" x="340" y="326" text-anchor="middle" dominant-baseline="central">View UPI ID, QR code</text></g>
  <line x1="340" y1="340" x2="340" y2="370" stroke="#888" stroke-width="1.5" marker-end="url(#a1)"/>

  <g class="c-amber"><rect x="220" y="372" width="240" height="56" rx="10" stroke-width="0.5"/><text class="th" x="340" y="394" text-anchor="middle" dominant-baseline="central">Upload payment proof</text><text class="ts" x="340" y="414" text-anchor="middle" dominant-baseline="central">Screenshot / receipt</text></g>
  <line x1="340" y1="428" x2="340" y2="458" stroke="#888" stroke-width="1.5" marker-end="url(#a1)"/>

  <g class="c-green"><rect x="220" y="460" width="240" height="56" rx="10" stroke-width="0.5"/><text class="th" x="340" y="482" text-anchor="middle" dominant-baseline="central">Order ID generated</text><text class="ts" x="340" y="502" text-anchor="middle" dominant-baseline="central">Track status + add comments</text></g>

  <text class="ts" x="54" y="50" text-anchor="middle" dominant-baseline="central">Auth</text>
  <line x1="80" y1="48" x2="218" y2="48" stroke="#bbb" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="54" y="136" text-anchor="middle" dominant-baseline="central">Shop</text>
  <line x1="80" y1="136" x2="218" y2="136" stroke="#bbb" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="54" y="224" text-anchor="middle" dominant-baseline="central">Cart</text>
  <line x1="80" y1="224" x2="218" y2="224" stroke="#bbb" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="54" y="312" text-anchor="middle" dominant-baseline="central">Pay</text>
  <line x1="80" y1="312" x2="218" y2="312" stroke="#bbb" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="54" y="400" text-anchor="middle" dominant-baseline="central">Proof</text>
  <line x1="80" y1="400" x2="218" y2="400" stroke="#bbb" stroke-width="0.5" stroke-dasharray="4 3"/>
  <text class="ts" x="54" y="488" text-anchor="middle" dominant-baseline="central">Order</text>
  <line x1="80" y1="488" x2="218" y2="488" stroke="#bbb" stroke-width="0.5" stroke-dasharray="4 3"/>
</svg>
</div>

<!-- DIAGRAM 2: Admin Workflow -->
<h2>2. Admin workflow</h2>
<div class="diagram">
<svg width="100%" viewBox="0 0 680 400">
  <defs>
    <marker id="a2" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
      <path d="M2 1L8 5L2 9" fill="none" stroke="#888" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    </marker>
  </defs>

  <g class="c-coral"><rect x="240" y="20" width="200" height="50" rx="10" stroke-width="0.5"/><text class="th" x="340" y="40" text-anchor="middle" dominant-baseline="central">Admin login</text><text class="ts" x="340" y="58" text-anchor="middle" dominant-baseline="central">Role-protected route</text></g>
  <line x1="340" y1="70" x2="340" y2="104" stroke="#888" stroke-width="1.5" marker-end="url(#a2)"/>

  <g class="c-gray"><rect x="240" y="106" width="200" height="44" rx="10" stroke-width="0.5"/><text class="th" x="340" y="128" text-anchor="middle" dominant-baseline="central">Admin dashboard</text></g>

  <path d="M280 150 L140 204" fill="none" stroke="#888" stroke-width="1.5" marker-end="url(#a2)"/>
  <path d="M310 150 L260 204" fill="none" stroke="#888" stroke-width="1.5" marker-end="url(#a2)"/>
  <path d="M370 150 L420 204" fill="none" stroke="#888" stroke-width="1.5" marker-end="url(#a2)"/>
  <path d="M400 150 L540 204" fill="none" stroke="#888" stroke-width="1.5" marker-end="url(#a2)"/>

  <g class="c-teal"><rect x="50" y="206" width="170" height="56" rx="10" stroke-width="0.5"/><text class="th" x="135" y="226" text-anchor="middle" dominant-baseline="central">Manage products</text><text class="ts" x="135" y="246" text-anchor="middle" dominant-baseline="central">Add, edit, delete + image</text></g>
  <g class="c-teal"><rect x="232" y="206" width="170" height="56" rx="10" stroke-width="0.5"/><text class="th" x="317" y="226" text-anchor="middle" dominant-baseline="central">View orders</text><text class="ts" x="317" y="246" text-anchor="middle" dominant-baseline="central">See payment proof</text></g>
  <g class="c-amber"><rect x="334" y="206" width="170" height="56" rx="10" stroke-width="0.5"/><text class="th" x="419" y="226" text-anchor="middle" dominant-baseline="central">Verify payment</text><text class="ts" x="419" y="246" text-anchor="middle" dominant-baseline="central">Approve or reject</text></g>
  <g class="c-pink"><rect x="460" y="206" width="170" height="56" rx="10" stroke-width="0.5"/><text class="th" x="545" y="226" text-anchor="middle" dominant-baseline="central">Upload video</text><text class="ts" x="545" y="246" text-anchor="middle" dominant-baseline="central">Tag: IG / FB / YouTube</text></g>

  <line x1="317" y1="262" x2="317" y2="308" stroke="#888" stroke-width="1.5" marker-end="url(#a2)"/>
  <line x1="419" y1="262" x2="419" y2="308" stroke="#888" stroke-width="1.5" marker-end="url(#a2)"/>

  <g class="c-green"><rect x="204" y="310" width="328" height="56" rx="10" stroke-width="0.5"/><text class="th" x="368" y="330" text-anchor="middle" dominant-baseline="central">Order detail + comment thread</text><text class="ts" x="368" y="350" text-anchor="middle" dominant-baseline="central">Admin and user communicate per order</text></g>
</svg>
</div>

</body>
</html>

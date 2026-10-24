# 📦 SmartWare — Full-Stack E-Commerce & Smart Warehouse Fulfillment Platform

SmartWare is a production-grade, full-stack web application combining an **Amazon-inspired customer shopping experience** with an integrated, automated **Smart Warehouse Management System (WMS)**.

Rather than treating e-commerce and inventory management as separate silos, SmartWare links every customer action (searching, adding to cart, placing an order) directly to real-time warehouse bin allocations, reserve states, wave pick paths, and package telemetry.

---

## 🌟 Key Capabilities & Architecture

### 🛒 1. Customer E-Commerce Experience (Amazon-Style)
- **High-Density Navigation**: Department search with category picker, "Deliver to [Alex, Boston 02134]", interactive 1-click demo role switcher, returns & orders, and real-time cart item counter badge.
- **Deals & Department Showcase**: Banners, Deals of the Day with discount percentages, and curated categories.
- **Advanced Catalog & Filtering**: Real-time search with department filters, minimum customer review star ratings, price range filters, stock availability toggles, and "SmartWare Choice" tags.
- **Rich Product Detail Pages (PDP)**:
  - Multi-image gallery with thumbnail switcher.
  - Live warehouse stock status (*"Only 4 left in stock - order soon"* or *"In Stock: 84 units in Boston FC-01"*).
  - Exact facility and bin location telemetry (*"FC-BOS-01, Zone A, Bin 101"*).
  - Detailed technical specifications table.
  - Customer review breakdown (ratings distribution bar chart) and review submission modal.
  - Buy Box with Express & Same-Day delivery availability.
- **Cart & Slide-Over Drawer**: Real-time quantity steppers, subtotal computation, and free delivery progress threshold bar.
- **Amazon-Style Checkout**: Multi-step checkout with saved address selection, speed options (Standard Free, Express $9.99, Same-Day $14.99), payment methods (Credit Card, SmartWare Pay, COD, UPI), and **real-time database inventory reservation**.
- **Customer Order History & Live Tracker**:
  - Live Amazon-style tracker timeline:
    $$\text{Order Verified} \longrightarrow \text{Warehouse Picking} \longrightarrow \text{Packed \& Inspected} \longrightarrow \text{Dispatched from FC} \longrightarrow \text{Out for Delivery} \longrightarrow \text{Delivered}$$
  - Interactive *"Advance Next FC Step"* simulator to observe the order progress live through fulfillment stages.

---

### 🏭 2. Smart Warehouse Management System (WMS)
- **WMS Command Dashboard**:
  - Key Performance Indicators: Total Inventory Valuation, Units on Hand, Reserved Units, Facility Utilization %, Low-Stock SKU alerts, and Pending Wave Picks.
  - **Recharts Data Visualizations**: Daily fulfillment throughput trends (Picked vs. Dispatched vs. Inbound Intake), zone inventory density, and order fulfillment status distribution.
- **2D Interactive Facility Floor Map**:
  - Interactive spatial floor plan of **FC-BOS-01** (Boston Metro Fulfillment Center).
  - Visual zones: Zone A (High-Velocity Electronics), Zone B (Smart Home & IoT), Zone C (Heavy Industrial).
  - Individual bin slots with occupancy telemetry, heat status (Stocked, Low Stock, Empty), barcode tags, and click-to-inspect drawers.
- **Master SKU Inventory & Quick Adjustment**:
  - Full inventory table with SKU, Barcode, Bin location, On-Hand, Reserved, and Available counts.
  - Cycle count modal with positive/negative quantity adjustments, reason tags (`CYCLE_COUNT`, `RECEIVING`, `DAMAGE`, `TRANSFER`), and complete audit trail logging.
- **Wave Picking & Packing Station**:
  - FIFO queue of pending customer orders.
  - Automated **Aisle Path Optimization**: items sequenced by bin location (*Zone $\rightarrow$ Aisle $\rightarrow$ Shelf $\rightarrow$ Bin*) to minimize walking distance for pickers.
  - Interactive pick checklist (mark items picked with instant visual confirmation).
  - Packing verification and one-click dispatch with **automated database stock deduction and 4x6 thermal shipping label generation**.
- **Inbound Receiving (Purchase Orders)**:
  - PO intake with QA inspection and automated bin putaway.
- **Audit Logs & Telemetry**:
  - Complete history of all warehouse events, user operators, reasons, and unit changes.

---

## 🛠️ Technology Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | React 18, TypeScript, Vite, Tailwind CSS, Lucide React, Recharts |
| **Routing** | React Router DOM v6 (Storefront & WMS sub-routes) |
| **Backend** | Node.js, Express, TypeScript (`tsx`), Morgan, CORS |
| **Database** | SQLite (default zero-dependency local) or PostgreSQL |
| **ORM** | Prisma ORM 5.x (with automated migrations & rich seed data) |
| **Auth & Security** | JWT (JSON Web Tokens), Bcrypt password hashing, Role-Based Access Control |
| **Validation** | Zod (Type-safe request body validation on server) |
| **Testing** | Vitest, Supertest (13 integration tests covering Auth, Products, Inventory RBAC, Orders) |

---

## 👥 Pre-Seeded Demo Accounts & 1-Click Persona Switcher

The application includes an interactive **1-Click Persona Switcher** in the top navigation bar, allowing you to instantly switch roles without re-typing credentials:

| Persona | Name | Role | Email | Password |
| :--- | :--- | :--- | :--- | :--- |
| **Customer** | Alex Mercer | `CUSTOMER` | `customer@smartware.com` | `Password123!` |
| **Warehouse Picker** | Elena Rostova | `WAREHOUSE_STAFF` | `picker@smartware.com` | `Password123!` |
| **Warehouse Manager** | Marcus Vance | `WAREHOUSE_MANAGER` | `manager@smartware.com` | `Password123!` |
| **Administrator** | Sarah Connor | `ADMIN` | `admin@smartware.com` | `Password123!` |

---

## 🚀 Getting Started

### 1. Prerequisites
- **Node.js** v18+ or v22+
- **npm** v9+ or v10+

### 2. Install Dependencies
Dependencies are already installed, but to install cleanly across both projects:
```bash
npm run install:all
```

### 3. Database Sync & Seed
To sync the Prisma schema and seed demo users, warehouses, zones, products, reviews, and sample orders:
```bash
npm run db:push
npm run db:seed
```

### 4. Run the Backend API Server
```bash
npm run dev:server
```
The Express API runs at **`http://localhost:5000`** with health check at `http://localhost:5000/api/health`.

### 5. Run the Frontend Client
In a separate terminal:
```bash
npm run dev:client
```
The Vite development server runs at **`http://localhost:5173`** with an automated API proxy to port 5000.

### 6. Run Test Suite
To execute Vitest API integration tests:
```bash
npm run test:server
```
All 13 integration tests will run against the API, verifying authentication, RBAC, inventory reservation, and product queries.

---

## 📂 Repository Structure

```
smartware/
├── package.json                   # Root orchestrator scripts
├── README.md                      # Documentation & project manual
├── server/
│   ├── package.json
│   ├── tsconfig.json
│   ├── vitest.config.ts           # Vitest configuration
│   ├── .env                       # Environment variables (PORT, JWT_SECRET, DATABASE_URL)
│   ├── prisma/
│   │   ├── schema.prisma          # Database schema (User, Product, Warehouse, Zone, Order, etc.)
│   │   └── seed.ts                # Rich seed dataset generator
│   ├── src/
│   │   ├── app.ts                 # Express application & route mounting
│   │   ├── server.ts              # HTTP listener
│   │   ├── config/prisma.ts       # Prisma Client instance
│   │   ├── middleware/            # JWT auth, RBAC requireRole, Zod errorHandler
│   │   ├── routes/                # auth, product, cart, order, warehouse, inventory, analytics, review
│   │   └── utils/                 # JWT helper
│   └── tests/
│       └── api.test.ts            # Supertest + Vitest integration tests
└── client/
    ├── package.json
    ├── vite.config.ts             # Vite config with /api proxy & aliases
    ├── tsconfig.json
    ├── tailwind.config.js
    ├── index.html
    └── src/
        ├── App.tsx                # App routing (Storefront & WMS)
        ├── main.tsx
        ├── index.css              # Tailwind base & custom styles
        ├── types/index.ts         # TypeScript data contracts
        ├── services/api.ts        # Typed fetch client with auth token injection
        ├── context/
        │   ├── AuthContext.tsx    # Auth state, login, 1-click persona switcher
        │   └── CartContext.tsx    # Cart state & drawer controller
        ├── components/
        │   ├── layout/            # Navbar, SubNav, Footer, WarehouseSidebar
        │   ├── ecommerce/         # ProductCard, CartDrawer, ReviewModal
        │   └── warehouse/          # ZoneMap (2D floor plan), StockAdjustModal
        └── pages/
            ├── ecommerce/         # HomePage, ProductCatalog, ProductDetail, Cart, Checkout, Orders, Tracking
            ├── warehouse/         # Dashboard, Inventory, PickPack, Inbound, Zones, Reports
            └── auth/              # LoginPage, RegisterPage
```

---

## 🧪 Verified Integration Tests

The backend test suite (`server/tests/api.test.ts`) verifies:
1. `GET /api/health` — Healthy status verification.
2. `POST /api/auth/login` — Bad credentials rejection (401).
3. `GET /api/auth/me` — JWT verification and profile payload.
4. `GET /api/products` — Product catalog retrieval with available stock calculation.
5. `GET /api/products?category=Electronics` — Category filtering.
6. `GET /api/products/:id` — Single product details with bin allocations and customer reviews.
7. `GET /api/products/categories` — Distinct categories with counts.
8. `GET /api/inventory` (RBAC) — Forbidden (403) for regular customer.
9. `GET /api/inventory` (RBAC) — Authorized (200) for Warehouse Manager.
10. `GET /api/warehouse` — Warehouse metrics and capacity utilization.
11. `GET /api/analytics/warehouse` — Full dashboard metrics and charts.
12. `GET /api/cart` — Cart items and subtotal computation.
13. `POST /api/orders` — End-to-end checkout transaction with inventory reservation and initial tracking steps.

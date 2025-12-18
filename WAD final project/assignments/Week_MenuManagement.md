# Week Assignment: Menu Management

## Overview

This week focuses on implementing the **Menu Management** module for the Smart Restaurant system. You will build functionality that allows restaurant admins to manage menu categories, menu items, item photos, availability status, and item modifiers (e.g., size, extras).

This module directly supports the guest ordering experience (QR menu browsing) and is a foundation for analytics (popular items, top revenue).

---

## Learning Objectives

By the end of this assignment, you will be able to:
- Design and implement CRUD operations for menu categories and menu items
- Implement validation for pricing, status, and business rules
- Support item modifiers (modifier groups and options)
- Upload and manage item photos
- Build an admin UI with filtering, sorting, and pagination

---

## Prerequisites

- Completed authentication module (Admin login)
- Database setup and migrations workflow ready
- Basic understanding of file upload handling (multipart/form-data)
- Basic understanding of pagination and filtering patterns

---

## Feature Requirements

### A. Alignment to self-assessment feature list (Admin Menu Management)

This assignment is designed to match the **Administration features (Restaurant Admin)** items in your self-assessment table:

| Self-assessment feature | Where covered in this assignment |
|---|---|
| Manage menu categories | Section 1 |
| View menu item list | Section 2.2 |
| Filter menu items by name, category | Section 2.2 |
| Sort menu items by creation time, price, popularity | Section 2.2 + Section 6 (popularity note) |
| Create a new menu item | Section 2.1 |
| Specify menu item status | Section 2.1 |
| Verify user input (create/update) | Validation blocks in Sections 1–4 + Section 6 |
| Update a menu item | Section 2.3 |
| Change menu item category, modifiers | Section 2.3 + Section 4.3 |
| Update menu item status | Section 2.3 |
| Upload multiple menu item photos | Section 3.1 |
| Add, remove menu item photos | Section 3.2 |
| Add menu item to category with modifiers | Section 2.1 + Section 4.3 |
| Menu Item Modifiers | Section 4 |

### 1. Menu Categories CRUD (0.5 points)

#### 1.1 Create Category
- **Fields:**
  - Name (required, unique within restaurant)
  - Description (optional)
  - Display order (integer, optional)
  - Status (Active/Inactive)

- **Validation:**
  - Name is required, 2–50 characters
  - Unique per restaurant
  - Display order must be a non-negative integer

#### 1.2 View Categories
- List all categories with:
  - Name, status, display order, number of items
- Sort categories by:
  - Display order (default), name, creation date

#### 1.3 Update Category
- Update name/description/order/status
- If category is set to Inactive:
  - Items under it remain in the database
  - Items may be hidden from the guest menu (depending on item status; see business rules)

#### 1.4 Delete Category (Soft Delete)
- Soft delete or mark as inactive (recommended)
- Must prevent deletion if:
  - Category still contains active items (unless using soft delete)

---

### 2. Menu Item CRUD (1.5 points)

#### 2.1 Create Menu Item
- **Fields:**
  - Name (required)
  - Category (required)
  - Price (required)
  - Description (optional)
  - Preparation time in minutes (optional)
  - Status: Available / Unavailable / Sold out (required)
  - Chef recommendation (boolean, optional)

- **Validation:**
  - Name is required, 2–80 characters
  - Price must be a positive number (e.g., 0.01 to 999999)
  - Preparation time must be a non-negative integer (0–240 suggested)
  - Category must exist and belong to the same restaurant

#### 2.2 View Menu Item List (Admin)
- Display items in a table/grid with:
  - Name, category, price, status, chef recommendation, created date
- Support:
  - **Filter** by name (contains), category, status
  - **Sort** by creation time, price, popularity (if tracked)
  - **Pagination** (page/limit)

#### 2.3 Update Menu Item
- Update fields: name, category, price, description, prep time, status, chef recommendation
- Must allow changing:
  - Category (move item between categories)
  - Status (Available / Unavailable / Sold out)
- Must validate business rules (see section 6)

#### 2.4 Delete Menu Item (Soft Delete)
- Soft delete recommended
- Item should no longer appear in guest menu after deletion
- Order history must remain intact (do not delete historical order items)

---

### 3. Menu Item Photos (0.5 points)

#### 3.1 Upload Photos
- Upload **multiple photos** per menu item
- Requirements:
  - Accepted formats: JPG/PNG/WebP
  - Max file size per image (e.g., 2–5MB)
  - Store file path/URL in DB

#### 3.2 Manage Photos
- Admin can:
  - Add new photos
  - Remove a photo
  - Set a primary photo (first image shown in guest menu)

#### 3.3 Security & Validation
- Validate MIME type and file extension
- Use randomized filenames and safe storage paths
- Do not allow arbitrary path writes

---

### 4. Menu Item Modifiers (1.0 points)

Modifiers allow customization of an item, for example:
- **Size:** Small/Medium/Large (+$0 / +$1 / +$2)
- **Extras:** Cheese (+$0.5), Bacon (+$1)

#### 4.1 Create Modifier Groups
- **Fields:**
  - Group name (required)
  - Selection type: single-select or multi-select
  - Required (boolean)
  - Min selections / max selections (optional; mainly for multi-select)
  - Display order

- **Validation:**
  - Group name required
  - If required=true:
    - single-select must have exactly 1 chosen
    - multi-select must respect min/max

#### 4.2 Create Modifier Options
- **Fields:**
  - Option name (required)
  - Price adjustment (can be 0; can be positive)
  - Status (Active/Inactive)

#### 4.3 Attach Modifiers to Items
- An item can have 0..N modifier groups
- Guest ordering must be able to calculate:
  - Item base price + sum of selected modifier adjustments

---

### 5. Guest Menu Consumption (Read-only) (0.5 points)

Implement an endpoint (or service layer) for the guest menu to load:
- Categories (active)
- Items (active/available rules)
- Primary photo
- Modifier groups and options

To support the guest features in your self-assessment (menu browsing/search/filter/paging/sort), the endpoint should accept query parameters such as:
- Search by item name (e.g., `q`)
- Filter by category (e.g., `categoryId`)
- Sort by popularity (e.g., `sort=popularity`)
- Filter/highlight chef recommendations (e.g., `chefRecommended=true`)
- Pagination (e.g., `page`, `limit`)

Must be scoped to a restaurant and usable by a QR flow.

---

### 6. Business Rules & Input Validation (Required)

- A menu item is visible to guests only when:
  - Category is Active
  - Item is not deleted
  - Item status is **Available** (recommended) OR show Unavailable/Sold out with disabled ordering (if your UX supports it)

- Sold out items:
  - Must not be addable to cart
  - Should be clearly labeled

- Popularity sorting (admin/guest):
  - If you implement `sort=popularity`, define popularity as a measurable field (e.g., total quantity ordered)
  - It can be stored as a cached counter on `menu_items` or computed from `order_items` (trade-off: speed vs accuracy)

- Verify user input:
  - Server-side validation is mandatory for create/update endpoints
  - Return consistent validation errors (400) with field-level messages
  - Do not trust client-provided `restaurantId`/ownership; derive tenant scope from authenticated admin session

- Deleting a category/item must not delete historical order records.

---

## Technical Specifications (Suggested)

### Database Schema (Example)

> Use UUIDs if your project already uses them.

```sql
-- Categories
CREATE TABLE menu_categories (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    restaurant_id UUID NOT NULL,
    name VARCHAR(50) NOT NULL,
    description TEXT,
    display_order INT DEFAULT 0,
    status VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    UNIQUE (restaurant_id, name)
);

CREATE INDEX idx_menu_categories_restaurant ON menu_categories(restaurant_id);
CREATE INDEX idx_menu_categories_status ON menu_categories(status);

-- Items
CREATE TABLE menu_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    restaurant_id UUID NOT NULL,
    category_id UUID NOT NULL,
    name VARCHAR(80) NOT NULL,
    description TEXT,
    price DECIMAL(12,2) NOT NULL CHECK (price > 0),
    prep_time_minutes INT DEFAULT 0 CHECK (prep_time_minutes >= 0 AND prep_time_minutes <= 240),
    status VARCHAR(20) NOT NULL CHECK (status IN ('available', 'unavailable', 'sold_out')),
    is_chef_recommended BOOLEAN DEFAULT FALSE,
    is_deleted BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_menu_items_restaurant ON menu_items(restaurant_id);
CREATE INDEX idx_menu_items_category ON menu_items(category_id);
CREATE INDEX idx_menu_items_status ON menu_items(status);

-- Item photos
CREATE TABLE menu_item_photos (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    menu_item_id UUID NOT NULL,
    url TEXT NOT NULL,
    is_primary BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_menu_item_photos_item ON menu_item_photos(menu_item_id);

-- Modifier groups
CREATE TABLE modifier_groups (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    restaurant_id UUID NOT NULL,
    name VARCHAR(80) NOT NULL,
    selection_type VARCHAR(20) NOT NULL CHECK (selection_type IN ('single', 'multiple')),
    is_required BOOLEAN DEFAULT FALSE,
    min_selections INT DEFAULT 0,
    max_selections INT DEFAULT 0,
    display_order INT DEFAULT 0,
    status VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Modifier options
CREATE TABLE modifier_options (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    group_id UUID NOT NULL,
    name VARCHAR(80) NOT NULL,
    price_adjustment DECIMAL(12,2) DEFAULT 0 CHECK (price_adjustment >= 0),
    status VARCHAR(20) DEFAULT 'active' CHECK (status IN ('active', 'inactive')),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_modifier_options_group ON modifier_options(group_id);

-- Attach groups to items
CREATE TABLE menu_item_modifier_groups (
    menu_item_id UUID NOT NULL,
    group_id UUID NOT NULL,
    PRIMARY KEY (menu_item_id, group_id)
);
```

---

### API Endpoints (Suggested)

> Prefixes may vary based on your architecture.

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/admin/menu/categories` | List categories (filter/sort) |
| POST | `/api/admin/menu/categories` | Create category |
| PUT | `/api/admin/menu/categories/:id` | Update category |
| PATCH | `/api/admin/menu/categories/:id/status` | Activate/Deactivate category |
| GET | `/api/admin/menu/items` | List items (filter/sort/paging) |
| GET | `/api/admin/menu/items/:id` | Item details |
| POST | `/api/admin/menu/items` | Create item |
| PUT | `/api/admin/menu/items/:id` | Update item |
| DELETE | `/api/admin/menu/items/:id` | Soft delete item |
| POST | `/api/admin/menu/items/:id/photos` | Upload one/multiple photos |
| DELETE | `/api/admin/menu/items/:id/photos/:photoId` | Remove photo |
| PATCH | `/api/admin/menu/items/:id/photos/:photoId/primary` | Set primary photo |
| POST | `/api/admin/menu/modifier-groups` | Create modifier group |
| POST | `/api/admin/menu/modifier-groups/:id/options` | Create option |
| PUT | `/api/admin/menu/modifier-groups/:id` | Update group |
| PUT | `/api/admin/menu/modifier-options/:id` | Update option |
| POST | `/api/admin/menu/items/:id/modifier-groups` | Attach/detach groups to item |
| GET | `/api/menu` | Guest menu load (restaurant-scoped) |

---

### Suggested Libraries

**Backend (Node.js):**
- `multer` (or equivalent) for uploads
- `zod` / `joi` / `yup` for request validation
- `sharp` for image resizing/compression (optional)

**Frontend (React):**
- React Hook Form + Zod/Yup for form validation
- A table/grid component pattern for paging/filtering UI

---

## Deliverables

1. **Source Code**
   - Backend API for categories/items/modifiers/photos
   - Admin UI screens for menu management
   - Database migrations and seeds (optional but recommended)

2. **Documentation**
   - API documentation for menu endpoints
   - Description of menu visibility rules (guest)

3. **Demo**
   - Admin creates categories and items
   - Upload photos and set primary
   - Add modifiers and attach to items
   - Guest menu loads and displays final results

---

## Grading Criteria

| Criteria | Points | Description |
|----------|--------|-------------|
| Category CRUD | 2 | Create, list, update, deactivate with validation |
| Item CRUD | 4 | Create, list with filter/sort/paging, update, soft delete |
| Photos | 2 | Multi-upload, remove, set primary, safe validation |
| Modifiers | 2 | Groups/options + attach to items + pricing rule support |
| **Total** | **10** | |

---

## Resources

- OWASP File Upload Cheat Sheet (concepts for safe uploads)
- Express Multer Documentation
- Sharp Documentation (image processing)
- JWT/Role-based authorization docs used in your project

---

## Tips

1. **Start simple:** categories + items first, then photos, then modifiers.

2. **Validate on both sides:** server-side is mandatory; client-side improves UX.

3. **Soft delete everything user-facing:** keep order history intact.

4. **Avoid N+1 queries:** when listing items, fetch category name and primary photo efficiently.

5. **Guest menu should be fast:** it’s the most frequently called endpoint.

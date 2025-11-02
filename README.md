# 🛍️ 6sense-Technologies-Assignment

# Database Design - Data Model Diagram

![Database ER Diagram](./Database%20ER%20diagram.png)

### Entity Relationship Diagram
- **One Category** → **Many Products**
- `productCode` is **globally unique**
- `categoryId` is a **foreign key** referencing `Category._id`






 API Documentation

This document e API endpoints for managing **Categories** and **Products**.

---

## 💻 Base URL

The API is hosted locally at: `http://localhost:3000`

---

## 📂 Category Endpoints

### 1. Create a New Category (`POST /category`)

Creates a new product category.

* **URL:** `http://localhost:3000/category`
* **Method:** `POST`
* **Request Body (JSON):**
    ```json
    {
      "name": "laptop"
    }
    ```
* **Success Response (201 Created):**
    ```json
    {
      "name": "laptop",
      "_id": "690752e436b9886a8a4e2f60",
      "createdAt": "2025-11-02T12:47:32.782Z",
      "updatedAt": "2025-11-02T12:47:32.782Z",
      "__v": 0
    }
    ```

### 2. Get All Categories (`GET   /category`)

Retrieves a list of all product categories.

* **URL:** `http://localhost:3000/category`
* **Method:** `GET`
* **Success Response (200 OK):**
    ```json
    [
      {
        "_id": "690752d436b9886a8a4e2f5a",
        "name": "Electronics",
        "createdAt": "2025-11-02T12:47:16.177Z",
        "updatedAt": "2025-11-02T12:47:16.177Z",
        "__v": 0
      },
      {
        "_id": "690752dd36b9886a8a4e2f5d",
        "name": "phone",
        "createdAt": "2025-11-02T12:47:25.140Z",
        "updatedAt": "2025-11-02T12:47:25.140Z",
        "__v": 0
      },
      {
        "_id": "690752e436b9886a8a4e2f60",
        "name": "laptop",
        "createdAt": "2025-11-02T12:47:32.782Z",
        "updatedAt": "2025-11-02T12:47:32.782Z",
        "__v": 0
      }
      // ... more categories
    ]
    ```

---

## 📦 Product Endpoints

### 1. Create a New Product (`POST    /products`)

Creates a new product and links it to a category.

* **URL:** `http://localhost:3000/products`
* **Method:** `POST`
* **Request Body (JSON):**
    > **Note:** The `category` field requires a valid Category `_id`. Example uses **Electronics** ID: `690752d436b9886a8a4e2f5a`.
    ```json
    {
      "name": "Alpha Sorter",
      "description": "Test product for NestJS",
      "price": 1000,
      "discount": 10,
      "image": "[https://example.com/image.png](https://example.com/image.png)",
      "category": "690752d436b9886a8a4e2f5a" 
    }
    ```
* **Success Response (201 Created):**
    ```json
    {
      "name": "Alpha Sorter",
      "description": "Test product for NestJS",
      "price": 1000,
      "discount": 10,
      "image": "[https://example.com/image.png](https://example.com/image.png)",
      "status": "In Stock",
      "productCode": "355e7f05-0alport9",
      "category": "690752d436b9886a8a4e2f5a",
      "_id": "6907555436b9886a8a4e2f6c",
      "createdAt": "2025-11-02T12:57:56.856Z",
      "updatedAt": "2025-11-02T12:57:56.856Z",
      "__v": 0
    }
    ```

### 2. Update a Product (`PUT /products/:productId`)

Updates details of an existing product using its ID.

* **URL:** `http://localhost:3000/products/{productId}` (e.g., `/products/6907562936b9886a8a4e2f76`)
* **Method:** `PUT`
* **Request Body (JSON):**
    > Only fields to be updated are required.
    ```json
    {
      "status": "Stock Out",
      "description": "Updated description",
      "discount": 15
    }
    ```
* **Success Response (200 OK):**
    ```json
    {
      "_id": "6907562936b9886a8a4e2f76",
      "name": "hp",
      "description": "Updated description",
      "price": 1000,
      "discount": 15,
      "image": "[https://example.com/image.png](https://example.com/image.png)",
      "status": "Stock Out",
      "productCode": "93969d19-0hp1",
      "category": "690752e436b9886a8a4e2f60",
      "createdAt": "2025-11-02T13:01:29.943Z",
      "updatedAt": "2025-11-02T13:02:06.842Z",
      "__v": 0
    }
    ```

### 3. Get Products (List and Search) (`GET /products`)

Retrieves a list of products and supports filtering via query parameters.

* **URL:** `http://localhost:3000/products`
* **Method:** `GET`

#### 🔎 Search and Filtering Options:

| Parameter | Example Value | Description |
| :--- | :--- | :--- |
| `category` | `690752e436b9886a8a4e2f60` | Filters products by Category ID (`laptop`). |
| `name` | `alpha` | Searches for products whose name contains the query (case-insensitive). |

#### **Example 1: Get All Products**

* **URL:** `http://localhost:3000/products`
* **Response:**
    ```json
    [
      {
        "name": "Alpha Sorter",
        "description": "Test product for NestJS",
        "price": 1000,
        "discount": 10,
        "finalPrice": 900,
        "productCode": "355e7f05-0alport9",
        "category": "Electronics",
        "status": "In Stock"
      }
      // ... other products
    ]
    ```

#### **Example 2: Filter by Category**

* **URL:** `http://localhost:3000/products?category=690752e436b9886a8a4e2f60`
* **Response (Products in 'laptop' category):**
    ```json
    [
      {
        "name": "hp",
        "description": "Updated description",
        "price": 1000,
        "discount": 15,
        "finalPrice": 850,
        "productCode": "93969d19-0hp1",
        "category": "laptop",
        "status": "Stock Out"
      }
    ]
    ```

#### **Example 3: Search by Name**

* **URL:** `http://localhost:3000/products?name=alpha`
* **Response (Products with 'alpha' in name):**
    ```json
    [
      {
        "name": "Alpha Sorter",
        "description": "Test product for NestJS",
        "price": 1000,
        "discount": 10,
        "finalPrice": 900,
        "productCode": "355e7f05-0alport9",
        "category": "Electronics",
        "status": "In Stock"
      }
    ]
    ```

#### **Example 4: Combined Filter (Category and Name)**

* **URL:** `http://localhost:3000/products?category=690752e436b9886a8a4e2f60&name=h`
* **Response (Products in 'laptop' category with 'h' in name):**
    ```json
    [
      {
        "name": "hp",
        "description": "Updated description",
        "price": 1000,
        "discount": 15,
        "finalPrice": 850,
        "productCode": "93969d19-0hp1",
        "category": "laptop",
        "status": "Stock Out"
      }
    ]
    ```



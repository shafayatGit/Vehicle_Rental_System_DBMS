# 🚗 Vehicle Rental System – Database Design & SQL Queries

## 📌 Project Overview
This project demonstrates a **Vehicle Rental System database** designed with real-world rental business logic.  
It focuses on:

- Proper **ERD design**
- Correct use of **Primary Keys (PK)** and **Foreign Keys (FK)**
- Writing SQL queries using  
  `JOIN`, `EXISTS`, `WHERE`, `GROUP BY`, `HAVING`

The system manages **Users**, **Vehicles**, and **Bookings** efficiently.

---

## 🧩 Entity Relationship Diagram (ERD)

### Relationships
- **Users → Bookings** : One-to-Many  
- **Vehicles → Bookings** : One-to-Many  
- Each booking is linked to **exactly one user and one vehicle**

🔗 **ERD Diagram**  
[View ERD on DrawSQL](https://drawsql.app/teams/shafayats-company/diagrams/vehicle-rental-system)

---

## 🗄️ Database Tables

### 👤 Users
Stores customer and admin information.

**Fields**
- `user_id` (PK)
- `name`
- `email` (unique)
- `phone`
- `password`
- `role` (admin / customer)

---

### 🚙 Vehicles
Stores rental vehicle information.

**Fields**
- `vehicle_id` (PK)
- `name`
- `type` (car, bike, etc.)
- `model`
- `registration_number` (unique)
- `price_per_day`
- `status` (available / rented / maintenance)

---

### 📅 Bookings
Stores booking records and links users with vehicles.

**Fields**
- `booking_id` (PK)
- `user_id` (FK → Users)
- `vehicle_id` (FK → Vehicles)
- `start_date`
- `end_date`
- `status`
- `total_cost`

Each booking belongs to **one user** and **one vehicle**.

---

## 📄 SQL Queries (`queries.sql`)

### Query 1: JOIN
**Purpose:** Retrieve booking details with customer and vehicle information.

```sql
SELECT 
    b.booking_id,
    u.name AS customer_name,
    v.name AS vehicle_name,
    b.start_date,
    b.end_date,
    b.status
FROM bookings b
INNER JOIN users u ON b.user_id = u.user_id
INNER JOIN vehicles v ON b.vehicle_id = v.vehicle_id;

### Query 2: EXISTS
**Purpose:** Find vehicles that have never been booked.
FROM vehicles v
WHERE NOT EXISTS (
    SELECT 1
    FROM bookings b
    WHERE b.vehicle_id = v.vehicle_id
);

### Query 3: WHERE
**Purpose:** Retrieve available vehicles of a specific type (e.g., car).
SELECT *
FROM vehicles
WHERE status = 'available'
  AND type = 'car';

### Query 4: GROUP BY & HAVING
**Purpose:** Find vehicles booked more than 2 times.
SELECT 
    v.name AS vehicle_name,
    COUNT(b.booking_id) AS total_bookings
FROM vehicles v
INNER JOIN bookings b ON v.vehicle_id = b.vehicle_id
GROUP BY v.vehicle_id, v.name
HAVING COUNT(b.booking_id) > 2;

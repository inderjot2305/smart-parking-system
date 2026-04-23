# 🅿 SmartPark — Intelligent Parking System
### Java (Swing) + MySQL

A full-stack desktop application for managing parking slots, bookings, and revenue — built in pure Java with a MySQL backend.

---

## 📁 Project Structure

```
SmartParkingSystem/
└── src/
    └── smartparking/
        ├── Main.java                    ← Entry point
        ├── model/
        │   ├── ParkingSlot.java         ← Slot entity
        │   ├── Booking.java             ← Booking entity
        │   └── User.java                ← User entity
        ├── dao/
        │   ├── SlotDAO.java             ← DB ops for parking_slots
        │   └── BookingDAO.java          ← DB ops for bookings
        ├── service/
        │   └── BookingService.java      ← Business logic + transactions
        ├── util/
        │   └── DBConnection.java        ← JDBC singleton connection
        └── ui/
            ├── MainFrame.java           ← Main Swing window
            ├── DarkTabbedPaneUI.java    ← Custom tab styling
            └── WrapLayout.java          ← Wrapping FlowLayout
```

---

## ⚙️ Prerequisites

| Requirement | Version |
|---|---|
| JDK | 11 or higher |
| MySQL | 8.x |
| mysql-connector-j | 8.x (JAR) |

---

## 🗄️ Database Setup

1. Start MySQL and run:
```sql
CREATE DATABASE smartpark_db;
USE smartpark_db;
```

2. Copy the DDL from the **SQL Viewer → Schema DDL** tab in the app, or run:
```sql
-- See: src/smartparking/ui/MainFrame.java → SQL_DDL string
```

3. Insert sample data from **SQL Viewer → INSERT Data** tab.

---

## 🔧 Configure Database Password

Edit `src/smartparking/util/DBConnection.java`:
```java
private static final String PASSWORD = "your_mysql_password";
```

---

## 🏗️ Compile

```bash
# Windows
javac -cp ".;mysql-connector-j-8.x.x.jar" -d out -sourcepath src src/smartparking/Main.java

# Linux / Mac
javac -cp ".:mysql-connector-j-8.x.x.jar" -d out -sourcepath src src/smartparking/Main.java
```

---

## ▶️ Run

```bash
# Windows
java -cp ".;out;mysql-connector-j-8.x.x.jar" smartparking.Main

# Linux / Mac
java -cp ".:out:mysql-connector-j-8.x.x.jar" smartparking.Main
```

---

## 🖥️ Application Tabs

| Tab | Description |
|---|---|
| 📊 Dashboard | Live slot grid with colour-coded status, summary cards |
| 📋 Bookings | Full booking history table, cancel/checkout actions |
| 🔖 Book Slot | Form to create new bookings |
| 🗄️ SQL Viewer | Reference MySQL queries (DDL, SELECT, stored procedures, triggers) |

---

## 🧩 Architecture

```
UI Layer (Swing)
     ↓
Service Layer (BookingService) — transactions, business rules
     ↓
DAO Layer (SlotDAO, BookingDAO) — SQL via PreparedStatement
     ↓
DBConnection (JDBC Singleton)
     ↓
MySQL 8.x (smartpark_db)
```

---

## 🔑 Key Java Concepts Used

- **JDBC** with `PreparedStatement` (SQL injection safe)
- **Transactions** — `setAutoCommit(false)` + `commit()` / `rollback()`
- **Row-level locking** — `SELECT ... FOR UPDATE` prevents double booking
- **DAO Pattern** — separates data access from business logic
- **Swing GUI** — `JFrame`, `JTable`, `JTabbedPane`, custom UI delegates
- **MVC-like structure** — Model / DAO / Service / UI

---

## 📊 MySQL Features Used

- DDL with `FOREIGN KEY`, `ON DELETE CASCADE`
- `ENUM` types for status/role
- Stored Procedure `sp_book_slot` with transaction
- Triggers `trg_booking_cancel`, `trg_auto_payment`
- Aggregate queries: `SUM`, `COUNT`, `GROUP BY`, `CASE WHEN`
- `TIMESTAMPDIFF` for duration calculation

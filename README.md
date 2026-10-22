# Smart Library Management System (SLMS)

A robust, multi-tier enterprise desktop application developed in Java Swing and MySQL, featuring Role-Based Access Control (RBAC), Data Access Object (DAO) architecture, and full transactional book borrowing lifecycle management.

[![Java](https://img.shields.io/badge/Java-8%2B-ED8B00?style=flat&logo=openjdk&logoColor=white)](https://www.java.com/)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![Architecture](https://img.shields.io/badge/Architecture-DAO_Pattern-blue?style=flat)](#-system-architecture)
[![GUI](https://img.shields.io/badge/GUI-Java_Swing-orange?style=flat)](#-role-based-features)

---

## 📌 Overview

The **Smart Library Management System** digitalizes institutional library operations by replacing manual paper workflows with an audited, database-backed management system. The platform strictly enforces permission boundaries across three distinct user roles—**Administrator**, **Librarian**, and **Student**—and coordinates book inventory, hold requests, fine calculations, and member notifications.

---

## 🏗️ System Architecture

The application is structured following enterprise software design principles, enforcing a clear separation of concerns across three primary tiers:

```
┌────────────────────────────────────────────────────────┐
│                   Presentation Layer                   │
│  (Java Swing: LoginFrame, Admin/Librarian/Student UI)  │
└───────────────────────────┬────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────┐
│                Data Access Layer (DAO)                 │
│  (BookDAO, IssuedBookDAO, FineDAO, UserDAO, StudentDAO) │
└───────────────────────────┬────────────────────────────┘
                            │
┌───────────────────────────▼────────────────────────────┐
│                   Persistence Layer                    │
│        (MySQL Database via JDBC & DBUtil pool)         │
└────────────────────────────────────────────────────────┘
```

### 1. Data Access Object (DAO) Pattern
All database queries are encapsulated inside dedicated DAO classes (`BookDAO`, `IssuedBookDAO`, `FineDAO`, `LibrarianDAO`, `NotificationDAO`, `StudentDAO`, `UserDAO`). This prevents SQL query leakage into UI listeners and ensures that database transactions remain clean, maintainable, and testable.

### 2. Domain Model Encapsulation
Domain entities (`Book`, `User`, `Student`, `Librarian`, `Fine`, `IssuedBook`, `BookRequest`, `Notification`) use strict OOP encapsulation with private state variables and strongly-typed getters and setters.

---

## ✨ Role-Based Features

### 👑 Administrator Portal
- **Librarian Management:** Create, update, and manage librarian accounts and system permissions.
- **User Directory:** Audit all active student and staff accounts.
- **Financial & Fine Reports:** Review collected overdue fees and outstanding library penalties.
- **System Metrics:** Real-time summary of total inventory, active circulations, and system activity.

### 📚 Librarian Portal
- **Catalog Management:** Add new acquisitions, update metadata, edit stock levels, and remove archived books.
- **Issue & Return Processing:** Transactionally check out books to students and process returns.
- **Overdue Detection:** Visual table indicators (`OverdueCellRenderer`) highlighting loans past the due date.
- **Student Record Audits:** Review student borrowing history and track outstanding balances.

### 🎓 Student Portal
- **Catalog Search:** Browse available books and verify immediate availability.
- **Active Loans & Status:** Inspect current borrow dates, due dates, and return status.
- **Hold & Reservation:** Submit hold requests for currently checked-out volumes.
- **Reissue Requests:** Request loan extensions directly from the dashboard.
- **Notifications & Fines:** View system alerts and track any incurred fines.

---

## 🛠️ Database Schema (`library_db.sql`)

The underlying MySQL schema enforces relational integrity across core tables:
- `users`: Core credentials and role categorization (`ADMIN`, `LIBRARIAN`, `STUDENT`).
- `students`: Academic profiles linked to base user credentials.
- `librarians`: Staff operational profiles.
- `books`: Title, author, category, ISBN, and quantity tracking.
- `issued_books`: Active and historical borrowing records with checkout and return timestamps.
- `fines`: Penalty records tied to specific overdue issues.
- `book_requests`: Student hold and acquisition requests (`NEW`, `HOLD`, `REISSUE`).
- `notifications`: Student-specific operational alerts.

---

## 🚀 Installation & Setup

### Prerequisites

- **Java Development Kit (JDK):** Version 8 or higher
- **MySQL Server:** Version 8.0 or higher
- **MySQL Connector/J:** Included in `LibraryManagementSystem/src/lib/mysql-connector-j-9.3.0.jar`

### 1. Database Initialization

1. Start your local MySQL server.
2. Import the provided schema:
   ```bash
   mysql -u root -p < LibraryManagementSystem/library_db.sql
   ```

### 2. Configure Database Connection

Open `LibraryManagementSystem/src/util/DBUtil.java` and ensure your database credentials match:

```java
String url = "jdbc:mysql://localhost:3306/library_db?useSSL=false&serverTimezone=UTC";
String user = "root";       // Your MySQL username
String password = "your_password"; // Your MySQL password
```

### 3. Compilation & Execution

Navigate to the source directory and run:

```bash
cd LibraryManagementSystem/src

# Compile all source files against the JDBC driver
javac -cp ".;lib/mysql-connector-j-9.3.0.jar" -d ../bin $(Get-ChildItem -Recurse -Filter *.java | Select-Object -ExpandProperty FullName)

# Launch the Application Login Frame
java -cp ".;lib/mysql-connector-j-9.3.0.jar" ui.LoginFrame
```

*(On Linux / macOS bash, use `:` instead of `;` as the classpath delimiter).*

---

## 💡 Software Engineering Principles Demonstrated

- **Encapsulation:** Protects internal entity state while ensuring controlled modification through validation methods.
- **Inheritance:** `Student` and `Librarian` extend the foundational `User` class, maximizing code reusability across identity and authentication logic.
- **Polymorphism:** Method overriding enables specialized login behaviors and permission checks depending on runtime role resolution.
- **Separation of Concerns:** Zero UI code inside DAOs and zero SQL queries inside Swing event listeners.

---

## 📄 License

This project is licensed under the MIT License.

# BikeIsNice - Dedicated Bicycle &amp; Cycling E-Commerce Platform

**BikeIsNice** is a specialized Java Enterprise bicycle shopping platform built using **Jakarta EE 11 (Servlet 6.1 / JSP 3.1)**, **MySQL 8.0**, and **Apache Tomcat 11**. Tailored exclusively for cycling enthusiasts and commuters, it features dedicated categories for **Mountain Bikes (MTB)**, **Road &amp; Racing Bikes**, **Electric Bikes (E-Bikes)**, **Gravel &amp; Hybrid Bikes**, and **Cycling Gear &amp; Accessories**. It combines a responsive customer storefront, a secure zero-trust server-side checkout engine, an administrative fleet dashboard, a native **Java Swing desktop application**, and standalone distributed networking modules.

---

## Architecture Overview

```
                                      +---------------------------------------------+
                                      |             Client Applications             |
                                      +---------------------------------------------+
                                      | 1. Web Browser (Bootstrap 5.3 + JSP Views)  |
                                      | 2. Desktop GUI (Java Swing PLAF)            |
                                      | 3. TCP Socket Client / RMI Client           |
                                      +---------------------------------------------+
                                                             |
                                                             v
+--------------------------------------------------------------------------------------------------------------------+
|                                                Apache Tomcat 11                                                    |
|                                                                                                                    |
|   +------------------------------------+               +-------------------------------------------------------+   |
|   |          Security Filters          |               |                    Web Controllers                    |   |
|   | - AuthenticationFilter (Session)   | ------------> | - LoginServlet         - RegisterServlet              |   |
|   | - AdminFilter (Role: 'ADMIN')      |               | - ProductServlet       - CartServlet                   |   |
|   +------------------------------------+               | - CheckoutServlet                                     |   |
|                                                        +-------------------------------------------------------+   |
|                                                                                    |                               |
|                                                                                    v                               |
|                                                        +-------------------------------------------------------+   |
|                                                        |                     Service Layer                     |   |
|                                                        | - ProductService       - OrderService                 |   |
|                                                        | - CheckoutService (Zero-Trust Server Calculations)    |   |
|                                                        +-------------------------------------------------------+   |
|                                                                                    |                               |
|                                                                                    v                               |
|                                                        +-------------------------------------------------------+   |
|                                                        |                   DAO & Data Layer                    |   |
|                                                        | - UserDAO              - ProductDAO                   |   |
|                                                        | - CartDAO              - OrderDAO                     |   |
|                                                        | - CouponDAO            - PaymentDAO                   |   |
|                                                        | (Pure PreparedStatement, Connection Pooling)          |   |
|                                                        +-------------------------------------------------------+   |
+--------------------------------------------------------------------------------------------------------------------+
                                                             |
                                                             v
                                      +---------------------------------------------+
                                      |               MySQL 8.0 RDBMS               |
                                      |          Database: `shopsphere`             |
                                      |       (12 Tables, Foreign Keys,             |
                                      |     Indexes, Seed Products & Users)         |
                                      +---------------------------------------------+
```

---

## Technology Stack

- **Backend Runtime:** Java 25 (LTS)
- **Web Specification:** Jakarta EE 11 (Jakarta Servlet 6.1, JSP 3.1, JSTL 3.0)
- **Application Server:** Apache Tomcat 11.0.x
- **Database:** MySQL 8.0+ / 8.4 LTS
- **Build Tool:** Apache Maven 3.9+
- **Front-End:** HTML5, CSS3, Bootstrap 5.3.3, Bootstrap Icons 1.11, JSP Expression Language (EL), Custom JSP Tags
- **Desktop Application:** Java Swing with OS-Native Pluggable Look and Feel (PLAF)
- **Distributed Networking:** Raw TCP Sockets (`java.net.Socket`, `ServerSocket`) & Java RMI (`java.rmi.Remote`)
- **Cryptography:** PBKDF2 with HMAC-SHA256 (65,536 iterations, 16-byte salt, constant-time verification)

---

## Prerequisites

Before starting, ensure the following are installed on your machine:

1. **Java Development Kit (JDK 25 LTS):**
   ```bash
   java -version
   javac -version
   ```
2. **MySQL Server (8.0 or 8.4 LTS):**
   ```bash
   mysql --version
   ```
3. **Apache Maven (3.9+):**
   ```bash
   mvn -version
   ```
4. **Apache Tomcat 11 (11.0.x):**
   Download from [tomcat.apache.org](https://tomcat.apache.org/download-11.cgi).

---

## 1. Database Setup

The [`shopsphere.sql`](shopsphere.sql) script creates the `shopsphere` database schema, all 12 tables, indexes, constraints, and populates initial demo data (categories, products, coupons, and test user accounts).

### Step 1: Import Schema & Seed Data
Run the following command in your terminal:
```bash
mysql -u root -ppassword < /home/sahil-sepat/Desktop/ShopSphare/shopsphere.sql
```
*(Replace `password` with your actual MySQL root password if different).*

### Step 2: Database Configuration
The database connection parameters are defined in [`src/main/java/com/shopsphere/util/DBConnection.java`](src/main/java/com/shopsphere/util/DBConnection.java):
- **JDBC URL:** `jdbc:mysql://localhost:3306/shopsphere?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true`
- **Default User:** `root`
- **Default Password:** `password`

You can override these at runtime by setting environment variables:
```bash
export DB_URL="jdbc:mysql://localhost:3306/shopsphere?useSSL=false&serverTimezone=UTC&allowPublicKeyRetrieval=true"
export DB_USER="root"
export DB_PASSWORD="password"
```

---

## 2. Pre-Configured Test Credentials & Coupons

| Role | Email | Password | Access Level |
|---|---|---|---|
| **Administrator** | `admin@shopsphere.com` | `Password123!` (or `password`) | Storefront, Cart, Checkout, `/admin/*`, Swing Desktop GUI |
| **Customer** | `customer@shopsphere.com` | `Password123!` (or `password`) | Storefront, Cart, Checkout, Profile, Orders |

### Verified Promotional Bike Coupons
- **`BIKE10`**: **10% OFF** any order exceeding **$100.00** (max discount $300.00). Perfect for complete bike builds!
- **`RIDE50`**: Flat **$50.00 OFF** any bike purchase exceeding **$500.00**.
- **`SAVE10`**: 10% discount on orders over **$50.00** (max discount $25.00).
- **`WELCOME20`**: Flat **$20.00 OFF** on order subtotals over **$100.00**.

### Specialized Bike Catalog Categories
1. **Mountain Bikes (MTB)**: Full-suspension enduro 29ers, lightweight hardtails.
2. **Road & Racing Bikes**: Aero carbon road bikes, endurance disc frames.
3. **Electric Bikes (E-Bikes)**: 750W high-torque commuter e-bikes with long-range 48V batteries.
4. **Gravel & Hybrid Bikes**: All-road adventure gravel bikes with wide tire clearance.
5. **Cycling Gear & Accessories**: MIPS certified safety helmets, high-lumen bike lights, heavy-duty U-locks, and flat pedals.

---

## 3. Web Application Deployment (Apache Tomcat 11)

### Step 1: Fix Permissions in Tomcat
If Tomcat was recently extracted, make sure all `.sh` scripts in `bin/` are executable:
```bash
cd ~/Desktop/apache-tomcat-11.0.26/bin
chmod +x *.sh
```

### Step 2: Package the Application
Build the Web Application Archive (`shopsphere.war`):
```bash
cd /home/sahil-sepat/Desktop/ShopSphare
mvn clean package
```
*Note: A pre-compiled and updated `shopsphere.war` is also located in the `target/` directory.*

### Step 3: Deploy to Tomcat 11
Copy the WAR file into your Tomcat `webapps/` folder:
```bash
cp /home/sahil-sepat/Desktop/ShopSphare/target/shopsphere.war ~/Desktop/apache-tomcat-11.0.26/webapps/
```

### Step 4: Start Tomcat
```bash
cd ~/Desktop/apache-tomcat-11.0.26/bin
./startup.sh
```
*(To stop Tomcat when finished: `./shutdown.sh`)*

### Step 5: Access the Storefront
Open your browser and navigate to:
- **Product Catalog (Storefront):** [http://localhost:8080/shopsphere/products](http://localhost:8080/shopsphere/products)
- **Home Auto-Redirect:** [http://localhost:8080/shopsphere/](http://localhost:8080/shopsphere/)
- **Shopping Cart:** [http://localhost:8080/shopsphere/cart](http://localhost:8080/shopsphere/cart)
- **Secure Checkout:** [http://localhost:8080/shopsphere/checkout](http://localhost:8080/shopsphere/checkout)
- **User Login:** [http://localhost:8080/shopsphere/login](http://localhost:8080/shopsphere/login)
- **User Registration:** [http://localhost:8080/shopsphere/register](http://localhost:8080/shopsphere/register)
- **Admin Dashboard:** [http://localhost:8080/shopsphere/admin/dashboard.jsp](http://localhost:8080/shopsphere/admin/dashboard.jsp) *(Protected by AdminFilter)*
- **Admin Orders:** [http://localhost:8080/shopsphere/admin/orders.jsp](http://localhost:8080/shopsphere/admin/orders.jsp) *(Protected by AdminFilter)*

---

## 4. Java Swing Desktop Administration GUI

ShopSphere includes a native desktop application for store administrators. It programmatically applies the native OS Pluggable Look and Feel (PLAF) and uses `JTabbedPane` with data-dense `JTable` components in `JScrollPane`.

### How to Launch the Desktop GUI
```bash
cd /home/sahil-sepat/Desktop/ShopSphare
java -cp target/classes com.shopsphere.desktop.LoginFrame
```

### Desktop Application Features:
1. **`LoginFrame`:** Administrator login screen. Log in with `admin@shopsphere.com` / `password`.
2. **`AdminDashboard`:**
   - **Products Management Tab:** View live catalog, Search SKU/Name, Restock (+10 units), Add Product, and Delete Product.
   - **Orders Management Tab:** Inspect order totals and customer emails, update order statuses (Processing, Delivered, Cancelled).
   - **Customers Management Tab:** Manage customer accounts, inspect registration dates, and toggle active/inactive status.

---

## 5. Standalone Networking Modules (Academic Demos)

Two standalone networking modules demonstrate distributed communication without requiring an active web browser.

### Option A: One-Command Automated Test Suite
Run the bundled test script to compile and run both networking modules:
```bash
cd /home/sahil-sepat/Desktop/ShopSphare
./test-networking.sh
```

---

### Option B: Low-Level TCP Socket Client-Server Module
Package: `com.shopsphere.network.socket`

This module uses `java.net.ServerSocket` and `java.net.Socket` to communicate across raw byte streams with a custom command protocol.

1. **Start the Socket Server:**
   ```bash
   java -cp target/classes com.shopsphere.network.socket.SocketInventoryServer 8085
   ```
2. **Run the Socket Client Demo:**
   ```bash
   java -cp target/classes com.shopsphere.network.socket.SocketInventoryClient localhost 8085
   ```
3. **Interactive Testing via Netcat (`nc`):**
   ```bash
   nc localhost 8085
   ```
   Type any of these commands:
   ```text
   PING
   GET_STOCK SKU-LAPTOP-01
   UPDATE_STOCK SKU-LAPTOP-01 -2
   LIST_ALL
   QUIT
   ```

---

### Option C: Java RMI (Remote Method Invocation) Module
Package: `com.shopsphere.network.rmi`

This module demonstrates remote procedure calls over JRMP, remote interface stubs, in-process RMI registry management, and passing serialized [`Product`](src/main/java/com/shopsphere/model/Product.java) objects across the network.

1. **Start the RMI Server & Registry:**
   ```bash
   java -cp target/classes com.shopsphere.network.rmi.RmiInventoryServer
   ```
2. **Run the RMI Client:**
   ```bash
   java -cp target/classes com.shopsphere.network.rmi.RmiInventoryClient localhost 1099
   ```

---

## 6. Project Directory Structure

```
ShopSphare/
├── pom.xml                                      # Maven WAR build configuration (Jakarta EE 11)
├── shopsphere.sql                               # MySQL 8 schema, 12 tables, constraints & seed data
├── test-networking.sh                           # Automated test runner for Sockets and RMI
├── README.md                                    # Comprehensive setup and usage documentation
├── src/
│   ├── main/
│   │   ├── java/com/shopsphere/
│   │   │   ├── controller/                      # Web Servlets (jakarta.servlet.http.*)
│   │   │   │   ├── CartServlet.java             # Shopping cart item management (/cart)
│   │   │   │   ├── CheckoutServlet.java         # Server-verified order checkout (/checkout)
│   │   │   │   ├── LoginServlet.java            # User authentication (/login)
│   │   │   │   ├── ProductServlet.java          # Catalog browsing (/products)
│   │   │   │   └── RegisterServlet.java         # Account registration (/register)
│   │   │   ├── dao/                             # JDBC DAO Layer (PreparedStatements)
│   │   │   │   ├── CartDAO.java
│   │   │   │   ├── CouponDAO.java
│   │   │   │   ├── OrderDAO.java                # Transactional order & line items creator
│   │   │   │   ├── PaymentDAO.java
│   │   │   │   ├── ProductDAO.java
│   │   │   │   └── UserDAO.java
│   │   │   ├── desktop/                         # Native Swing Desktop Application
│   │   │   │   ├── AdminDashboard.java          # JTabbedPane, JTable in JScrollPane, PLAF
│   │   │   │   └── LoginFrame.java              # PLAF-styled admin login window
│   │   │   ├── filter/                          # Security Servlet Filters
│   │   │   │   ├── AdminFilter.java             # Restricts /admin/* to role 'ADMIN'
│   │   │   │   └── AuthenticationFilter.java    # Intercepts protected routes for active sessions
│   │   │   ├── model/                           # Domain POJO Entities
│   │   │   │   ├── Address.java
│   │   │   │   ├── CartItem.java
│   │   │   │   ├── Category.java
│   │   │   │   ├── Coupon.java
│   │   │   │   ├── CouponUsage.java
│   │   │   │   ├── Order.java                   # Implements Serializable
│   │   │   │   ├── OrderItem.java
│   │   │   │   ├── Payment.java
│   │   │   │   ├── Product.java                 # Implements Serializable
│   │   │   │   ├── Review.java
│   │   │   │   ├── User.java
│   │   │   │   └── WishlistItem.java
│   │   │   ├── network/
│   │   │   │   ├── rmi/                         # Java Remote Method Invocation (RMI)
│   │   │   │   │   ├── InventoryService.java    # java.rmi.Remote interface
│   │   │   │   │   ├── InventoryServiceImpl.java# UnicastRemoteObject implementation
│   │   │   │   │   ├── RmiInventoryClient.java  # RMI Registry lookup client
│   │   │   │   │   └── RmiInventoryServer.java  # LocateRegistry & service binder
│   │   │   │   └── socket/                      # Low-Level TCP Byte-Stream Sockets
│   │   │   │       ├── SocketInventoryClient.java
│   │   │   │       └── SocketInventoryServer.java
│   │   │   ├── service/                         # Business Logic & Pricing Engine
│   │   │   │   ├── CheckoutRequest.java         # DTO with zero client pricing fields
│   │   │   │   ├── CheckoutResult.java          # DTO carrying server-verified breakdown
│   │   │   │   ├── CheckoutService.java         # Multi-DAO coordinator & server pricing engine
│   │   │   │   ├── OrderService.java            # Order tracking & cancellations
│   │   │   │   └── ProductService.java          # Catalog & inventory rules
│   │   │   └── util/
│   │   │       ├── DBConnection.java            # DriverManager connection provider
│   │   │       └── PasswordUtil.java            # Salted PBKDF2 with HMAC-SHA256
│   │   └── webapp/
│   │       ├── index.jsp                        # Storefront welcome view (auto-redirects to /products)
│   │       ├── products.jsp                     # Dynamic catalog listing with sidebar filters
│   │       ├── cart.jsp                         # Responsive cart table & quantity editor
│   │       ├── checkout.jsp                     # Two-column checkout & order summary panel
│   │       ├── css/
│   │       │   └── styles.css                   # Custom theme & Bootstrap styling overrides
│   │       ├── fragments/                       # Reusable JSP Layout Fragments
│   │       │   ├── footer.jsp
│   │       │   ├── header.jsp
│   │       │   ├── navbar.jsp                   # Responsive navbar with search & cart counter
│   │       │   └── sidebar.jsp
│   │       ├── admin/                           # Store Manager Web Views
│   │       │   ├── dashboard.jsp                # High-level metrics & low-stock view
│   │       │   ├── orders.jsp                   # Customer orders with colored status badges
│   │       │   └── sidebar.jsp                  # Administrative navigation
│   │       └── WEB-INF/
│   │           └── tags/                        # Reusable JSP Tag Components
│   │               ├── productCard.tag          # Product card with discount & conditional button
│   │               └── rating.tag               # Star rating visualizer
└── target/
    ├── classes/                                 # Compiled Java bytecode
    └── shopsphere.war                           # Ready-to-deploy Web Application Archive
```

---

## 7. Troubleshooting FAQ

### 1. `java.sql.SQLSyntaxErrorException: Unknown database 'shopsphere'`
- **Cause:** The database was not created in MySQL before launching Tomcat.
- **Solution:** Run `mysql -u root -ppassword < shopsphere.sql`.

### 2. `bash: ./startup.sh: Permission denied`
- **Cause:** Shell scripts in Tomcat's `bin/` directory lack execute permissions.
- **Solution:** Run `chmod +x ~/Desktop/apache-tomcat-11.x/bin/*.sh`.

### 3. `ClassNotFoundException: jakarta.servlet.http.HttpServlet`
- **Cause:** Running on Apache Tomcat 9 or earlier (which only supports `javax.servlet.*`).
- **Solution:** This project is built on Jakarta EE 11 / Servlet 6.1. Deploy on **Apache Tomcat 10.1+** or **Tomcat 11.0+**.

### 4. `HTTP Status 404 – Not Found [/shopsphere/cart]`
- **Cause:** Tomcat was running an older build before `CartServlet.java` was deployed.
- **Solution:** Re-copy the updated `target/shopsphere.war` to Tomcat's `webapps/` folder and restart Tomcat.

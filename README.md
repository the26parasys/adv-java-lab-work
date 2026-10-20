# Advanced Java Programming Laboratory (ADJ Lab)

A comprehensive collection of **Advanced Java Programming** laboratory practicals and projects, arranged chronologically from **Experiment 1 to the final Capstone Project**.

---

## 📋 Table of Contents

- [Index of Experiments](#-index-of-experiments)
- [Prerequisites & System Setup](#-prerequisites--system-setup)
- [Experiment 1: GUI Form Development using Java Swing](#-experiment-1-gui-form-development-using-java-swing)
- [Experiment 2: Model-View-Controller (MVC) Architecture](#-experiment-2-model-view-controller-mvc-architecture)
- [Experiment 3: TCP Socket Programming (Client-Server Communication)](#-experiment-3-tcp-socket-programming-client-server-communication)
- [Experiment 4: Java Database Connectivity (JDBC) CRUD Operations](#-experiment-4-java-database-connectivity-jdbc-crud-operations)
- [Experiment 5: Database-Driven GUI Application (Swing + JDBC Integration)](#-experiment-5-database-driven-gui-application-swing--jdbc-integration)
- [Experiment 6: Remote Method Invocation (Java RMI Distributed Computing)](#-experiment-6-remote-method-invocation-java-rmi-distributed-computing)
- [Experiment 7: Web Server Environment Setup & Application Deployment (Apache Tomcat)](#-experiment-7-web-server-environment-setup--application-deployment-apache-tomcat)
- [Experiment 8: Java Web Development with Servlets, Filters, and Listeners](#-experiment-8-java-web-development-with-servlets-filters-and-listeners)
- [Experiment 9 / Capstone: Enterprise Cycling E-Commerce Platform (BikeIsNice)](#-experiment-9--capstone-enterprise-cycling-e-commerce-platform-bikeisnice)

---

## 📑 Index of Experiments

| Exp No. | Experiment Title | Core Technologies | Directory |
|:---:|---|---|---|
| **01** | **Student Registration GUI Form** | Java Swing, AWT, Event Handling | [`exp1/`](exp1/) |
| **02** | **MVC Pattern Calculator Application** | Java Swing, MVC Architecture | [`calc/`](calc/) |
| **03** | **TCP Socket Client-Server Echo System** | Java Networking (`java.net`), Sockets, I/O Streams | [`server-client/`](server-client/) |
| **04** | **Database Connectivity & SQL Operations (JDBC)** | JDBC 4.2, MySQL, PreparedStatements | [`jdbc/exp3/`](jdbc/exp3/) |
| **05** | **Integrated GUI with Database Storage** | Swing GUI, JDBC, MySQL, Input Validation | [`strjdbc/`](strjdbc/) |
| **06** | **Distributed Computing via Java RMI** | Java RMI, Remote Interfaces, RMI Registry | [`Remote-Method-Invocation/`](Remote-Method-Invocation/) |
| **07** | **Servlet Container & Web Server Deployment** | Apache Tomcat 9, WAR Deployment, Web XML | [`apache-tomcat-9.0.121/`](apache-tomcat-9.0.121/) |
| **08** | **Java Servlets, Sessions, Filters & Listeners** | Jakarta EE Servlet API, Session Tracking, Filters | [`servent/`](servent/) |
| **09** | **Full-Stack Enterprise E-Commerce Platform** | Jakarta EE 11, Servlets 6.1, JSP 3.1, JSTL, MySQL DAO, Swing Desktop Admin, Sockets & RMI | [`BikeIsNice/`](BikeIsNice/) |

---

## 🛠️ Prerequisites & System Setup

Ensure the following tools and runtimes are installed on your workstation:

1. **Java Development Kit (JDK 25 LTS):**
   ```bash
   java -version
   javac -version
   ```
2. **MySQL Database Server (8.0+):**
   ```bash
   mysql --version
   ```
3. **Apache Maven (3.8+):**
   ```bash
   mvn -version
   ```
4. **Apache Tomcat Server (9.0.x or 10.1+/11.0.x):**
   Bundled inside [`apache-tomcat-9.0.121/`](apache-tomcat-9.0.121/) for Servlet development.

---

## 🧪 Experiment 1: GUI Form Development using Java Swing

### 🎯 Aim & Objective
To design and implement an interactive desktop Graphical User Interface (GUI) for a **Student Registration Form** using Java Swing components and event listeners.

### 📂 Directory & File Structure
```
exp1/
├── StudentRegistration.java    # Swing GUI class with ActionListener
└── StudentRegistration.class   # Compiled bytecode
```

### 💡 Key Concepts & Implementation Details
- **Components Used**: `JFrame`, `JLabel`, `JTextField`, `JRadioButton`, `ButtonGroup`, `JCheckBox`, `JButton`.
- **Layout Management**: Absolute positioning with `setLayout(null)` and `setBounds(x, y, width, height)`.
- **Event Handling**: Implements `ActionListener` to capture button clicks on **Submit** and **Reset**.
- **User Validation**: Validates required fields and terms acceptance, rendering user details via `JOptionPane.showMessageDialog()`.

### 🚀 Compilation & Execution
```bash
# Navigate to experiment directory
cd exp1

# Compile
javac StudentRegistration.java

# Run
java StudentRegistration
```

---

## 🧪 Experiment 2: Model-View-Controller (MVC) Architecture

### 🎯 Aim & Objective
To design and implement an interactive desktop Calculator adhering strictly to the **Model-View-Controller (MVC)** architectural design pattern, separating business logic, user interface, and event handling.

### 📂 Directory & File Structure
```
calc/
├── ModelsCalculator.java       # Model: Contains arithmetic calculation logic
├── ViewsCalculator.java        # View: Builds Swing GUI components & layouts
├── ControllerCalculator.java   # Controller: Binds View events to Model methods
├── Runmain.java                # Application Entry Point
└── README.md                   # Module documentation
```

### 💡 Key Concepts & Implementation Details
- **Model (`ModelsCalculator`)**: Encapsulates arithmetic operations (`add`, `sub`, `multiply`, `divide`) and edge-case handling (e.g. division by zero).
- **View (`ViewsCalculator`)**: Constructs the UI with `JTextField` for input/display and operator buttons (`+`, `-`, `*`, `/`).
- **Controller (`ControllerCalculator`)**: Attaches action listeners to the view buttons, parses inputs, triggers Model operations, and updates the view display.
- **Benefits**: Loose coupling, high maintainability, and clean code separation.

### 🚀 Compilation & Execution
```bash
# Navigate to experiment directory
cd calc

# Compile all source files
javac *.java

# Launch application
java Runmain
```

---

## 🧪 Experiment 3: TCP Socket Programming (Client-Server Communication)

### 🎯 Aim & Objective
To implement two-way network communication over a TCP connection using Java Sockets (`java.net` package) with a dedicated Server and Client echo protocol.

### 📂 Directory & File Structure
```
server-client/
├── server.java     # TCP Server listening on port 5000
└── client.java     # TCP Client connecting to localhost:5000
```

### 💡 Key Concepts & Implementation Details
- **TCP Socket Architecture**: Utilizes `ServerSocket` on port `5000` to listen for incoming connections and `Socket` to establish client-server streams.
- **I/O Streams**: Uses `BufferedReader` wrapping `InputStreamReader` for receiving messages and `PrintWriter` with auto-flush for sending text over the stream.
- **Communication Protocol**:
  - The server echoes every message back with an `"Echo: <message>"` prefix.
  - Graceful disconnection protocol when the client transmits `"bye"`.

### 🚀 Compilation & Execution
Open two terminal windows:

```bash
# Compile both programs
cd server-client
javac server.java client.java

# Terminal 1: Start Server
java server

# Terminal 2: Start Client
java client
```

---

## 🧪 Experiment 4: Java Database Connectivity (JDBC) CRUD Operations

### 🎯 Aim & Objective
To establish a connection between a Java program and a MySQL relational database using JDBC (Type-4 Driver), execute parameterized queries, and perform basic CRUD operations.

### 📂 Directory & File Structure
```
jdbc/exp3/
├── lib/
│   └── mysql-connector-j-26.7.0.jar   # MySQL JDBC Driver library
├── src/
│   └── Main.java                      # JDBC connection, insert & query execution
├── bin/                               # Compiled classes
└── README.md                          # Configuration notes
```

### 💡 Key Concepts & Implementation Details
- **Driver Registration**: Loading the MySQL driver class `Class.forName("com.mysql.cj.jdbc.Driver")`.
- **Connection Management**: Connecting via `DriverManager.getConnection(url, user, password)`.
- **Parameterized SQL with `PreparedStatement`**: Safe query execution for inserting student records into the database.
- **Data Retrieval with `Statement` & `ResultSet`**: Executing `SELECT` queries and reading result records via cursor traversal (`rs.next()`).

### 🗄️ Database Setup
```sql
CREATE DATABASE IF NOT EXISTS collage_db;
USE collage_db;

CREATE TABLE IF NOT EXISTS Student (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    branch VARCHAR(50)
);
```

### 🚀 Compilation & Execution
```bash
cd jdbc/exp3

# Compile with JDBC Driver in classpath
javac -cp "lib/*:src" src/Main.java -d bin

# Run program
java -cp "lib/*:bin" Main
```

---

## 🧪 Experiment 5: Database-Driven GUI Application (Swing + JDBC Integration)

### 🎯 Aim & Objective
To develop a database-integrated desktop application by connecting a Java Swing GUI form to a MySQL database, persisting user registration data through JDBC.

### 📂 Directory & File Structure
```
strjdbc/
├── lib/
│   └── mysql-connector-j-26.7.0.jar   # MySQL JDBC Driver
├── src/
│   └── StudentRegistration.java       # Full Swing GUI with JDBC persistence
├── bin/                               # Compiled classes
└── README.md
```

### 💡 Key Concepts & Implementation Details
- **Integrated Architecture**: Merges interactive Swing UI (`JFrame`, `JTextField`, `JRadioButton`, `JCheckBox`) with a JDBC data persistence layer.
- **Robust Input Validation**: Verifies that required fields are non-empty and terms are accepted before executing database calls.
- **Database Persistence**: Employs `PreparedStatement` to insert sanitized student records (`name`, `roll`, `gender`, `branch`) into table `streg`.
- **Thread Safety**: Dispatches UI rendering via `SwingUtilities.invokeLater()`.

### 🗄️ Database Setup
```sql
CREATE DATABASE IF NOT EXISTS collage_db;
USE collage_db;

CREATE TABLE IF NOT EXISTS streg (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    roll VARCHAR(50) NOT NULL,
    gender VARCHAR(10),
    branch VARCHAR(50)
);
```

### 🚀 Compilation & Execution
```bash
cd strjdbc

# Compile
javac -cp "lib/*:src" src/StudentRegistration.java -d bin

# Run
java -cp "lib/*:bin" StudentRegistration
```

---

## 🧪 Experiment 6: Remote Method Invocation (Java RMI Distributed Computing)

### 🎯 Aim & Objective
To build a distributed client-server application using Java Remote Method Invocation (RMI), allowing a remote client to execute object methods hosted on a remote server.

### 📂 Directory & File Structure
```
Remote-Method-Invocation/
├── Calculator.java         # Remote Interface (extends java.rmi.Remote)
├── CalculatorImpl.java     # Remote Implementation (extends UnicastRemoteObject)
├── Server.java             # RMI Server (binds service to RMI Registry)
└── client.java             # RMI Client (performs lookup and calls remote methods)
```

### 💡 Key Concepts & Implementation Details
- **Remote Interface (`Calculator`)**: Declares remote methods (`add`, `subtract`) throwing `RemoteException`.
- **Remote Implementation (`CalculatorImpl`)**: Extends `UnicastRemoteObject` to export the object for remote communication.
- **RMI Registry & Binding**: `Server` creates the implementation and registers it via `Naming.rebind("rmi://localhost/CalculatorService", calc)`.
- **Client Lookup & Invocation**: `client` resolves the remote stub via `Naming.lookup(...)` and transparently invokes arithmetic operations across JVMs.

### 🚀 Compilation & Execution
```bash
cd Remote-Method-Invocation

# Compile all RMI files
javac *.java

# Start RMI Registry (if not embedded)
rmiregistry &

# Terminal 1: Start RMI Server
java Server

# Terminal 2: Run RMI Client
java client
```

---

## 🧪 Experiment 7: Web Server Environment Setup & Application Deployment (Apache Tomcat)

### 🎯 Aim & Objective
To configure and manage an enterprise Java Servlet Container and Web Server (**Apache Tomcat 9**), understand the container directory structure, configure deployment descriptors, and deploy web applications.

### 📂 Directory & File Structure
```
apache-tomcat-9.0.121/
├── bin/                    # Server lifecycle scripts (startup.sh, shutdown.sh, catalina.sh)
├── conf/                   # Configuration files (server.xml, web.xml, context.xml)
├── lib/                    # Servlet API & container libraries (servlet-api.jar, jsp-api.jar)
├── logs/                   # Catalina and access log files
├── webapps/                # Deployment folder for web applications
│   ├── myWebapp/           # Static and dynamic test webapp
│   └── servlet-1.0-SNAPSHOT.war # Deployed Web Archive
└── work/                   # Compiled JSP servlets and temporary runtime files
```

### 💡 Key Concepts & Implementation Details
- **Servlet Container Lifecycle**: Starting, stopping, and inspecting server logs.
- **Port & Connector Configuration**: Configuring HTTP connectors in `conf/server.xml` (default port `8080`).
- **WAR Deployment**: Automated unpacking and deployment of Web Application Archives in `webapps/`.

### 🚀 Running the Tomcat Server
```bash
cd apache-tomcat-9.0.121/bin

# Ensure scripts have execute permissions
chmod +x *.sh

# Start Tomcat Server
./startup.sh

# Open browser: http://localhost:8080/

# Stop Tomcat Server
./shutdown.sh
```

---

## 🧪 Experiment 8: Java Web Development with Servlets, Filters, and Listeners

### 🎯 Aim & Objective
To develop a dynamic Java Web Application utilizing the Servlet API (Jakarta EE), demonstrating form processing, session tracking (`HttpSession`), request filtering (`@WebFilter`), and application lifecycle events (`@WebListener`).

### 📂 Directory & File Structure
```
servent/
├── pom.xml                 # Maven build file configured for Jakarta Servlet API
└── src/
    └── main/
        ├── java/com/servelent/
        │   ├── HelloServlet.java   # Form processing & RequestDispatcher forward
        │   ├── LoginServlet.java   # Authentication & session creation
        │   ├── HomeServlet.java    # Protected user home page
        │   ├── AuthFilter.java     # Security filter restricting /home
        │   ├── AppListener.java    # ServletContextListener lifecycle logger
        │   └── Main.java           # Runner / helper
        └── webapp/
            └── index.html          # Login and entry HTML form
```

### 💡 Key Concepts & Implementation Details
- **`HelloServlet` (`@WebServlet("/hello")`)**: Extracts form parameters (`request.getParameter()`), sets request attributes (`request.setAttribute()`), and dispatches to views.
- **`LoginServlet` (`@WebServlet("/login")`)**: Authenticates credentials, creates a user session via `request.getSession()`, and sets session attributes.
- **`HomeServlet` (`@WebServlet("/home")`)**: Displays personalized content for authenticated users with active sessions.
- **`AuthFilter` (`@WebFilter("/home")`)**: Intercepts requests to `/home`, checks for valid user sessions, and redirects unauthorized users back to `index.html`.
- **`AppListener` (`@WebListener`)**: Implements `ServletContextListener` to log messages upon application startup (`contextInitialized`) and shutdown (`contextDestroyed`).

### 🚀 Building and Deploying with Maven
```bash
cd servent

# Build WAR archive using Maven
mvn clean package

# Copy generated WAR into Tomcat webapps
cp target/servlet-1.0-SNAPSHOT.war ../apache-tomcat-9.0.121/webapps/

# Start Tomcat and browse:
# http://localhost:8080/servlet-1.0-SNAPSHOT/
```

---

## 🧪 Experiment 9 / Capstone: Enterprise Cycling E-Commerce Platform (BikeIsNice)

### 🎯 Aim & Objective
To architect and implement an enterprise-grade full-stack Java web application and distributed system (**BikeIsNice** / ShopSphere) synthesizing all Advanced Java concepts: Jakarta Servlets, JSP/JSTL views, custom tags, relational MySQL database with 12 tables, JDBC DAO design pattern, session security filters, native Java Swing desktop administration dashboard, and standalone distributed networking modules (Sockets & RMI).

### 📂 Directory & File Structure
```
BikeIsNice/
├── pom.xml                         # Maven project configuration (Jakarta EE 11)
├── shopsphere.sql                  # Complete MySQL schema (12 tables + seed data)
├── test-networking.sh              # Automated test runner for Sockets and RMI
├── README.md                       # Dedicated detailed documentation
├── src/main/
│   ├── java/com/shopsphere/
│   │   ├── controller/             # Jakarta Servlets (Cart, Checkout, Login, Product, Register)
│   │   ├── dao/                    # JDBC DAO Layer (PreparedStatements & connection pooling)
│   │   ├── desktop/                # Native Swing Administration GUI (AdminDashboard, LoginFrame)
│   │   ├── filter/                 # Security Filters (AdminFilter, AuthenticationFilter)
│   │   ├── model/                  # Domain POJOs & Entities (implements Serializable)
│   │   ├── network/
│   │   │   ├── rmi/                # Java RMI inventory service & client
│   │   │   └── socket/             # Low-level TCP Socket inventory server & client
│   │   ├── service/                # Business logic & zero-trust server checkout engine
│   │   └── util/                   # DBConnection & PBKDF2 Password Hashing
│   └── webapp/
│       ├── index.jsp               # Storefront portal & catalog router
│       ├── products.jsp            # Product catalog with category filters
│       ├── cart.jsp                # Interactive shopping cart
│       ├── checkout.jsp            # Secure checkout workflow
│       ├── admin/                  # Protected manager views (dashboard, orders)
│       └── WEB-INF/tags/           # Custom reusable JSP tags (productCard, rating)
└── target/
    └── shopsphere.war              # Packaged ready-to-deploy Web Application Archive
```

### 💡 Enterprise Features & Advanced Java Highlights
1. **Full-Stack Web Architecture**: Jakarta Servlet 6.1, JSP 3.1, JSTL 3.0, and Bootstrap 5.3.
2. **Zero-Trust Pricing & Checkout Engine**: Computes subtotals, taxes, and promotional coupons on the server to prevent client-side price tampering.
3. **Database & Persistence**: 12 relational MySQL tables (`users`, `products`, `orders`, `order_items`, `cart_items`, `categories`, `coupons`, `payments`, etc.) utilizing pure `PreparedStatement` queries.
4. **Desktop Administrator GUI**: Java Swing desktop app with native OS Pluggable Look and Feel (PLAF), `JTabbedPane`, and `JTable` inside `JScrollPane` for real-time inventory and order management.
5. **Distributed Networking Modules**:
   - **TCP Sockets**: `SocketInventoryServer` & `SocketInventoryClient` implementing a custom inventory protocol.
   - **Java RMI**: `RmiInventoryServer` & `RmiInventoryClient` with remote procedure calls for inventory status updates.

### 🚀 Running the Capstone Platform
```bash
cd BikeIsNice

# Step 1: Import Database Schema & Seed Data
mysql -u root -ppassword < shopsphere.sql

# Step 2: Build the WAR package
mvn clean package

# Step 3: Run the Swing Desktop Admin Application
java -cp target/classes com.shopsphere.desktop.LoginFrame

# Step 4: Run the Standalone Networking Test Suite (Sockets + RMI)
./test-networking.sh
```

---

## 👨‍💻 Repository Author

- **Author**: Sahil Sepat
- **Course**: Advanced Java Laboratory (ADJ Lab)
- **Repository**: [`adjrepo`](https://github.com/sahil1205-jat/adj)

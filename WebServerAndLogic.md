## 5. Web Server and Backend Logic

### 5.1. Web Server

A web server (e.g., **Nginx**, **Apache**) receives HTTP requests and routes them to the correct application or static files.

### 5.2. Application Server

Hosts backend logic written in frameworks such as:

- **Node.js / Express**
- **Java / Spring Boot**
- **Python / Flask or Django**
- **C# / ASP.NET**

### 5.3. Database Layer

Stores and retrieves data used by the backend.  
Common systems: **MySQL, PostgreSQL, MongoDB, Redis.**

### 5.4. Backend Workflow

1. Request arrives at web server.
2. Server passes it to the application.
3. Application processes logic and queries the database.
4. Response is generated and sent back to client.

1. Software Requirement Specification (SRS)
1.1 Introduction
1.1.1 Purpose
The purpose of this document is to define the requirements for a banking application that provides account management, 
transaction processing, and service request handling for users.

1.1.2 Scope
The application will allow users to register, log in, manage their accounts, perform transactions, and submit service requests. Administrators will have additional privileges for managing users and reviewing service requests.

1.1.3 Definitions, Acronyms, and Abbreviations
SRS: Software Requirement Specification
FRS: Functional Requirement Specification
API: Application Programming Interface
UI: User Interface
JWT: JSON Web Token
1.1.4 References
React Documentation
Spring Boot Documentation
MySQL Documentation
1.2 Overall Description
1.2.1 Product Perspective
The banking application is a standalone system providing essential banking functionalities. 
It is a web-based application accessible via modern browsers.

1.2.2 Product Functions
User registration and authentication
Account management
Transaction processing (withdraw, deposit, transfer)
Service request submission
Email and OTP services for password recovery
1.2.3 User Characteristics
End-users: Individuals who will use the application to manage their accounts and perform transactions.
Administrators: Users with additional privileges for managing users and service requests.
1.2.4 Constraints
The application must support current versions of major browsers (Chrome, Firefox, Safari).
The application should handle up to 10,000 concurrent users.
1.2.5 Assumptions and Dependencies
Users have access to the internet.
Users have a valid email address for registration and OTP services.
The backend server and database are hosted in a reliable environment.
1.3 Specific Requirements
1.3.1 User Interfaces
Registration Page: Collects user details for creating an account.
Login Page: Authenticates users using their credentials.
Dashboard: Displays account information and recent transactions.
Transaction Page: Allows users to perform financial transactions.
Service Request Page: Enables users to submit and view service requests.
1.3.2 Functional Requirements
User Registration

The system shall allow users to register by providing personal information and credentials.
The system shall validate the uniqueness of the username and email.
User Authentication

The system shall authenticate users using JWT tokens.
The system shall prevent access to secured pages for unauthenticated users.
Account Management

The system shall allow users to view account details, including balance and transaction history.
Transaction Processing

The system shall support withdrawals, deposits, and transfers.
The system shall validate sufficient balance for withdrawals and transfers.
Service Requests

The system shall allow users to submit service requests and view their status.
The system shall allow administrators to manage service requests.
Email and OTP Services

The system shall send OTP emails for password recovery.
The system shall validate OTPs within a 10-minute window.
1.3.3 Performance Requirements
The system shall load the dashboard within 2 seconds.
The system shall process transactions within 3 seconds.
1.3.4 Design Constraints
The application shall be built using React for the frontend and Spring Boot for the backend.
The database shall be implemented using MySQL.
1.3.5 Security Requirements
The system shall encrypt passwords using a secure hashing algorithm.
The system shall use HTTPS for all communications between client and server.
The system shall implement role-based access control for sensitive operations.


Functional Requirement Specification (FRS)
2.1 Functional Requirements
2.1.1 User Registration and Authentication
User Registration

Functionality: Allow users to register with personal and credential information.
Inputs: First name, last name, email, username, password.
Outputs: Success or error message.
Processing:
Validate email and username uniqueness.
Hash the password and store user data in the database.
User Login

Functionality: Authenticate users and provide access to the dashboard.
Inputs: Username, password.
Outputs: JWT token and success or error message.
Processing:
Validate credentials.
Generate JWT token for successful authentication.
2.1.2 Account Management
View Account Details

Functionality: Display account balance and transaction history.
Inputs: User ID.
Outputs: Account details and transaction list.
Processing:
Fetch account data from the database.
Display account information on the dashboard.
Create Account

Functionality: Allow users to create a new account.
Inputs: Account name, initial deposit.
Outputs: Account creation success or error message.
Processing:
Validate account details.
Store new account data in the database.
2.1.3 Transaction Processing
Withdraw Funds

Functionality: Allow users to withdraw funds from their account.
Inputs: Account ID, amount.
Outputs: Transaction success or error message.
Processing:
Validate sufficient balance.
Update account balance and store transaction details.
Deposit Funds

Functionality: Allow users to deposit funds into their account.
Inputs: Account ID, amount.
Outputs: Transaction success or error message.
Processing:
Update account balance and store transaction details.
Transfer Funds

Functionality: Allow users to transfer funds between accounts.
Inputs: Payer ID, payee ID, amount.
Outputs: Transaction success or error message.
Processing:
Validate sufficient balance.
Update account balances for both payer and payee.
Store transaction details.
2.1.4 Service Requests
Submit Service Request

Functionality: Allow users to submit service requests.
Inputs: User ID, request details.
Outputs: Request submission success or error message.
Processing:
Store service request details in the database.
View Service Requests

Functionality: Display all service requests for the user.
Inputs: User ID.
Outputs: List of service requests.
Processing:
Fetch service requests from the database.
Display requests on the service request page.


3. Design Document
3.1 System Architecture
The banking application follows a client-server architecture with a React frontend and a Spring Boot backend. 
The system is designed as a multi-tier application with distinct presentation, business logic, and data access layers.

3.1.1 Presentation Layer
Technologies: React, Bootstrap, CSS
Components:
Auth Components: Handles user registration and login.
Dashboard Components: Displays account information and transactions.
Transaction Components: Manages transaction forms and lists.
Service Request Components: Manages service requests.
3.1.2 Business Logic Layer
Technologies: Spring Boot
Components:
Authentication Service: Handles user authentication and JWT generation.
Account Service: Manages account operations and data.
Transaction Service: Manages transaction processing and validation.
Service Request Service: Manages service request operations.
3.1.3 Data Access Layer
Technologies: Spring Data JPA, MySQL
Components:
Repositories: Provides CRUD operations for entities (User, Account, Transaction, ServiceRequest).
3.2 Database Design
3.2.1 ER Diagram
plaintext
Copy code
+-----------------+      +-------------------+
|     User        |      |     Account       |
+-----------------+      +-------------------+
| userId (PK)     |      | accountId (PK)    |
| firstName       |      | userId (FK)       |
| lastName        |      | balance           |
| email           |      | transactions      |
| username        |      | beneficiaries     |
| password        |      |                   |
+-----------------+      +-------------------+
         |                        |
         | 1                      | 1
         |                        |
         +------------------------+
                     |
                    *|                     
+------------------+ +------------------+ 
|   Transaction    | | ServiceRequest   |
+------------------+ +------------------+
| transactionId(PK)| | requestId (PK)   |
| payerId (FK)     | | userId (FK)      |
| payeeId (FK)     | | requestDetails   |
| amount           | | status           |
| transactionType  | +------------------+
| timestamp        | 
+------------------+ 
3.2.2 Tables
User Table

Columns: userId (PK), firstName, lastName, email, username, password
Account Table

Columns: accountId (PK), userId (FK), balance
Transaction Table

Columns: transactionId (PK), payerId (FK), payeeId (FK), amount, transactionType, timestamp
ServiceRequest Table

Columns: requestId (PK), userId (FK), requestDetails, status
3.3 API Design
3.3.1 Endpoints
User Authentication

POST /api/auth/register: Registers a new user
POST /api/auth/login: Authenticates a user
Account Management

GET /api/accounts/{id}: Retrieves account details
POST /api/accounts: Creates a new account
Transaction Management

GET /api/transactions: Retrieves all transactions for a user
POST /api/transactions: Creates a new transaction
Service Requests

GET /api/service-requests: Retrieves all service requests
POST /api/service-requests: Creates a new service request
DELETE /api/service-requests/{id}: Deletes a service request
3.4 Security Design
JWT Authentication: Secure endpoints using JWT tokens for user authentication.
Password Encryption: Use bcrypt for hashing passwords before storing them in the database.
Role-Based Access Control: Implement role-based access for distinguishing between user and administrator functionalities.
3.5 UI Design
The UI design focuses on usability and responsiveness, utilizing React components and Bootstrap for a modern and user-friendly 
interface. Here’s a brief description of key UI elements:

Navbar: Provides easy navigation across different sections of the application.
Forms: Utilized for user inputs like registration, login, transactions, and service requests.
Tables: Display data like transaction history and service requests in a structured format.
Cards: Used for displaying account summaries and important alerts.
 

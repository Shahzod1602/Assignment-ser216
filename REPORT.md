# Banking System - UML Design Report

## 1. System Overview

The **Banking System** is a real-world software system that manages financial operations for customers, bank employees, and administrators. It handles core banking functions including account management, money deposits and withdrawals, fund transfers between accounts, loan applications, and transaction tracking.

This system involves interactions between multiple types of users (actors) and various system functions, making it an ideal candidate for UML modeling. The system supports operations through web/mobile applications and ATM machines.

---

## 2. Identifying Actors and Use Cases

### 2.1 Actors (Roles)

The system has **four key actors**:

| Actor | Description |
|-------|-------------|
| **Customer** | The primary user who holds bank accounts, performs financial transactions, and applies for loans |
| **Bank Teller** | A bank employee who processes transactions on behalf of customers and verifies their identity |
| **Admin** | A system administrator who manages accounts, users, and generates system reports |
| **ATM Machine** | An external system actor that provides self-service banking operations (deposit, withdraw, balance check) |

### 2.2 Use Cases (Actions)

The system includes the following **key use cases**:

1. **Open Account** - Customer opens a new bank account
2. **Deposit Money** - Money is deposited into an account (by Customer, Teller, or ATM)
3. **Withdraw Money** - Money is withdrawn from an account
4. **Transfer Funds** - Customer transfers money between accounts
5. **View Account Balance** - Check current account balance
6. **View Transaction History** - View past transactions
7. **Apply for Loan** - Customer submits a loan application
8. **Process Transactions** - Teller processes transactions for customers
9. **Verify Customer Identity** - Teller verifies customer identity for sensitive operations
10. **Manage Accounts** - Admin creates, modifies, or closes accounts
11. **Generate Reports** - Admin generates financial and operational reports
12. **Manage Users** - Admin manages system users
13. **Authenticate User** - System authenticates user credentials (included in most operations)
14. **Send Notification** - System sends notifications after key events

### 2.3 Relationships in the Use Case Diagram

- **Include (`<<include>>`)**: Deposit, Withdraw, and Transfer all include Authentication, because the user must be authenticated before performing any financial operation. Transfer also includes Send Notification.
- **Extend (`<<extend>>`)**: View Transaction History extends View Account Balance, since a user may optionally view their full history when checking their balance.
- **Actor-to-Use-Case associations**: Lines connecting actors to the use cases they can perform.

---

## 3. Class Diagram Design

### 3.1 Key Entities (Classes)

The system is modeled with the following classes:

| Class | Purpose | Key Attributes | Key Methods |
|-------|---------|----------------|-------------|
| **User** (abstract) | Base class for all system users | userId, firstName, lastName, email, passwordHash | login(), logout(), updateProfile() |
| **Customer** | Bank customer | customerId, address, dateOfBirth, nationalId | openAccount(), applyForLoan(), viewAccounts() |
| **BankTeller** | Bank employee | employeeId, branch | processTransaction(), verifyIdentity() |
| **Admin** | System administrator | adminLevel | manageAccounts(), generateReport(), manageUsers() |
| **Account** (abstract) | Base class for bank accounts | accountId, accountNumber, balance, status | deposit(), withdraw(), getBalance(), getTransactionHistory() |
| **SavingsAccount** | Interest-bearing account | interestRate, minimumBalance | calculateInterest() |
| **CheckingAccount** | Everyday transaction account | overdraftLimit | processCheck() |
| **Transaction** | Record of a financial operation | transactionId, type, amount, date, status | execute(), cancel(), getReceipt() |
| **Transfer** | Fund transfer between accounts | transferId, fromAccount, toAccount, amount | execute(), validate() |
| **Loan** | Loan application and tracking | loanId, amount, interestRate, termMonths | approve(), reject(), calculateMonthlyPayment() |
| **Notification** | System notifications | notificationId, message, type, isRead | send(), markAsRead() |
| **Branch** | Physical bank branch | branchId, name, address | getEmployees(), getAccounts() |

### 3.2 Relationships Between Classes

| Relationship Type | Between | Description |
|-------------------|---------|-------------|
| **Inheritance** | User → Customer, BankTeller, Admin | All user types inherit common properties from User |
| **Inheritance** | Account → SavingsAccount, CheckingAccount | Account subtypes inherit from the abstract Account class |
| **Association (1 to many)** | Customer → Account | One customer owns one or more accounts |
| **Association (1 to many)** | Account → Transaction | One account records many transactions |
| **Association (1 to many)** | Customer → Loan | One customer can apply for multiple loans |
| **Association (1 to many)** | Customer → Notification | One customer receives multiple notifications |
| **Association** | Transfer → Account | A transfer references a source and destination account |
| **Association (many to 1)** | BankTeller → Branch | Many tellers work at one branch |
| **Association (1 to many)** | Branch → Account | One branch manages many accounts |

### 3.3 Role of the Class Diagram

The Class Diagram models the **static structure** of the system. It defines:
- What data the system stores (attributes)
- What operations the system can perform (methods)
- How entities relate to each other (relationships)

This diagram serves as the blueprint for database schema design and object-oriented code implementation.

---

## 4. Sequence Diagram - Fund Transfer Process

### 4.1 Process Overview

The Sequence Diagram models the **Fund Transfer** process, showing how system components interact over time when a customer transfers money from one account to another.

### 4.2 Participants

| Participant | Role |
|-------------|------|
| Customer | Initiates the transfer |
| Web/Mobile App | User interface |
| Auth Service | Handles authentication |
| Transfer Service | Orchestrates the transfer logic |
| Account Service | Manages account operations |
| Database | Persistent data storage |
| Notification Service | Sends confirmations |

### 4.3 Interaction Flow

The sequence follows these phases:

**Phase 1 - Authentication:**
1. Customer opens the Transfer page
2. System requests authentication credentials
3. Customer enters username and password
4. Auth Service validates credentials against the database
5. Authentication token is returned

**Phase 2 - Transfer Initiation:**
1. Customer enters transfer details (recipient account, amount, description)
2. App sends the transfer request to the Transfer Service

**Phase 3 - Validation:**
1. Transfer Service checks the sender's account balance
2. Transfer Service verifies the recipient account exists and is active

**Phase 4 - Execution (if funds are sufficient):**
1. Sender's account is debited
2. Recipient's account is credited
3. Transaction is logged in the database

**Phase 5 - Confirmation:**
1. Success message and receipt are shown to the customer
2. Email/SMS notification is sent

**Alternative Flow (Insufficient Funds):**
- If the sender has insufficient balance, the transfer is rejected and an error message is displayed

### 4.4 Key Interactions Shown

- Synchronous calls (solid arrows) for request-response interactions
- Return messages (dashed arrows) for responses
- **alt** fragment for conditional logic (sufficient vs. insufficient funds)
- Clear separation into phases using UML interaction fragments

---

## 5. UML Tool Selection

### 5.1 Tool: PlantUML

We chose **PlantUML** for creating all diagrams because:

1. **Text-based** - Diagrams are written as code, making them easy to version control with Git
2. **Free and open source** - No licensing costs
3. **Precise control** - Every element is explicitly defined, ensuring consistency
4. **Reproducible** - Diagrams can be regenerated identically from the source files
5. **Multiple output formats** - Supports PNG, SVG, PDF export

### 5.2 How to Render the Diagrams

The `.puml` files in the `diagrams/` folder can be rendered using:

- **PlantUML Online Server**: Paste the code at https://www.plantuml.com/plantuml/uml
- **VS Code Extension**: Install "PlantUML" extension and press Alt+D to preview
- **Command line**: `java -jar plantuml.jar diagrams/*.puml`

### 5.3 Experience Reflection

PlantUML provides full control over diagram elements through a simple text syntax. The learning curve is gentle - the syntax is intuitive (e.g., `-->` for arrows, `class ClassName { }` for classes). The main advantage over visual tools like Lucidchart is that diagrams are stored as code, making collaboration through Git seamless.

---

## 6. Comparison of UML Diagrams

### 6.1 Use Case Diagram vs. Class Diagram

| Aspect | Use Case Diagram | Class Diagram |
|--------|-----------------|---------------|
| **Purpose** | Shows what the system does from the user's perspective | Shows how the system is structured internally |
| **Focus** | System functionality and user interactions | Data structure, attributes, methods, and relationships |
| **Elements** | Actors, use cases, associations, include/extend | Classes, attributes, methods, inheritance, associations |
| **Perspective** | External (user-facing) | Internal (developer-facing) |
| **When to use** | Early in requirements gathering phase | During system design and before implementation |
| **Audience** | Stakeholders, clients, product managers | Developers, database architects, technical leads |

### 6.2 When to Use Each

- **Use Case Diagram**: Use during the **requirements analysis** phase to capture what the system should do and who interacts with it. It helps stakeholders validate that all user needs are covered.
- **Class Diagram**: Use during the **system design** phase to define the data model, object structure, and relationships. It guides implementation and database design.

### 6.3 How They Complement Each Other

These diagrams work together in the design process:
1. The **Use Case Diagram** identifies *what* the system must do (functional requirements)
2. The **Class Diagram** defines *how* the system is structured to fulfill those requirements
3. Each use case maps to methods in one or more classes
4. Together, they ensure that the system's structure supports all required functionality

For example, the "Transfer Funds" use case maps directly to the `Transfer` class with its `execute()` and `validate()` methods, and to the `Account` class with its `deposit()` and `withdraw()` methods.

---

## 7. How UML Contributes to Software Development

### 7.1 Communication Benefits

- **Common language**: UML provides a standardized visual language that developers, stakeholders, and clients can all understand
- **Reduces ambiguity**: Visual diagrams eliminate misinterpretation that often occurs with text-only requirements
- **Cross-team alignment**: Different teams (frontend, backend, QA) share the same understanding of the system

### 7.2 Identifying Flaws Before Coding

Modeling the system with UML diagrams before writing code helps:
- **Detect missing requirements**: If an actor has no associated use case, functionality is missing
- **Find relationship errors**: The Class Diagram reveals incorrect or missing relationships between entities
- **Validate process flows**: The Sequence Diagram exposes gaps in the interaction flow (e.g., missing error handling for insufficient funds)
- **Reduce rework**: Fixing a diagram takes minutes; fixing implemented code takes hours or days

---

## 8. Files Included

```
banking-system-uml/
├── diagrams/
│   ├── use-case-diagram.puml      (Task 1: Use Case Diagram source)
│   ├── class-diagram.puml         (Task 2: Class Diagram source)
│   └── sequence-diagram.puml      (Task 3: Sequence Diagram source)
├── images/                        (Rendered diagram images go here)
├── REPORT.md                      (This report)
└── PRESENTATION_OUTLINE.md        (Presentation structure)
```

---

## 9. Conclusion

This project demonstrates the application of three core UML diagrams to model a Banking System:

1. **Use Case Diagram** - Captured 4 actors and 14 use cases with include/extend relationships
2. **Class Diagram** - Modeled 12 classes with inheritance, association, and composition relationships
3. **Sequence Diagram** - Illustrated the complete fund transfer process with authentication, validation, execution, and error handling

These diagrams collectively provide a comprehensive view of the system from both the user's perspective (Use Case) and the developer's perspective (Class and Sequence), enabling effective communication and reducing the risk of implementation errors.

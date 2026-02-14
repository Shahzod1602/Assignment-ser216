# Banking System UML - Presentation Outline

Use this outline to build your PowerPoint/Canva presentation.

---

## Slide 1: Title Slide
- **Title**: Banking System - UML Design
- **Subtitle**: Software Modeling with Use Case, Class, and Sequence Diagrams
- **Group members**: [Names]
- **Date**: February 2026

---

## Slide 2: System Overview
- Banking System manages financial operations
- Supports: Account management, Deposits, Withdrawals, Transfers, Loans
- Multiple user types interact with the system
- Accessed via Web/Mobile App and ATM

---

## Slide 3: Actors in the System
- **Customer** - Holds accounts, performs transactions
- **Bank Teller** - Processes transactions, verifies identity
- **Admin** - Manages system, generates reports
- **ATM Machine** - Self-service banking operations

---

## Slide 4: Use Case Diagram (Member 1)
- Insert the rendered Use Case Diagram image
- Highlight: 4 actors, 14 use cases
- Point out `<<include>>` relationships (Authentication is required for financial operations)
- Point out `<<extend>>` relationship (Transaction History extends Balance View)

---

## Slide 5: Use Case Diagram - Key Points
- Every financial operation **includes** authentication
- ATM supports a subset of operations (deposit, withdraw, balance)
- Bank Teller can act on behalf of customers
- Admin has separate management-focused use cases

---

## Slide 6: Class Diagram (Member 2)
- Insert the rendered Class Diagram image
- 12 classes with attributes and methods
- Two inheritance hierarchies: User and Account

---

## Slide 7: Class Diagram - Relationships
- **Inheritance**: User → Customer, BankTeller, Admin
- **Inheritance**: Account → SavingsAccount, CheckingAccount
- **Association**: Customer owns Accounts (1 to many)
- **Association**: Account records Transactions (1 to many)
- Explain how the Class Diagram maps to database tables

---

## Slide 8: Sequence Diagram (Member 3)
- Insert the rendered Sequence Diagram image
- Models the **Fund Transfer** process
- 7 participants interacting over time

---

## Slide 9: Sequence Diagram - Flow Breakdown
1. **Authentication Phase** - Customer logs in, credentials validated
2. **Validation Phase** - Check balance, verify recipient
3. **Execution Phase** - Debit sender, credit recipient
4. **Confirmation Phase** - Show receipt, send notification
5. **Error Handling** - Insufficient funds alternative flow

---

## Slide 10: UML Tool - PlantUML
- Text-based diagram tool (diagrams written as code)
- Free and open source
- Advantages: version control, reproducibility, precise control
- How to render: Online server, VS Code extension, or command line

---

## Slide 11: Use Case vs Class Diagram Comparison
| Aspect | Use Case | Class |
|--------|----------|-------|
| Purpose | What the system does | How it's structured |
| Focus | User interactions | Data & relationships |
| Audience | Stakeholders | Developers |
| Phase | Requirements | Design |

They complement each other: Use Cases define *what*, Classes define *how*

---

## Slide 12: Benefits of UML in Software Development
- **Communication**: Common visual language for all team members
- **Early flaw detection**: Find missing requirements before coding
- **Reduces rework**: Fixing a diagram is faster than fixing code
- **Documentation**: Serves as living documentation of the system

---

## Slide 13: Conclusion
- UML provides a structured approach to system design
- Three diagram types cover different perspectives:
  - Use Case → functional requirements
  - Class → system structure
  - Sequence → dynamic behavior
- Modeling before coding saves time and reduces errors

---

## Slide 14: Q&A
- Open for questions from classmates and instructor
- Be prepared to explain:
  - Why you chose specific relationships
  - How diagrams map to actual code
  - Alternative design decisions you considered

# Novabank Banking System Architecture

## 1. Overview
NovaBank is a relational database-backed banking system that manages users, accounts, and transactions. It supports deposits, withdrawals, transfers, multi-currency operations, and includes auditing mechanisms for account changes.

---

## 2. Database Architecture

### 2.1 Tables

#### **Users**
Stores system users.

| Column      | Data Type     | Constraints          | Description                     |
|------------|---------------|--------------------|---------------------------------|
| user_id    | NUMBER        | PRIMARY KEY         | Unique identifier for a user    |
| full_name  | VARCHAR2(100) | NOT NULL            | Full name of the user           |
| email      | VARCHAR2(100) | UNIQUE, NOT NULL    | Email address of the user       |
| phone      | VARCHAR2(15)  |                    | Phone number of the user        |
| created_at | DATE          | DEFAULT SYSDATE     | Account creation date           |

---

#### **Accounts**
Holds account details.

| Column        | Data Type     | Constraints                  | Description                           |
|---------------|---------------|------------------------------|---------------------------------------|
| account_id    | NUMBER        | PRIMARY KEY                 | Unique identifier for an account      |
| user_id       | NUMBER        | FOREIGN KEY → Users(user_id)| Linked user                            |
| balance       | NUMBER(15,2)  | DEFAULT 0                   | Current account balance               |
| currency_code | VARCHAR2(3)   | FOREIGN KEY → Currencies    | Currency type of the account          |
| created_at    | DATE          | DEFAULT SYSDATE             | Account creation date                  |

---

#### **Transactions**
Records all monetary operations.

| Column           | Data Type     | Description                        |
|-----------------|---------------|------------------------------------|
| transaction_id   | NUMBER        | Primary key                        |
| account_id       | NUMBER        | FK → Accounts(account_id)          |
| transaction_type | VARCHAR2(10)  | 'deposit', 'withdrawal', 'transfer_in', 'transfer_out' |
| amount           | NUMBER(15,2)  | Transaction amount                  |
| transaction_date | DATE          | Defaults to SYSDATE                 |
| details          | VARCHAR2      | Optional description                |

---

#### **Currencies**
Defines supported currencies.

| Column        | Data Type    | Constraints  | Description         |
|---------------|-------------|-------------|-------------------|
| currency_code | VARCHAR2(3) | PRIMARY KEY | Currency code       |
| name          | VARCHAR2(50)|             | Currency full name  |
| symbol        | VARCHAR2(10)|             | Currency symbol     |

---

#### **ExchangeRates**
Stores currency exchange rates.

| Column        | Data Type   | Description                      |
|---------------|------------|----------------------------------|
| rate_id       | NUMBER     | Primary key                      |
| from_currency | VARCHAR2(3)| FK → Currencies(currency_code)   |
| to_currency   | VARCHAR2(3)| FK → Currencies(currency_code)   |
| rate          | NUMBER(10,4)| Exchange rate                    |
| updated_at    | DATE       | Defaults to SYSDATE              |

---

#### **Accounts Log** (Audit Table)
Captures balance changes for auditing.

| Column       | Data Type     | Description                 |
|-------------|---------------|-----------------------------|
| account_id   | NUMBER       | Linked account             |
| old_balance  | NUMBER(15,2) | Balance before change      |
| new_balance  | NUMBER(15,2) | Balance after change       |
| change_type  | VARCHAR2     | Type of transaction        |
| changed_at   | TIMESTAMP    | Time of change             |

---

## 3. Packages and Procedures

### **user_account_pkg**
- **Function:** `create_user_with_account_fn` → Creates a user and account, returns a summary message.  
- **Procedure:** `create_user_with_account_proc` → Calls the function and outputs result.

---

### **Deposit Procedure**
- `deposit_money(p_account_id, p_amount, p_details)`  
- Validates positive amount  
- Updates account balance  
- Inserts transaction record  
- Commits transaction  

---

### **Withdrawal Procedure**
- `withdraw_money(p_account_id, p_amount, p_details)`  
- Validates amount and sufficient balance  
- Deducts balance  
- Logs transaction  
- Commits transaction  

---

### **Transfer Procedure**
- `transfer_money(p_sender_account_id, p_receiver_account_id, p_amount, p_details)`  
- Validates positive amount and sufficient sender balance  
- Converts currency if needed using `ExchangeRates`  
- Updates sender and receiver balances  
- Logs both transactions  
- Commits transaction  

---

### **User Profile Procedure**
- `show_user_profile(p_user_id)`  
- Fetches user and account info  
- Displays transaction history  
- Handles missing users gracefully  

---

## 4. Triggers

### **trg_log_account_changes**
- Trigger: AFTER UPDATE on `Accounts.balance`  
- Inserts record into `Accounts_Log` with old/new balance, change type, and timestamp  

---

## 5. System Workflow

1. **User Registration**
   - User created via `user_account_pkg` → auto-creates account  
   - Outputs user ID, account ID, currency  

2. **Transactions**
   - Deposit, Withdraw, Transfer  
   - Updates balances and logs transactions  
   - Triggers record audit logs  

3. **Profile Viewing**
   - `show_user_profile` outputs user info, account info, and transaction history  

4. **Multi-Currency Handling**
   - Transfers between different currencies use `ExchangeRates` for conversion  

---

## 6. Key Notes
- All monetary operations are transactional (COMMIT/ROLLBACK).  
- Error handling prevents invalid operations (negative deposits, insufficient funds).  
- DBMS_OUTPUT used for console logging in PL/SQL.  
- Context-sensitive triggers store change types for accurate auditing.

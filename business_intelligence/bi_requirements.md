# Business Intelligence Requirements - Novank System

## 1. Overview
The Business Intelligence (BI) requirements define the data and reporting needs to support management decision-making for NovaBank. It focuses on analyzing transactions, account activity, user behavior, and financial KPIs.

---

## 2. Stakeholders
- **Management:** Needs insights into revenue, user growth, transaction volumes.  
- **Operations Team:** Monitors deposits, withdrawals, transfers, and suspicious activity.  
- **Finance Team:** Tracks multi-currency balances, exchange rate trends, and liquidity.  

---

## 3. Functional Requirements
1. **User Analytics**
   - Total number of active users
   - New user sign-ups per day/week/month
   - Users per currency type

2. **Account Analytics**
   - Total accounts by currency
   - Average account balance
   - Accounts with negative or low balances

3. **Transaction Analytics**
   - Total deposits, withdrawals, and transfers
   - Transactions by type, currency, and date
   - High-value transactions for monitoring

4. **Audit & Compliance**
   - Changes to account balances (audit logs)
   - Suspicious or failed transactions
   - Regulatory reporting requirements

5. **Multi-Currency Monitoring**
   - Exchange rate trends
   - Currency-specific transaction volumes
   - Cross-currency transfers

---

## 4. Non-Functional Requirements
- Reports must be generated in near real-time
- Data must be accurate and consistent with the core database
- Support export to PDF, Excel, or CSV
- Secure access control to BI dashboards

---

## 5. Data Sources
- Users table
- Accounts table
- Transactions table
- ExchangeRates table
- Accounts_Log table

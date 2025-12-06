# KPI Definitions - NovaBank System

## 1. Overview
Key Performance Indicators (KPIs) are metrics used to measure the performance, health, and growth of the banking system.

---

## 2. KPIs

| KPI Name                     | Definition                                                 | Calculation / Notes                                       |
|-------------------------------|-----------------------------------------------------------|-----------------------------------------------------------|
| Total Active Users             | Number of users with at least one transaction in last 30 days | Count(distinct user_id) where transaction_date >= SYSDATE - 30 |
| New Users                      | Number of users registered in a specific period           | Count of user_id from Users where created_at in period   |
| Total Deposits                 | Total sum of deposit transactions                           | SUM(amount) where transaction_type='deposit'            |
| Total Withdrawals              | Total sum of withdrawal transactions                        | SUM(amount) where transaction_type='withdrawal'         |
| Total Transfers                | Total sum of money transferred                               | SUM(amount) where transaction_type in ('transfer_in','transfer_out') |
| Average Account Balance        | Mean balance across all accounts                             | AVG(balance) from Accounts                                |
| High-Value Transactions        | Number of transactions above threshold                       | COUNT(*) where amount > threshold                         |
| Cross-Currency Transfers       | Number of transfers between different currencies            | COUNT(*) join ExchangeRates where sender_currency != receiver_currency |
| Exchange Rate Volatility       | Fluctuation in exchange rates over period                   | MAX(rate) - MIN(rate) per currency pair                  |
| Suspicious Activity Count      | Transactions flagged for audit                                | COUNT(*) from Accounts_Log or transactions flagged      |

---

## 3. Notes
- All KPIs should be filterable by date range, currency, or account type.
- Metrics must reconcile with core database tables to ensure accuracy.
- High-value and cross-currency KPIs help monitor risk and regulatory compliance.

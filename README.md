# NovaBank – Peer-to-Peer Currency Transfer System

An Oracle-based backend project that enables users to deposit, withdraw, and transfer money across multiple currencies (FRW, USD, EUR, KES) with automatic currency conversion and full transaction auditing.

---

## 👥 Identity

| Name             | Student ID     |
|------------------|----------------|
| MUGISHA Espoir   | 27824          |

---

## 🧠 Problem Statement

Managing peer-to-peer transactions across different currencies is challenging. Users often struggle with manual conversions, high transfer fees, and delays in traditional banking systems.

## Key Objectives

Traditional banking and financial apps often suffer from:
- Manual currency conversion delays.
- Lack of transparency in user transactions.
- Limited real-time logging and auditability.

**NovaBank** solves this by:
- Allowing instant deposits (form-based).
- Enabling cross-currency transfers with exchange rate logic.
- Recording every transaction and balance change in real-time.

---

## Quick start instructionS

## 1. Requirements
Before running the NovaBank project, ensure the following tools are installed:

Oracle Database 21c (XE or Standard)

SQL Developer 23+

A working Pluggable Database (PDB)

## 2. Start Oracle Database Services
start them using Command Prompt:

net start OracleServiceXE

net start OracleXETNSListener

## 3. Connect to the Database in SQL Developer
Open SQL Developer

Create a new connection and fill in these fields:

(Connection Name, Username, Password, Service Name'use your PDB database you have created')

Click Test → ensure status is Success

Click Connect


## 4. Load the NovaBank Database Schema

Once connected:

Open the SQL file:
NovaBank_Schema.sql
(contains tables, sequences, triggers, and sample data)

Run the script using:
(F5) Run Script

Confirm that all objects are created successfully:

Tables: ACCOUNTS, CUSTOMERS, TRANSACTIONS, etc.

Sequences for primary keys

Triggers for logging activities

## 5. Run NovaBank Queries

After importing the schema, you can execute any NovaBank operations, such as:

SELECT * FROM customers;
SELECT * FROM accounts;

## 6. (Optional) Switch to NovaBank PDB

If starting in CDB$ROOT, switch to the correct PDB:

ALTER SESSION SET CONTAINER = WEN_27824_ESPOIR_NOVABANK_FINANCIALSYSTEM_DB;
---

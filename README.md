# Tech Repair Shop Database Project

A relational database project modeled for a fictional tech repair business. Designed in PostgreSQL, this project demonstrates key database concepts including normalization, security, triggers, role-based access, and analytics queries.

---

## 🔹 Project Scope
This project meets the following SQL objectives:
- **Data normalization** (3NF: separate customers, devices, orders, etc.)
- **User roles & security** with login restrictions and row-level policies
- **Constraints** including PKs, FKs, `CHECK`, and `UNIQUE`
- **Trigger-based automation** for timestamps
- **Aggregate queries** to support reporting and insights
- **Maintenance logic** via vacuuming and reindexing
- **ACID-aware design** (e.g. safe updates, FK enforcement)

---

## 📅 Use Case: Tech Repair Store
- Customers bring electronics for service
- Devices are tied to customer records
- Repairs are handled by technicians
- Parts used per repair are tracked
- Role-based system controls technician, admin, and auditor permissions

---

## 📑 Schema Overview
Each table is designed with purpose and integrity in mind:

| Table        | Description                                             |
|--------------|---------------------------------------------------------|
| `customer`   | Stores contact info and identity                        |
| `device`     | Links devices to customers with serial and status info |
| `repair_order` | Tracks issues, resolutions, and assigned technicians  |
| `part_used`  | Tracks part name, quantity, and unit cost              |
| `technician` | Stores staff contact info                              |
| `user_role`  | Login-based access and permission control              |

> See: ![Entity Relationship Diagram](screenshots/ERD/ERD.png)

---

## 🔐 Roles & Permissions
### Login Roles
- `tech_login` → Basic tech access (limited RLS view)
- `admin_login` → Full CRUD access across all tables
- `auditor_login` → Read-only access

### Row-Level Security (RLS)
- Enabled on `user_role`
- Techs can only read/update their own rows
- Admins are exempt from RLS restrictions

<details>
  <summary>📂 View SQL snippets for Roles & Permissions</summary>

  ![Creating login roles with secure passwords in PostgreSQL](screenshots/Security_Permissions_Roles/p_s1.png)

---

  ![Enabling row-level security and creating access policy for technicians](screenshots/Security_Permissions_Roles/p_s2.png)

---

  ![Granting permissions on tables to tech and admin roles in PostgreSQL](screenshots/Security_Permissions_Roles/p_s3.png)

</details>

---

## 📋 Trigger Automation
Two trigger functions simplify timestamp management:

- `set_timestamp()` → Updates `updated_at` before every row change
- `set_closed_timestamp()` → Automatically fills `closed_at` when a repair is marked 'Closed'

Trigger scripts are in `trigger_creation.sql`

<details>
  <summary>📂 View SQL trigger creation snippets</summary>

  ![PostgreSQL trigger function to update the updated_at timestamp](screenshots/Triggers/trigger1.png)

---

  ![PostgreSQL trigger function to update the updated_at timestamp](screenshots/Triggers/trigger2.png)

</details>

---

## 🔢 Indexed Performance
PostgreSQL indexes added for speed and searchability:
- Email, phone on `customer`
- Status on `repair_order`
- Serial number on `device`
- Composite index on `(technician_id, status)`
- Full-text GIN indexes on `model` and `issue_description`


<details>
  <summary>📂 View SQL index & maintenance function snippets</summary>

  ![Creating indexes on customer email and phone fields](screenshots/Indexes/idx1.png)
  
---

  ![Composite index on technician ID and status in repair_order](screenshots/Indexes/idx2.png)

---

  ![Full-text search indexes for device models and repair issues](screenshots/Indexes/idx3.png)

</details>

---

## 🔤 Query Demonstrations
Twelve sample queries were written and tested to demonstrate insights, reporting, and relational joins. Topics include:
- Devices per customer
- Parts and cost per repair
- Most used parts
- Technicians with most repairs
- Average repair cost by technician

Each query is saved in `queries.sql` and visually shown in:
## 🧪 Query Demonstrations

Below are examples of real SQL queries and the results they return.

<details>
  <summary>📂 View queries and results</summary>

<details>
  <summary>🔍 Query 1: Get all customers</summary>

  **SQL to list all customers:**

  ![SQL to list all customers](screenshots/Queries/query1.png)

  **Result set for all customers:**

  ![Result set for all customers](screenshots/Queries/answer1.png)

</details>


  <details>
  <summary>🔍 Query 2: Get devices for customer #1</summary>

  **SQL to find devices for customer ID 1:**

  ![SQL to find devices for customer ID 1](screenshots/Queries/query2.png)

  **Result showing devices owned by customer 1:**

  ![Result showing devices owned by customer 1](screenshots/Queries/answer2.png)

</details>

<details>
  <summary>🔍 Query 3: List all open repair orders</summary>

  **SQL to list open repair orders:**

  ![SQL to list open repair orders](screenshots/Queries/query3.png)

  **Result set showing open repairs:**

  ![Result set showing open repairs](screenshots/Queries/answer3.png)

</details>

<details>
  <summary>🔍 Query 4: List customer names with their devices</summary>

  **SQL to join customers and their devices:**

  ![SQL to join customers and their devices](screenshots/Queries/query4.png)

  **Result showing each customer’s device info:**

  ![Result showing each customer’s device info](screenshots/Queries/answer4.png)

</details>

<details>
  <summary>🔍 Query 5: Get repair orders with tech & device info</summary>

  **SQL to join repair orders, devices, and technicians:**

  ![SQL to join repair orders, devices, and technicians](screenshots/Queries/query5.png)

  **Result showing detailed repair info:**

  ![Result showing detailed repair info](screenshots/Queries/answer5.png)

</details>

<details>
  <summary>🔍 Query 6: Show parts used in each repair order</summary>

  **SQL to show parts used per repair:**

  ![SQL to show parts used per repair](screenshots/Queries/query6.png)

  **Result showing parts and costs:**

  ![Result showing parts and costs](screenshots/Queries/answer6.png)

</details>

<details>
<summary>🔍 Query 7: Count how many devices each customer owns</summary>

**[SQL to count devices per customer]**  
![Query 7](screenshots/Queries/query7.png)  

**[Result set showing total devices per customer]**  
![Answer 7](screenshots/Queries/answer7.png)  

</details>

<details>
<summary>🔍 Query 8: Get total repair cost per order</summary>

**[SQL to calculate total cost of repairs]**  
![Query 8](screenshots/Queries/query8.png)  

**[Result set showing total cost per order]**  
![Answer 8](screenshots/Queries/answer8.png)  

</details>

<details> <summary>🔍 Query 9: List all repairs completed by each technician</summary>

![Query9](screenshots/Queries/query9.png)


![Answer9](screenshots/Queries/answer9.png)

</details>

<details>
  <summary>🔍 Query 10: Customer who spent the most on repairs</summary>

  **SQL to find the top-spending customer:**

  ![Query 10](screenshots/Queries/query10.png)

  **Result showing the customer who spent the most:**

  ![Answer 10](screenshots/Queries/answer10.png)

</details>

<details>
  <summary>🔍 Query 11: Avg. repair cost per technician (Closed only)</summary>

  **SQL to calculate average cost per technician:**

  ![Query 11](screenshots/Queries/query11.png)

  **Result showing average repair cost per tech:**

  ![Answer 11](screenshots/Queries/answer11.png)

</details>

<details>
  <summary>🔍 Query 12: Show all non-Closed repair orders</summary>

  **SQL to display open and in-progress repair orders:**

  ![Query 12](screenshots/Queries/query12.png)

  **Result showing repair orders that are not closed:**

  ![Answer 12](screenshots/Queries/answer12.png)

</details>
</details>

---

## 📂 SQL Scripts Included
All database logic is broken out cleanly in:
- `table_creation.sql`
- `trigger_creation.sql`
- `roles.sql`
- `seed_data.sql`
- `queries.sql`
- `indexes.sql`

---

## 📅 Testing & Sample Data
20+ rows of realistic dummy data were inserted for testing devices, repairs, tech assignments, and repair parts. This enabled testing of joins, aggregation, filters, and policy enforcement.

See: `seed_data.sql`

---


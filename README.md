# 🐘 AWS RDS PostgreSQL Lab — Create a Database & Read Replica

In this lab, I learned how to **provision an Amazon RDS PostgreSQL database, configure its settings, create a read replica, verify its status, and finally delete both instances** safely.

---

## 📋 Lab Overview

**Goal:**

* Deploy a PostgreSQL RDS instance
* Configure engine version, storage, and instance class
* Retrieve connection details and verify status
* Create a **read replica** from the primary database
* Delete both the primary instance and its replica

**Learning Outcomes:**

* Use AWS Console to create RDS databases
* Configure PostgreSQL engine settings (version, storage, sizing)
* Understand how RDS handles automatic backups for replicas
* Create an RDS read replica and validate its availability
* Delete RDS instances safely and understand snapshot settings

---

## 🛠 Step-by-Step Journey

### **Step 1 — Log Into AWS Console**

Used the provided lab credentials and logged into the AWS Management Console.
Region set to **US East (N. Virginia)**.

---

### **Step 2 — Create a PostgreSQL RDS Instance**

From **RDS → Create Database**:

**Configuration details:**

| Setting                | Value                    |
| ---------------------- | ------------------------ |
| Create Method          | Standard Create          |
| Engine                 | PostgreSQL               |
| Version                | **15.15-R1**             |
| Template               | Dev/Test                 |
| Availability           | Single DB (Single-AZ)    |
| DB Identifier          | `my-postgreSQL-instance` |
| Master Username        | `postgres`               |
| Password               | Auto-generated           |
| Instance Class         | Burstable                |
| Storage Type           | GP2                      |
| Allocated Storage      | 30 GB                    |
| Storage Auto-Scaling   | **Disabled**             |
| Performance Insights   | **Disabled**             |
| Credentials Management | Self-managed             |

Saved the **master username** and **auto-generated password** for later.

---

### **Step 3 — Wait for the Database to Become Available**

After several minutes, the instance changed to:

```
Status: Available
```

---

### **Step 4 — Understand Automatic Backups for Read Replicas**

Initial guess:
➡️ “Maybe both source DB and reader replica get automatic backups.”

Correct answer:
✅ **RDS only creates automatic backups for the source instance — NOT the read replica.**

---

### **Step 5 — Retrieve Connection Details**

Checked database details:

* **Endpoint:** `my-postgreSQL-instance.xxxxxxxxxxxx.region.rds.amazonaws.com`
* **Resource ID:** `db-xxxxxxxxxxxxxxxxxxxx`

Both values were recorded for the lab questions.

---

### **Step 6 — Create a Read Replica**

**Actions → Create Read Replica**

**Read Replica Configuration:**

| Setting              | Value                                 |
| -------------------- | ------------------------------------- |
| DB Identifier        | `my-postgreSQL-instance-read-replica` |
| Instance Class       | Burstable                             |
| Storage Type         | GP2                                   |
| Allocated Storage    | 30 GB                                 |
| Storage Auto-Scaling | Disabled                              |
| Deployment           | Single-AZ                             |
| Performance Insights | Disabled                              |

Clicked **Create Read Replica**.

After a few minutes →

```
Status: Available
```

---

### **Step 7 — Delete Both RDS Instances**

Deleted **primary instance** and **read replica** with the following deletion settings:

* **Unchecked**: Create final snapshot
* **Unchecked**: Retain automatic backups
* **Checked**: Acknowledge deletion warning
* Typed **delete me** to confirm
* Repeated the same for the read replica

Both instances successfully deleted.

---

## ✅ Key Concepts Summary

| Concept               | Explanation                                                        |
| --------------------- | ------------------------------------------------------------------ |
| RDS engine versioning | Selected PostgreSQL 15.15-R1                                       |
| RDS backup behavior   | **Automatic backups only apply to the source DB**, not the replica |
| Read replica purpose  | Offloads **read queries** to improve performance                   |
| Instance storage      | GP2, 30GB, autoscaling disabled                                    |
| Deletion workflow     | Must disable snapshots & confirm destruction                       |

---

## 📌 Lab Summary

| Step                           | Status | Key Takeaways                                           |
| ------------------------------ | ------ | ------------------------------------------------------- |
| Create PostgreSQL RDS instance | ✅      | Configured engine, storage, credentials, instance class |
| Verify connection details      | ✅      | Retrieved endpoint + resource ID                        |
| Understand backup behavior     | ✅      | Only *source DB* gets automatic backups                 |
| Create read replica            | ✅      | Configured and launched successfully                    |
| Delete both DBs                | ✅      | No snapshots, no retained backups                       |

---

## 💡 Notes / Tips

* **Always save your master credentials** before leaving the creation page.
* RDS read replicas are **asynchronous** → slight replication lag possible.
* Read replicas are meant for **read-heavy workloads**, not write operations.
* Deleting without snapshots is permanent — use carefully.

---

## 🔗 References

* [Amazon RDS for PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)
* [Working with RDS Read Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)
* [RDS Backups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)

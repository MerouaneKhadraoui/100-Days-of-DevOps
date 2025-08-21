# Day 17: Install and Configure PostgreSQL

## Question

The `Nautilus` application development team has shared that they are planning to deploy one newly developed application on `Nautilus` infra in `Stratos DC`. The application uses PostgreSQL database, so as a pre-requisite we need to set up PostgreSQL database server as per requirements shared below:

PostgreSQL database server is already installed on the `Nautilus` database server.

**Task:**  

a. Create a database user `kodekloud_top` and set its password to `Rc5C9EyvbU`.

b. Create a database `kodekloud_db8` and grant full permissions to user `kodekloud_top` on this database.

`Note:` Please do not try to restart PostgreSQL server service.

---

## Solution

1. **SSwitch to the `postgres` user (default superuser in PostgreSQL)**

```bash
sudo su - postgres
```

---

2. **Enter PostgreSQL shell**

```bash
psql
```

---

3. **Create the user `kodekloud_top` with password `Rc5C9EyvbU`**

```sql
CREATE USER kodekloud_top WITH PASSWORD 'Rc5C9EyvbU';
```

---

4. **TCreate the database `kodekloud_db8`**

```sql
CREATE DATABASE kodekloud_db8;
```

---

5. **Grant all privileges on the database to the user**

```sql
GRANT ALL PRIVILEGES ON DATABASE kodekloud_db8 TO kodekloud_top;
```

---

6. **Exit psql**

```sql
\q
```

---

7. **Exit postgres user**

```bash
exit
```

---

## Verification

You can test by logging in with the new user:

```bash
psql -U kodekloud_top -d kodekloud_db8 -h localhost
```

It should ask for a password → enter `Rc5C9EyvbU`.

If login is successful, the setup is correct ✅
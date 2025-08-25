# Day 21: Set Up Git Repository on Storage Server

## Question

The Nautilus development team has provided requirements to the DevOps team for a new application development project, specifically requesting the establishment of a Git repository. Follow the instructions below to create the Git repository on the `Storage server` in the Stratos DC:

**Task:**

1. Utilize `yum` to install the `git` package on the `Storage Server`.

2. Create a bare repository named `/opt/demo.git` (ensure exact name usage).

---

## Solution


**Step 1 - Switch to Storage Server**

In KodeKloud Engineer, you first connect to the Storage Server:

```bash
ssh natasha@ststor01
# password: Bl@kW
```

---

**Step 2 - Install Git**

Update repos and install `git` using yum:

```bash
sudo yum install git -y
```

---

**Step 3 - Create Bare Git Repository**

A bare repository is required for central collaboration. Create it in `/opt`:

```bash
sudo git init --bare /opt/demo.git
```

---

**Step 4 - Verify Repository**

Check that it is indeed a bare repo:

```bash
ls -a /opt/demo.git
```

You should see files like:
.  ..  `HEAD`  `branches`  `config`  `description`  `hooks`  `info`  `objects`  `refs`

---

# Day 2: Temporary User Setup with Expiry

## Question

To support limited-duration access for the Nautilus project, the system admin team at `xFusionCorp Industries` requires the creation of a temporary user account.

**Task:**  
Create a user named `anita` on `App Server 3` in the Stratos Datacenter, with an expiry date of `2024-02-17`.

---

## Solution

```bash
ssh banner@stapp03
sudo useradd -e 2024-02-17 anita
```
Type the password for the user when prompted to complete the user creation process.

---

## Verification

To verify the expiry date for the user, run:

```bash
sudo chage -l anita
```

The output should show the account expiration date as `Feb 17, 2024`.

**Example Output:**

```bash
Account expires    : Feb 17, 2024
```

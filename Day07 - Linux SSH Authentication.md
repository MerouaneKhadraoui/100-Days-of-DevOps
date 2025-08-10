# Day 7: Linux SSH Authentication

## Question

The system admins team of `xFusionCorp Industries` has set up some scripts on `jump host` that run on regular intervals and perform operations on all app servers in `Stratos Datacenter`. To make these scripts work properly we need to make sure the `thor` user on jump host has password-less SSH access to all app servers through their respective sudo users (i.e `tony` for app server 1). Based on the requirements, perform the following:

**Task:**  

Set up a password-less authentication from user `thor` on jump host to all app servers through their respective sudo users.

---

## Solution

Generated new SSH key pair

```bash
ssh-keygen -t rsa -N ""
```
Copied public key to each target server

```bash
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
```

---

## Verification

Check jump host to all app servers:

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

**Example Output:**

```bash
thor@jumphost ~$ ssh tony@stapp01
[tony@stapp01 ~]$ exit
logout
Connection to stapp01 closed.
thor@jumphost ~$ ssh steve@stapp02
[steve@stapp02 ~]$ exit
logout
Connection to stapp02 closed.
thor@jumphost ~$ ssh banner@stapp03
[banner@stapp03 ~]$ exit
logout
Connection to stapp03 closed.
```


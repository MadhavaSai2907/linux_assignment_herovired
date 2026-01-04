# linux_assignment_herovired
This Repo is dedicated to Linux assigment.

---

# Task 1: System Monitoring Setup

## Objective

Configure basic system monitoring to ensure **health, performance visibility, and capacity planning** for a Linux-based development environment.

---

## Scenario

* The development server experiences **intermittent performance issues**
* Developers require **visibility into system resource usage**
* System metrics must be **tracked consistently** for troubleshooting and planning

---

## Tools Used

* `htop` / `nmon` – CPU, memory, and process monitoring
* `df` – Filesystem-level disk usage
* `du` – Directory-level disk usage
* `ps` – Process-level resource analysis
* Cron + log files – Reporting and history

---

## Monitoring Setup Steps

### 1. Install Monitoring Tools
To check **CPU usage, memory usage, and running processes**, we need to install **`htop`**.

On this server, `htop` is **not available in the default repositories**.  
Therefore, we must first install **EPEL (Extra Packages for Enterprise Linux)** and then install `htop`.

```bash
yum install epel-release
```
<img width="858" height="137" alt="epel for htop" src="https://github.com/user-attachments/assets/eae3cba8-dfb4-47ed-af11-a8f6fd624ec1" />

```bash
yum install htop nmon
```
**htop**

<img width="1125" height="553" alt="htop install" src="https://github.com/user-attachments/assets/86a09cc4-d5df-40f8-8b7c-20a87de4e8da" />


**nmon**

<img width="731" height="301" alt="3-nmon install" src="https://github.com/user-attachments/assets/7f4a1050-136f-49d6-9675-6ddd95e3a0fa" />

---

### 2. CPU, Memory, and Process Monitoring

* **htop**: Interactive, real-time monitoring
* **nmon**: Performance and capacity snapshots

```bash
htop
nmon
```
**htop output**

<img width="1878" height="941" alt="4-htop output" src="https://github.com/user-attachments/assets/513e563d-18ba-470d-a4ae-26e495fddf9f" />

**nmon output**

<img width="1918" height="1030" alt="5-nmon output" src="https://github.com/user-attachments/assets/4703cdf1-6d9e-4fda-adcd-ec76ebdfe292" />

---

### 3. Disk Usage Monitoring

**Filesystem-level usage:**

Disk usage monitoring is required to ensure **sufficient storage availability** and to support **capacity planning**.

### Purpose of `df`
- `df` (Disk Free) is used to monitor **filesystem-level disk usage**
- It shows total space, used space, and available space for each mounted filesystem
- Helps identify if the disk is nearing full capacity

```bash
df -h
```
<img width="1617" height="362" alt="6-df output" src="https://github.com/user-attachments/assets/ec67dbed-1a6c-419a-8e77-2eec0fe2fa87" />



**Directory-level usage:**

### Purpose of `du`
- `du` (Disk Usage) is used to monitor **directory-level disk consumption**
- Helps identify which directories are consuming the most space
- Useful for troubleshooting storage issues

```bash
du -h | head
```
<img width="731" height="392" alt="du top10" src="https://github.com/user-attachments/assets/aee95ff0-64ad-447e-9e5b-7de0be9975bf" />

**`du` for home directory**

<img width="825" height="285" alt="du home directory output" src="https://github.com/user-attachments/assets/e8ab4e43-71fb-4cdf-9ff2-be726ed226a3" />

---

### 4. Identify Resource-Intensive Processes

To analyze system performance issues, we identify processes that consume the **highest CPU and memory resources**.

**Top CPU consumers:**

Below are the **top 10 processes that are consuming CPU**:

```bash
ps -eo pid,user,cmd,%cpu,%mem --sort=-%cpu | head
```
<img width="1320" height="397" alt="top 10 cpu" src="https://github.com/user-attachments/assets/5c790816-6daa-4e04-8398-b4f69c378b79" />


**Top Memory consumers:**

Below are the **top 10 processes that are consuming MEMORY**:

```bash
ps -eo pid,user,cmd,%mem,%cpu --sort=-%mem | head
```

<img width="1372" height="396" alt="top 10 memory" src="https://github.com/user-attachments/assets/5def11a5-6847-48ec-9c0b-93de45626931" />

---

### 5. Reporting and Logs

To generate system monitoring reports, create a dedicated directory and store all reports in one centralized location.

```bash
mkdir -p /var/log/system-monitoring
```

Generate reports:

```bash
df -h > /var/log/system-monitoring/disk_report.log
ps -eo pid,user,cmd,%cpu,%mem --sort=-%cpu | head >> /var/log/system-monitoring/process_report.log
```

**disk_report**
<img width="1817" height="492" alt="image" src="https://github.com/user-attachments/assets/984ed704-8688-43e6-a19d-c955ba5279d5" />


**process_report**
<img width="1888" height="617" alt="process output" src="https://github.com/user-attachments/assets/f1ac1096-99ab-4622-a071-dbb3afab602b" />


## Outcome

* Real-time visibility into **CPU, memory, disk, and processes**
* Ability to **identify performance bottlenecks quickly**
* Logged data for **historical analysis and capacity planning**
---

## Conclusion

This setup provides a **lightweight yet effective monitoring baseline**, suitable for development environments and early-stage production systems.

---

# Task 2: User Management and Access Control

## Objective
Set up secure user accounts and access controls for new developers to ensure **isolation, security, and proper password management**.

---

## Scenario
- Two new developers, **Sarah** and **Mike**, require system access
- Each developer must have an **isolated working directory**
- Password policies must enforce **security and expiration**
- Access to directories must follow the **principle of least privilege**

---

## 1. Create User Accounts

Create user accounts for Sarah and Mike with home directories.

```bash
useradd Sarah
useradd mike
```

Verfiy Users Creation

```bash
id Sarah
id mike
```

<img width="959" height="155" alt="image" src="https://github.com/user-attachments/assets/7d76d81a-9cfa-4eaf-8ba1-53e36c05136e" />


**Check in home directory**

```bash
cd /home
```


<img width="547" height="208" alt="home directory" src="https://github.com/user-attachments/assets/24f4d668-1748-4839-9963-32842709a53d" />



## 2. Set Secure Passwords

Assign passwords for both users.

```bash
passwd Sarah
passwd mike
```
**Sarah**

<img width="745" height="292" alt="Sarah" src="https://github.com/user-attachments/assets/0b2b5c73-0ba7-4728-b31b-af1032800f3f" />

**Mike**

<img width="745" height="288" alt="Mike" src="https://github.com/user-attachments/assets/131f3343-12de-49b5-ad3c-9116ead7abe6" />


---

## 3. Create Dedicated Workspace Directories

Each user is provided with a dedicated workspace directory to ensure isolation.

```bash
mkdir -p /home/Sarah/workspace
mkdir -p /home/mike/workspace
```

Assign ownership:

```bash
sudo chown -R Sarah:Sarah /home/Sarah/workspace
sudo chown -R mike:mike /home/mike/workspace
```

---

## 4. Apply Secure Permissions

Restrict access so that only the respective user can access their workspace.

```bash
chmod 700 /home/Sarah/workspace
chmod 700 /home/mike/workspace
```

Permission `700` ensures:

* Owner: full access
* Group & Others: no access

---

## 5. Enforce Password Expiration Policy

To improve security, enforce a password expiration policy of **30 days**.

```bash
chage -M 30 Sarah
chage -M 30 mike
```

Verify password policy:

```bash
chage -l Sarah
chage -l mike
```

<img width="1422" height="843" alt="password change" src="https://github.com/user-attachments/assets/c38a6f98-fe83-400f-a3cc-794e132d0366" />

---

## 6. Grant Sudo Access Using visudo

To allow Sarah and Mike to perform administrative operations securely, sudo access is granted using the `visudo` command.

The `visudo` utility is used because it:
- Safely edits the sudoers file
- Performs syntax validation before saving
- Prevents misconfiguration that could lock out administrative access

```bash
visudo
```

<img width="575" height="156" alt="Sudo permissions" src="https://github.com/user-attachments/assets/7bc20691-81d5-4d80-9e9a-8281defc8d0b" />


## 7. Verify User Isolation and Access Control

To validate that user access restrictions are working correctly, switch to the **Sarah** user and perform access tests.

Switch to Sarah:
```bash
su Sarah
````

Create a new directory inside Sarah’s workspace:

```bash
sudo mkdir Firstfolder
```

This operation succeeds because Sarah owns her workspace.

Next, attempt to create a directory inside Mike’s workspace:

```bash
sudo mkdir /home/mike/workspace/mike_folder
```

This operation fails with a **Permission denied** error, confirming that:

* Sarah does not have access to Mike’s workspace
* User isolation and directory permissions are enforced correctly

<img width="1603" height="113" alt="access issue for ceating folder" src="https://github.com/user-attachments/assets/0002dabf-f07c-47b9-acf7-a426815a5030" />

---

### Purpose of This Verification

* Confirms least-privilege access
* Prevents unauthorized data access
* Validates correct permission configuration

---

### One-Line Summary
This step proves that **each user can access only their own workspace**, ensuring strong isolation and security.

## Outcome

* Secure user accounts created for Sarah and Mike
* Isolated workspaces with strict access controls
* Passwords enforced with expiration and warnings
* System follows **enterprise Linux security best practices**

---


## Conclusion

This task establishes a **secure and scalable user management foundation**, suitable for development environments and compliant with standard Linux security practices.

```











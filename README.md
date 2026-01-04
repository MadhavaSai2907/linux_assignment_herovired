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
yum install htop nmon
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

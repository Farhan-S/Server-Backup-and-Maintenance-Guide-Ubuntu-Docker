# 🚀 Server Backup and Maintenance Guide (Ubuntu + Docker)

This guide explains how to **backup and maintain your Ubuntu server running Docker containers**.
We’ll walk through connecting to your server, accessing Docker containers, creating compressed backups (`.tar.gz`), and securely copying them to your local machine using `scp`.

Perfect for **DevOps engineers, backend developers, and system administrators** who want to safely back up **Docker container data** from an **Ubuntu server** to a **local computer**.

---

## 🔑 Prerequisites

* An **Ubuntu server** with Docker installed.
* Access via **SSH** (password or SSH key).
* A local Linux/Mac machine (Windows users can use WSL or Git Bash).

---

## 🖥 Step 1: Connect to the Remote Server

Open your terminal and connect to the server using SSH:

```bash
ssh root@151.106.125.216
```

> Replace `root` and `151.106.125.216` with your **username** and **server IP address**.

---

## 🐳 Step 2: List Running Docker Containers

To see which Docker containers are running:

```bash
docker ps
```

You’ll get an output like:

```
CONTAINER ID   IMAGE         COMMAND   CREATED   STATUS   PORTS   NAMES
d4a4c8c2a123   my-backend    ...       ...       ...      ...     srv-captain--biddarthi-backend.1.q3qu4xbcvdb9x8h6pncmp2h7a
```

Copy the **container name or ID** (`srv-captain--biddarthi-backend.1.q3qu4xbcvdb9x8h6pncmp2h7a` in this example).

---

## 📂 Step 3: Enter the Docker Container

To open a shell inside the container:

```bash
docker exec -it <container_name_or_id> /bin/sh
```

> Replace `<container_name_or_id>` with your container’s actual name or ID.

---

## 📦 Step 4: Create a Backup (Compress Folder)

Inside the container, navigate to the folder you want to back up. Then run:

```bash
tar -czvf /tmp/myfolder.tar.gz myfolder/
```

* `tar` = archive command
* `-c` = create archive
* `-z` = compress using gzip
* `-v` = verbose (show progress)
* `-f` = filename

This creates a compressed file: `/tmp/myfolder.tar.gz`.

---

## ⬆ Step 5: Copy Backup File to Host Server

Exit the container:

```bash
exit
```

Now copy the backup file from inside the container to the server’s `/tmp` directory:

```bash
docker cp <container_name_or_id>:/tmp/myfolder.tar.gz /tmp/
```

---

## 💾 Step 6: Copy Backup from Server to Local Machine

Exit the server:

```bash
exit
```

Now download the backup to your **local machine** (e.g., into `~/Downloads/`):

```bash
scp root@151.106.125.216:/tmp/myfolder.tar.gz ~/Downloads/
```

* `scp` = secure copy
* `root@151.106.125.216` = server username and IP
* `/tmp/myfolder.tar.gz` = file on server
* `~/Downloads/` = destination on local

---

## ✅ Verification

On your local machine, go to your `~/Downloads/` folder and check:

```bash
ls ~/Downloads/
```

You should see `myfolder.tar.gz`. Extract it with:

```bash
tar -xzvf ~/Downloads/myfolder.tar.gz
```

---

## ⚡ Notes & Best Practices

* Always back up **critical server data** before major updates.
* Use **cron jobs** to automate this process.
* Store backups in **multiple locations** (local + cloud).
* Use SSH keys instead of passwords for better security.

---

## 📚 Conclusion

With this guide, you can easily **backup Docker container files from an Ubuntu server to your local machine**. It ensures safe data recovery and smooth server maintenance.


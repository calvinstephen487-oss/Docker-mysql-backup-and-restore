# Docker MySQL Backup and Restore

This project demonstrates how to back up and restore a MySQL database running in Docker using Docker volumes. It simulates real-world scenarios of database migration and disaster recovery using container volume management.

---

## 🚀 Project Overview

- Run a MySQL Docker container with a database and sample data
- Back up the Docker volume storing the database files to host
- Remove and recreate the container as needed
- Restore data into a new Docker volume and start a fresh container
- Verify data persistence and correctness after restoration

---

## 🛠️ Prerequisites

- Docker installed on your machine (Windows, macOS, Linux)
- Basic familiarity with CLI / terminal commands

---

## 📝 Main Commands and Workflow

### 1. Start the MySQL container:
`docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=my-secret-pw -e MYSQL_DATABASE=mydb -p 3306:3306 -d mysql:latest`

### 2. Connect to the container and create a sample table:

`docker exec -it mysql-container mysql -u root -p`
**enter password: my-secret-pw**

Inside MySQL shell, run:

`USE mydb;
CREATE TABLE employees (
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(100),
age INT,
email VARCHAR(100),
department VARCHAR(100)
);
INSERT INTO employees (name, age, email, department) VALUES ('calvin', 25, 'calvin@ymail.com', 'IT');

### 3. Stop the container:

`docker stop mysql-container`

### 4. Inspect the container volumes and find the database volume:

`docker inspecr mysql-container`

### 5. Backup the MySQL Data Volume
Find the volume used by the MySQL container (you can check with docker inspect mysql-container for "Mounts").
`docker run --rm -v <mysql_volume_name>:/volume -v /your/backup/path:/backup busybox cp -a /volume/. /backup/`
Replace <mysql_volume_name> with your data volume and /your/backup/path with your desired host path.

### 6. Remove the stopped container:
`docker rm mysql-container`

### 7. Restore or Move the Backup
docker volume create mysql_restored_volume
docker run --rm -v mysql_restored_volume:/volume -v <your desired host path>:/backup busybox cp -a /backup/. /volume/


### 8. Start a new container with the restored volume:

`docker run --name mysql-new -e MYSQL_ROOT_PASSWORD=my-secret-pw -p 3306:3306 -v mysql_restored_volume:/var/lib/mysql -d mysql:latest`

### 9. Verify the restored database and data:

`docker exec -it mysql-new mysql -u root -p`
USE mydb;
SHOW TABLES;
SELECT * FROM employees;


---

## 📂 Included Files

- `backup-of-sql.txt` – detailed step-by-step backup and restore commands
- `schema.sql` (optional) – SQL schema and inserts
- `screenshots/` (optional) – For proof of successful backup/restore

---

## 💡 Skills Demonstrated

- Docker volume management and data persistence
- MySQL backup and restoration techniques
- Real-world DevOps and database migration workflows




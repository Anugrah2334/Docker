# 🐬 Setting Up MySQL in a Docker Container with an Initialization Script 🚀

## 📌 Prerequisites
- ✅ Install Docker on your system.
- ✅ Ensure Docker is running.
- ✅ Create an SQL initialization script (e.g., `Anugrah_demo.sql`) with database and table definitions.

---

## 📂 Project Directory Structure
Ensure your project directory is organized as follows:

```
project-directory/
│── Dockerfile
│── Anugrah_demo.sql
```

This structure keeps all necessary files in one place for an efficient setup.

---

## 🛠 Step 1: Create a Dockerfile
Create a `Dockerfile` in your project directory:

```dockerfile
# 🏗 Use the official MySQL image
FROM mysql:latest

# 📂 Copy initialization script to the container
COPY Anugrah_demo.sql /docker-entrypoint-initdb.d/

# 🔥 Expose MySQL port
EXPOSE 3306
```

---

## 📜 Step 2: Create an SQL Initialization Script
Create a file named `Anugrah_demo.sql` in the same directory:

```sql
CREATE DATABASE Anugrah;
USE Anugrah;

CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);

INSERT INTO students (name, age) VALUES ('Alice', 22), ('Bob', 24);
```

---

## 🏗 Step 3: Build the Docker Image
Run the following command to build the Docker image:

```sh
docker build -t mysql-custom .
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Containerized%20MySQL%2C%20Agile%20%26%20Efficient/Screenshot1.png)

💡 *This command creates a custom MySQL image named `mysql-custom`.*

---

## 🚀 Step 4: Run MySQL Container
To start a MySQL container using the custom image and set the root password, execute:

```sh
docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=root -d mysql-custom
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Containerized%20MySQL%2C%20Agile%20%26%20Efficient/Screenshot2.png)

### 🧐 Explanation:
- 🏷 Creates a container named `mysql-container`.
- 🔐 Sets the root password to `root`.
- 🏃 Runs the container in detached mode (`-d`).
- 🛠 Uses the custom MySQL image `mysql-custom`.

---

## 🔍 Step 5: Access the Running Container
To enter the running container’s shell:

```sh
docker exec -it mysql-container bash
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Containerized%20MySQL%2C%20Agile%20%26%20Efficient/Screenshot3.png)

💡 *This command opens an interactive terminal session inside `mysql-container`.*

---

## 💻 Step 6: Connect to MySQL
Once inside the container, access MySQL using:

```sh
mysql -u root -p
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Containerized%20MySQL%2C%20Agile%20%26%20Efficient/Screenshot3.png)

🔑 Enter the root password (`root`) when prompted.

---

## 🏗 Step 7: Verify Database and Tables
After logging into MySQL, check the available databases:

```sql
SHOW DATABASES;
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Containerized%20MySQL%2C%20Agile%20%26%20Efficient/Screenshot4.png)

🔄 Switch to the `Anugrah` database:

```sql
USE Anugrah;
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Containerized%20MySQL%2C%20Agile%20%26%20Efficient/Screenshot5.png)

📊 Query the `students` table:

```sql
SELECT * FROM students;
```
![Example Image](https://github.com/Anugrah2334/Docker/blob/main/Containerized%20MySQL%2C%20Agile%20%26%20Efficient/Screenshot6.png)

---

## 🎉 Conclusion
🎯 You have successfully set up MySQL in a Docker container with an initialization script. This method ensures that the database is automatically initialized with predefined schemas and data when the container starts. 🚀 Happy coding! 🎨

# Docker Security Lab — DVWA Containerisation

## Overview
This lab demonstrates core containerisation skills using Docker Desktop on Windows. I deployed the **Damn Vulnerable Web Application (DVWA)** - a deliberately insecure PHP/MySQL web app used by security professionals to practice common web vulnerabilities in a safe, isolated environment.

**Skills demonstrated:** Docker image management, container lifecycle (run/stop/remove), port mapping, loopback networking and containerised web application deployment.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Environment
| Component      |        Detail                 |
|----------------|-------------------------------|
| Host OS        |       Windows 11              |
| Docker Desktop |        v4.98.0                |
| Docker Engine  | Running (WSL2 backend)        |
| Image          | `vulnerables/web-dvwa:latest` |
| Container port | 80 (internal) → 8080 (host)   |

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Steps Completed

### 1. Pull the DVWA Image
```bash
docker pull vulnerables/web-dvwa
```
Downloaded the DVWA image from Docker Hub. Docker stores this locally so containers can be spun up instantly without re-downloading.

**Evidence:** `Status: Image is up to date for vulnerables/web-dvwa:latest`

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 2. Run the Container
```bash
docker run -d -p 8080:80 vulnerables/web-dvwa
```

|     Flag     |             Meaning                      |
|--------------|------------------------------------------|
| `-d`         | Detached mode — runs in the background   |
| `-p 8080:80` | Maps host port 8080 to container port 80 |

Port 80 was used by the Windows system, so port 8080 was used instead — demonstrating understanding of port conflict resolution and port mapping.

**Evidence:** Container ID returned; `docker ps` confirmed `0.0.0.0:8080->80/tcp`

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 3. Verify Container is Running
```bash
docker ps
```
Confirmed container status: **Up**, with correct port mapping and assigned container name.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 4. Check Container Logs
```bash
docker logs <container_name>
```
Verified internal services started successfully:
- `[+] Starting mysql...` → MariaDB database server started
- `[+] Starting apache` → Apache 2.4 web server started

This confirms understanding of how to inspect a running container's internal processes without entering it.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 5. Access DVWA in Browser
Navigated to `http://127.0.0.1:8080` in Microsoft Edge.

> **Note:** `localhost` was intercepted by Edge's proxy settings. Using `127.0.0.1` (the loopback address) bypassed this - demonstrating knowledge of IP addressing and loopback networking.

DVWA login page loaded successfully.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 6. Database Setup & Login
- Navigated to `Setup / Reset DB` page
- Verified Setup Check: MySQL installed, PHP modules installed, Apache running
- Clicked **Create / Reset Database**
- Logged in: `admin` / `password`
- Accessed full DVWA dashboard with vulnerability modules: SQL Injection, XSS, Brute Force, CSRF, Command Injection, File Upload, etc.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
### 7. Stop the Container
```bash
docker stop <container_name>
```
Gracefully shut down the running container. Container status changed to **Exited**.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 8. Remove the Container
```bash
docker rm <container_name>
```
Deleted the container instance entirely. Docker Desktop confirmed **No containers are running**.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### 9. Verify Image Persists
```bash
docker images
```
Output confirmed `vulnerables/web-dvwa:latest` (935MB) still exists locally - demonstrating the key concept that **containers are temporary; images are persistent**. A new container can be spun up instantly from the same image at any time.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Key Concepts Demonstrated

|       Concept         |              Applied                      |
|-----------------------|-------------------------------------------|
| Container lifecycle   |   Pull → Run → Stop → Remove              |
| Port mapping          | `-p 8080:80` host-to-container mapping    |
| Loopback networking   | `127.0.0.1` vs `localhost` behaviour      |
| Container vs image    | Image persists after container removal    |
| Log inspection        | `docker logs` to verify internal services |
| Containerised web app | DVWA running isolated from host system    |

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
## Security Relevance
DVWA is used by security teams and SOC analysts to:
- Practise identifying and exploiting common OWASP Top 10 vulnerabilities (SQL injection, XSS, CSRF)
- Test WAF (Web Application Firewall) rules in a safe environment
- Train developers on secure coding practices

Running it in Docker ensures complete isolation from the host - the vulnerable application cannot affect the underlying system.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Repository
Part of my cybersecurity home lab portfolio: [github.com/aap-soc](https://github.com/aap-soc)

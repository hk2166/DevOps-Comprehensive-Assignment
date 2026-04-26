# DevOps Assignment: Deploying a Full-Stack Application

**Total marks: 30 &nbsp;|&nbsp; 15 automated checks &nbsp;|&nbsp; 2 marks each**

---

## Overview

You are given a full-stack web application with a React frontend, a Node.js/Express backend, and a MongoDB database. The repository contains the source code in an undeployed state. Your task is to get the application running on the EC2 instance provisioned for you, in a state that satisfies the grading criteria below.

You may use any tools or workflow you choose. There is no prescribed solution.

When performing this assignment, you will not have access to any LLM or external resources. You must rely on your own knowledge and the official documentation for any tools you use. The links for the official documentation are provided in the course materials and in this document.

**Repository:** `https://github.com/nst-sdc/DevOps-Comprehensive-Assignment`

---

## The Application

| Layer | Technology | Notes |
| --- | --- | --- |
| Frontend | React 19, Vite | Should be compiled to a static bundle |
| Backend | Node.js, Express | REST API |
| Database | MongoDB | Stores users |

**API endpoints:**

- `GET /api/users` — returns all users as a JSON array
- `POST /api/users` — creates a user; body: `{ "name": "...", "email": "..." }`

---

## Your EC2 Instance

You will receive SSH credentials to an Ubuntu EC2 instance. All work must be performed on that machine. The grader will log into the same machine and run the grading script.

---

## Grading

Grading is mostly automated (with some manual checks noted below). The grader runs a script that performs 15 independent checks on the deployed application. Each check is worth **2 marks**. There is no partial credit — a check either passes or fails.

**10 checks are disclosed below. 5 checks are hidden** — they will not be described in advance, but a well-structured deployment will pass them naturally.

---

## Disclosed Checks (01–10)

### Check 01 — Docker is installed

`docker` must be present and executable on the machine.

```sh
docker --version
# must succeed
```

### Check 02 — Docker daemon is active

The Docker daemon must be running and accepting commands.

```sh
docker info
# must succeed without error
```

### Check 03 — System packages are up to date

All available system package upgrades must have been applied.
The grader checks for 0 pending upgrades at time of grading.

### Check 04 — Exactly 2 containers are running

`docker ps` must show exactly 2 running containers — one for the application and one for the database.

### Check 05 — MongoDB container is running

One of the two running containers must be using a MongoDB image.

### Check 06 — App container is bound to host's default HTTP port

The application container must map host's default HTTP port to its internal port.

### Check 07 — GET `http://localhost` returns HTTP 200

The application must be accessible at `http://localhost` on the EC2 instance, and at the IP address of the EC2 instance from a remote machine.

### Check 08 — Frontend HTML is served at /

The response body at `http://localhost` must be the compiled React application. The grader checks for the presence of the root mount point in the HTML.

A development server (`npm run dev`) will not satisfy this check — the frontend must be built first.

### Check 09 — GET /api/users returns HTTP 200 with Content-Type: application/json

The response to GET /api/users must have the correct Content-Type header indicating JSON and a 200 status code.

### Check 10 — Database persists across container restarts

The MongoDB should be configured to store its data in such a way that if the container is stopped and removed, the data remains intact and can be accessed when a new container is restarted.

---

## Hints

- The frontend source contains a hardcoded API URL (`http://127.0.0.1:5000/api`) that will not work once deployed — find it and fix it.
- The backend currently only serves the API endpoints. You might want to look into how to make it serve the compiled frontend as well.
- You might need to add environment variables to your containers to configure the application correctly. Local testing with `.env` files can help with this.
- Think carefully about how your containers find each other at runtime. Container names matter.
- Read the Docker documentation on Dockerfile, images, containers, named volumes, networks and bind mounts. You will likely need to use all of these concepts.

---

## Documentation References

1. [Docker](https://docs.docker.com/)
2. [Docker CLI reference](https://docs.docker.com/engine/reference/commandline/cli/)
3. [Docker Compose reference](https://docs.docker.com/compose/compose)
4. [MongoDB Docker image](https://hub.docker.com/_/mongo)
5. [Node.js Docker image](https://hub.docker.com/_/node)
6. [Express documentation](https://expressjs.com/)
7. [React documentation](https://reactjs.org/docs/getting-started.html)
8. [Vite documentation](https://vitejs.dev/guide/)
9. [MDN Express tutorial](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs)
10. [SSH documentation](https://www.ssh.com/ssh/)
11. [Ubuntu package management](https://ubuntu.com/server/docs/package-management)
12. [Systemd documentation](https://www.freedesktop.org/wiki/Software/systemd/)
13. [Nginx documentation](https://nginx.org/en/docs/)
14. [PM2 documentation](https://pm2.keymetrics.io/docs/usage/quick-start/)
15. [Node.js documentation](https://nodejs.org/en/docs/)



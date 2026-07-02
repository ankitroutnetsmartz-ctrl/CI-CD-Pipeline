## About This Project

This repository contains a production-grade, highly automated CI/CD pipeline designed for rapid, low-overhead deployments of web applications. By eliminating complex Node.js/Next.js dependencies and focusing on containerized immutability, the infrastructure maintains lightweight overhead with 100% configuration reliability.

### 🏗️ Technical Architecture & Workflow
1. **Source Control:** Developers push verified code modifications directly to the `main` branch.
2. **Automated Orchestration:** A localized GitHub Actions self-hosted runner intercepts the push event and runs the deployment workflow without passing external artifacts across public servers.
3. **Fail-Safe Backups:** Prior to synchronization, the runner SSHs into the target host (`vm01`) to generate an archival `.tar.gz` backup of the active directory, automatically keeping a rolling index of only the last 3 versions to preserve disk storage.
4. **Smart Sync:** Code differences are incrementally pushed to the system folder path `/var/www/html` via secure, root-level `rsync`.
5. **Enforced Security Bounds:** Permissions are tightly audited dynamically (`755` for directories, `644` for files, and explicit user group ownership assignment).
6. **Container Rebuild:** Docker Compose forces a zero-downtime, no-cache container image rebuild out of the local directory context, safely evicting stale instances and binding directly to host port `80`.

### 🛠️ Tech Stack
* **Orchestration:** GitHub Actions (Self-Hosted Runner execution logic)
* **Containerization:** Docker, Docker Compose
* **Web Server Layer:** Nginx (Alpine-optimized base image)
* **Target OS Environment:** Ubuntu Linux (LTS Workstations)
* **Security & Network:** Secure Shell (SSH via PEM asymmetric pairs), Rsync, Automated File System Permission Masking

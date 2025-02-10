# Keycloak

This repository contains the necessary files to deploy a Keycloak instance using Ansible and GitHub Actions. The deployment process includes setting up Docker, configuring Keycloak, and ensuring the necessary Docker networks and volumes are in place.

## Directory Structure

- `.github/workflows/`
  - `main.yaml`: GitHub Actions workflow file for deploying Keycloak.
- `ansible/playbooks/`
  - `deploy-keycloak.yml`: Ansible playbook for deploying Keycloak.
- `docker/`
  - `docker-compose.yml`: Docker Compose file for setting up Keycloak and its dependencies.

## Deployment Process

### GitHub Actions Workflow

The GitHub Actions workflow (`.github/workflows/main.yaml`) is triggered on a push to the `main` branch. It performs the following steps:

1. **Checkout Repository**: Checks out the repository to the GitHub runner.
2. **Install gettext**: Installs `gettext` for environment variable substitution.
3. **Copy Project to Dropfolder**: Copies the Docker project files to the Ansible dropfolder.
4. **Run Ansible Playbook**: Executes the Ansible playbook to deploy Keycloak.

### Ansible Playbook

The Ansible playbook (`ansible/playbooks/deploy-keycloak.yml`) performs the following tasks:

1. **Publish Keycloak Project**: Checks if the project directory exists, creates a backup, and copies the project files to the remote nodes.
2. **Clean and Publish Keycloak Project**: Ensures the destination directory exists, cleans it, and copies the project files.
3. **Install Docker**: Installs Docker on the remote nodes if it is not already installed.
4. **Deploy Keycloak**: Ensures the necessary Docker networks and volumes are in place, generates the `.env` file, and starts the Docker Compose services.
5. **Release Keycloak**: Ensures Cloudflared tunnels are configured.

### Docker Compose

The Docker Compose file (`docker/docker-compose.yml`) defines the services for Keycloak and its PostgreSQL database. It includes common properties for the services, such as restart policies, DNS settings, environment variables, volumes, and networks.

## Environment Variables

The following environment variables are used in the deployment process:

- `ANSIBLE_PROJECT`: Path to the Ansible project.
- `KEYCLOAK_PROJECT`: Path to the Keycloak project.
- `CF_ACCOUNT_ID`: Cloudflare account ID.
- `CF_TUNNEL_API_TOKEN`: Cloudflare tunnel API token.
- `CF_TUNNEL_NAME`: Cloudflare tunnel name.
- `CF_ZONE_ID`: Cloudflare zone ID.
- `CF_SERVICE`: Cloudflare service.
- `CF_HOSTNAME`: Cloudflare hostname.
- `KEYCLOAK_USER`: Keycloak admin username.
- `KEYCLOAK_PASSWORD`: Keycloak admin password.
- `KEYCLOAK_DATABASE_VENDOR`: Database vendor for Keycloak.
- `KEYCLOAK_DATABASE_NAME`: Database name for Keycloak.
- `KEYCLOAK_DATABASE_USER`: Database user for Keycloak.
- `KEYCLOAK_DATABASE_PASSWORD`: Database password for Keycloak.
- `HOST_PORT`: Host port for Keycloak.

## Prerequisites

- A self-hosted GitHub runner with Docker installed.
- Ansible installed on the self-hosted runner.
- Cloudflare account and API token for setting up tunnels.

## Usage

1. Clone the repository to your local machine.
2. Configure the necessary environment variables in the GitHub Actions secrets and variables.
3. Push your changes to the `main` branch to trigger the deployment workflow.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

# Wordpress Docker Project
## Table of Description
[1- Description](#Description)

[2- Requirements](#Requirements)

[3- Quickstart](#Quickstart)

[4- Usage](#Usage)

[5- Configuration](#Configuration)

[6- Persitence](#Persitence)

[7- Security](#Security)

[8-Troubleshooting](#Troubleshooting)

## 1- Description
This project provides a WordPress installation running with Docker Compose.
The setup consists of two services:
* `WordPress`: The web application: (`https://hub.docker.com/_/wordpress`)
* `MariaDB`: The database used by WordPress:(`https://hub.docker.com/_/mariadb`)
For more informations:
Docker Compose creates a shared network between both services and a persistent volume for the MariaDB database.

The purpose of this repository is to provide a simple, reproducible, and persistent WordPress environment that can be started and managed with Docker Compose.

## 2- Requirements
The following software is required:
* Docker
* Docker Compose: (``https://docs.docker.com/compose``)
* A web browser(https://wordpress.org/documentation/): `https://wordpress.org/documentation/`
Docker Desktop can be used on Windows, macOS, and Linux systems with Docker support.

## 3- Quickstart
- Clone the repository.
- Create a `.env` file based on `.env.example`.
- Configure the required database variables.
- Start the services using:
```bash
docker compose up -d
```
- Check the running containers:
```bash
docker compose ps
```
- Open WordPress in a web browser:
`http://localhost:8080`
- Complete the WordPress installation in the browser.
To stop the setup:
```bash
docker compose down
```
To start it again:
```bash
docker compose up -d
```
## 4- Usage
### Starting the application
Start the WordPress and database services in detached mode:
```bash
docker compose up -d
```
The `-d` option runs the containers in the background.
### Checking the services
Use:
```bash
docker compose ps
```
to check whether the WordPress and database containers are running.
### Stopping the application
Use
```bash
docker compose down
```
This removes the running containers and the Compose network but keeps the database volume.
### Restarting the application
To start the services again after stopping them:
```bash
docker compose up -d
```

## 5- Configration
The project uses environment variables for configuration.
Sensitive values such as database passwords must not be stored directly in the Git repository.
Create a `.env` file in the project root and configure the required variables:

`MYSQL_DATABASE`=wordpress
`MYSQL_USER`=wordpress
`MYSQL_PASSWORD`=CHANGE_ME
`MYSQL_ROOT_PASSWORD`=CHANGE_ME
The `.env` file is excluded from Git by `.gitignore`.

The `.env.example` file provides a template without real credentials.
Changing the WordPress port

The default configuration exposes WordPress on `port 8080`:

`ports`:
  - "`8080:80`"

The first port is the host port and the second port is the container port.
For example, changing it to:
`ports`:
  - "`9090:80`"
would make WordPress available at:
`http://localhost:9090`

## 6- Persistence
The MariaDB service uses a named Docker volume:

`volumes`:
  - `db_data:/var/lib/mysql`

The volume stores the database data outside the database container.
Therefore, stopping and recreating the containers does not remove the stored WordPress database.
For example:
```bash
docker compose down
```
```bash
docker compose up -d
```
does not remove the db_data volume.
The database volume must not be removed when persistent data is required.
## 7- Security

The following security principles are used in this project:
* Passwords are provided through environment variables.
* The .env file is excluded from Git.
* No SSH keys are stored in the repository.
* No passwords or tokens are stored directly in the Docker Compose configuration.
* No IP addresses or other sensitive infrastructure information are stored in the repository.
* `.env.example` contains placeholders instead of real credentials.
Never commit the `.env` file or other files containing credentials to the repository.
## 8- Troubeshooting
### WordPress is not reachable
Check whether the containers are running:

```bash
docker compose ps
```
Check the WordPress container logs:

```bash
docker compose logs wordpress
```
Check the database logs:
```bash
docker compose logs db
```
### WordPress cannot connect to the database

Verify the database variables in `.env`:

`MYSQL_DATABASE=wordpress`
`MYSQL_USER=wordpress`
`MYSQL_PASSWORD=CHANGE_ME`
`MYSQL_ROOT_PASSWORD=CHANGE_ME`
Also verify that both services are running:
```bash
docker compose ps
```
### Resetting the complete environment
Removing the database volume deletes the persistent database data.
This command should therefore only be used when a complete reset is intended:
```bash
docker compose down -v
```
Afterwards, the environment can be recreated with:
```bash
docker compose up -d
```

## 9- File descriptions

* `.env`: Local environment configuration containing sensitive values. Not committed to Git.
* `.env.example`: Example environment configuration without real credentials.
* `.gitignore`: Prevents sensitive and irrelevant files from being committed.
* `docker-compose.yaml`: Defines and configures the WordPress and MariaDB services.
* `README.md`: Project documentation and usage instructions.


# Laravel Sail with Xdebug

![Laravel](https://img.shields.io/badge/Laravel-12.x-FF2D20?logo=laravel&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-8.3%2B-777BB4?logo=php&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Required-2496ED?logo=docker&logoColor=white)
![Sail](https://img.shields.io/badge/Laravel-Sail-FF2D20?logo=laravel&logoColor=white)
![Xdebug](https://img.shields.io/badge/Xdebug-Enabled-8A2BE2)
![License](https://img.shields.io/badge/License-Private-lightgrey)

A standardized **Laravel 12** base project prepared for local development with **Laravel Sail** and a fully functional **Xdebug** environment.

---

## Table of Contents

- [Project Purpose](#project-purpose)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Initial Installation](#initial-installation)
- [Environment Configuration](#environment-configuration)
- [Sail Alias Configuration](#sail-alias-configuration)
- [Basic Sail Commands](#basic-sail-commands)
- [Frontend Commands](#frontend-commands)
- [Xdebug](#xdebug)
- [Daily Development Flow](#daily-development-flow)
- [Rebuild Procedure](#rebuild-procedure)
- [Troubleshooting](#troubleshooting)
- [Project Notes](#project-notes)
- [License](#license)

---

## Project Purpose

This repository provides a reusable Laravel 12 base structure for development teams that require:

- a standardized local environment
- Docker-based execution
- Laravel Sail integration
- Xdebug already configured and operational
- a predictable setup process for all developers

This project is intended to reduce environment setup time and ensure consistency across development machines.

---

## Technology Stack

The project is based on the following technologies:

- **Laravel 12**
- **PHP**
- **Laravel Sail**
- **Docker**
- **Docker Compose support**
- **Node.js**
- **NPM**
- **Xdebug**

---

## Prerequisites

The following software must be installed on the host machine before running this project.

### Docker

Docker is required to run the project services inside containers.

### Docker Compose support

Docker Compose support is required because Laravel Sail orchestrates the project containers through Docker.

### Git

Git is required to clone the repository and manage version control.

### Composer

Composer is required to install the PHP dependencies declared in the project.

### Node.js

Node.js is required to install and compile frontend assets.

### NPM

NPM is required to manage frontend packages and run frontend build commands.

---

## Initial Installation

Follow the steps below in the exact order shown.

### 1. Clone the repository

```bash
git clone REPOSITORY_URL
```

This command downloads the repository from the remote source to your local machine.

### 2. Enter the project directory

```bash
cd PROJECT_FOLDER
```

This command moves the terminal into the root directory of the project.

### 3. Install PHP dependencies

```bash
composer install
```

This command installs all PHP dependencies listed in `composer.json`.

### 4. Create the environment file

```bash
cp .env.example .env
```

This command creates the application environment file based on Laravel's default example configuration.

### 5. Start the Sail containers

```bash
./vendor/bin/sail up -d
```

This command starts all required Docker containers in detached mode.

It performs the following actions:

- initializes the Sail environment
- starts the defined services
- runs the application stack in the background

### 6. Generate the Laravel application key

```bash
./vendor/bin/sail artisan key:generate
```

This command generates the application key and writes it into the `.env` file.

The application key is required for:

- session encryption
- cookie encryption
- framework-level security features

### 7. Run the database migrations

```bash
./vendor/bin/sail artisan migrate
```

This command creates the database tables required by the application.

### 8. Install frontend dependencies

```bash
./vendor/bin/sail npm install
```

This command installs all JavaScript dependencies declared in `package.json`.

### 9. Start the frontend development server

```bash
./vendor/bin/sail npm run dev
```

This command starts the frontend development server and watches for asset changes.

---

## Environment Configuration

The main environment file used by the application is:

```bash
.env
```

This file stores the project configuration, including:

- application name
- application URL
- database connection
- cache and session settings
- queue configuration
- debug-related variables

This project already includes a working debug environment. Standard usage does not require additional Xdebug setup.

---

## Sail Alias Configuration

To simplify command execution, create a shell alias for Sail.

### Linux, macOS, or WSL

Open your shell configuration file:

```bash
nano ~/.bashrc
```

Add the following line at the end of the file:

```bash
alias sail='sh $([ -f sail ] && echo sail || echo vendor/bin/sail)'
```

Reload the shell configuration:

```bash
source ~/.bashrc
```

After this configuration, the command below:

```bash
sail up -d
```

executes the same action as:

```bash
./vendor/bin/sail up -d
```

---

## Basic Sail Commands

### Start the environment

```bash
sail up -d
```

Starts the project containers in the background.

### Stop the environment

```bash
sail down
```

Stops and removes the project containers.

### Open a shell inside the application container

```bash
sail shell
```

Opens an interactive shell session inside the main application container.

### View container logs

```bash
sail logs
```

Displays the logs generated by the project containers.

### View logs in real time

```bash
sail logs -f
```

Streams container logs continuously in real time.

### Run Artisan commands

```bash
sail artisan COMMAND
```

Runs a Laravel Artisan command inside the application container.

Example:

```bash
sail artisan migrate
```

Runs the database migrations inside the container.

---

## Frontend Commands

### Install frontend dependencies

```bash
sail npm install
```

Installs JavaScript dependencies inside the container.

### Start the frontend development server

```bash
sail npm run dev
```

Starts the asset build process for development.

### Build frontend assets for production

```bash
sail npm run build
```

Compiles and optimizes frontend assets for production usage.

---

## Xdebug

Xdebug is already configured and functional in this project.

No additional setup is required for standard development usage.

This means the environment already supports debugging workflows for local development and issue investigation.

---

## Daily Development Flow

Use the following commands during normal daily development.

### Start the environment

```bash
sail up -d
```

Starts the application containers.

### Start the frontend development server

```bash
sail npm run dev
```

Starts asset compilation in development mode.

### Execute backend commands when necessary

```bash
sail artisan migrate
```

Runs database migrations when schema updates are required.

### Stop the environment after work is finished

```bash
sail down
```

Stops the application containers.

---

## Rebuild Procedure

Use the commands below when the environment requires a clean rebuild.

```bash
sail down -v
sail build --no-cache
sail up -d
```

These commands perform the following actions:

### `sail down -v`

Stops the containers and removes their associated volumes.

### `sail build --no-cache`

Rebuilds the Docker images from scratch without using cached layers.

### `sail up -d`

Starts the newly rebuilt containers in detached mode.

This procedure must be executed when:

- Docker definitions are changed
- image-level dependencies are changed
- the environment becomes inconsistent
- a clean rebuild is required for validation

---

## Troubleshooting

### Docker is not running

If Docker is not running, Sail commands will fail.

Ensure Docker Desktop is open and fully initialized before running any Sail command.

### Containers do not start

If the containers do not start correctly, run:

```bash
sail logs
```

This command displays the container logs and helps identify the startup error.

### Frontend assets are not updating

If frontend changes are not reflected, ensure the development server is running:

```bash
sail npm run dev
```

### Environment changes are not being applied

If configuration changes are not applied, clear Laravel caches:

```bash
sail artisan optimize:clear
```

### The environment is corrupted or inconsistent

If the environment behaves unexpectedly, execute a full rebuild:

```bash
sail down -v
sail build --no-cache
sail up -d
```

---

## Project Notes

- This project is intended to be executed through Laravel Sail.
- Docker must be running before any Sail command is executed.
- Xdebug is already configured and operational.
- The setup process must be followed in the documented order.
- A rebuild is required whenever container definitions or image-level dependencies change.

---

## License

This project is private and intended for internal use unless otherwise specified.

# NocoBase Docker Compose Installation

This project runs **NocoBase** with **PostgreSQL** using Docker Compose. The assignment only requires getting NocoBase running.

## Requirements

Install Docker Desktop before starting. Docker Desktop includes Docker Engine and Docker Compose.

You should be able to run the commands below from this project directory.

## Docker Compose basics

I used the official Docker Compose Quickstart to review the basics:


    - A Compose file defines the services that make up an application.
    - `docker compose up` creates and starts the services.
    - `docker compose down` stops and removes the containers/network created by Compose.
    - Ports map a port on the computer to a port inside a container.
    - Volumes keep application/database data outside the container's temporary writable layer.


Tutorial: https://docs.docker.com/compose/gettingstarted/

## How to run NocoBase

### 1. Open a terminal

Open PowerShell, Command Prompt, Terminal, or another shell.

### 2. Change to the project directory

For example:


    1. cd C:\
    2. cd DockerContains
    3. cd nocobase1


### 3. Start the system

Run: docker compose up


The first startup can take a few minutes because Docker needs to download the NocoBase and PostgreSQL images and NocoBase needs to initialize its database.

Leave this terminal running. The assignment specifically requires that the system work with `docker compose up`.


### 4. Open NocoBase

Open a web browser and go to: http://localhost:13000

NocoBase should display its login page.

## NocoBase login

Use these default credentials on the initial installation:


    1. Email/Username: `admin@nocobase.com`
    2. Password: `admin123`


Remember to change the default password after your first login if this installation will be used beyond the class/project environment.

## Useful Docker Compose commands

### Start


docker compose up

### Stop and remove the containers


docker compose down

The PostgreSQL data is stored in the local `storage/` directory, so `docker compose down` does not delete that project data.

## Sources


    - Docker Compose Quickstart: https://docs.docker.com/compose/gettingstarted/
    - Chapter 1: Getting Started — Build a Working System in 5 Minutes: https://docs.nocobase.com/tutorials/v2/01-getting-started

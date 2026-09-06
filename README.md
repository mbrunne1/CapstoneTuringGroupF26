
# NocoBase Docker Compose Installation

This project runs **NocoBase** with **PostgreSQL** using Docker Compose.

## Requirements

Install Docker Desktop before starting. Docker Desktop includes Docker Engine and Docker Compose.

## Docker Compose basics

I used the official Docker Compose Quickstart to review the basics:


    - A Compose file defines the services that make up an application.
    - `docker compose up` creates and starts the services.
    - `docker compose down` stops and removes the containers/network created by Compose.
    - Ports map a port on the computer to a port inside a container.
    - Volumes keep application/database data outside the container's temporary writable layer.


Tutorial: https://docs.docker.com/compose/gettingstarted/

## How to run NocoBase

You should be able to run the commands below from this project directory.


### 1. Open a terminal

Open PowerShell, Command Prompt, Terminal, or another shell.

### 2. Change to a new directory

Inside this directory, do: 
    git clone https://github.com/mbrunne1/CapstoneTuringGroupF26.git

    git checkout Sprint2DevelopmentJasonMittelstedt

    git pull

Now you have all the code from github for an empty nocobase app with Postgres.

For the second assignment, you must unzip the submited file to get the storage directory.

The storage directory has the app and the db information.


### 3. Start the system

Make sure DockerDesktop is running.

From the directory that has the file docker-compose.yml, start the app with the following command. 

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


## Inspect the new data sources 

    1. From the Gear Icon in the top right corner choose the data source dropdown 
    2. On the far right click Configure
    3. Look at the new collections Venue, Door, and Click_Event
    4. Click on configure field to see the fields. 

## Inspect the table Views 

    1. The first view is the Venue collection
    2. The second view is the Door collection
    3. The thrid view is the Click_Event collection
    

## Useful Docker Compose commands

### Start


docker compose up

### Stop and remove the containers


docker compose down

The PostgreSQL data is stored in the local `storage/` directory, so `docker compose down` does not delete that project data.

## Sources


    - Docker Compose Quickstart: https://docs.docker.com/compose/gettingstarted/
    - Chapter 1: Getting Started — Build a Working System in 5 Minutes: https://docs.nocobase.com/tutorials/v2/01-getting-started

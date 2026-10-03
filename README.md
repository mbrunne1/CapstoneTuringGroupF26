
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

## Inspect the Collection tables

    1. Select the Collection Editor Menu Item in the top left with a gear icon
    2. Select Clicker Venues on the left hand side to see the venue data. 
    3. Select Clicker Doors to see the door data.
    4. Select Clicker Click_Event to see the click_event data.

## Inspect the Entryways form

    1. At the top is a dropdown select containing each of the previously created doors. 
    2. Beside is another dropdown for the attendant to choose the direction of the entry.
    3. Below the entry form is a table containing a list of available doors and their corresponding Venues.
    4. The bottom lists the current Venues and their occupants. Actions done in the entry form will affect its data.   

## Inspect the Manager View
    1. Each of the tables track changes and actions done to the collections from other pages. 
    2. No editing can be done from this page.

## Useful Docker Compose commands

### Start


docker compose up

### Stop and remove the containers


docker compose down

The PostgreSQL data is stored in the local `storage/` directory, so `docker compose down` does not delete that project data.

## Sources


    - Docker Compose Quickstart: https://docs.docker.com/compose/gettingstarted/
    - Getting Started: https://docs.nocobase.com/tutorials/v2/01-getting-started


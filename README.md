# NocoBase Docker Compose Installation

This project runs **NocoBase** with **PostgreSQL** using Docker Compose.

## Requirements

Install Docker Desktop before starting. Docker Desktop includes Docker Engine and Docker Compose.

## Docker Compose basics

I used the official Docker Compose Quickstart to review the basics:



&#x20;   - A Compose file defines the services that make up an application.
- `docker compose up` creates and starts the services.
- `docker compose down` stops and removes the containers/network created by Compose.
- Ports map a port on the computer to a port inside a container.
- Volumes keep application/database data outside the container's temporary writable layer.





Tutorial: https://docs.docker.com/compose/gettingstarted/

## How to run NocoBase

You should be able to run the commands below from this project directory.



### 1\. Download required files

Open the "Turing Group" teams page in Teams.

Go to the "Shared" tab and go into the most recent sprint folder (As of right now it is Sprint2).

Download the Sprint2jasonMittelstedt.zip file and extract it to a folder that you can access.

### 2\. Open the terminal

Inside this folder, click on the address bar (it should look something like "Users->Desktop->Sprint2jasonMittelstedt.zip" and type "cmd".

It should open a terminal and you need to verify that the directory location is within the said folder.

### 3\. Start the system

Make sure DockerDesktop is running.

From the directory that has the file docker-compose.yml, start the app with the following command.

Run: "docker compose up"



The first startup can take a few minutes because Docker needs to download the NocoBase and PostgreSQL images and NocoBase needs to initialize its database.

Leave this terminal running. The assignment specifically requires that the system work with `docker compose up`.



### 4\. Open NocoBase

Open a web browser and go to: http://localhost:13000

NocoBase should display its login page.

## NocoBase login

Use these default credentials on the initial installation:



&#x20;   1. Email/Username: `admin@nocobase.com`
2. Password: `admin123`





## Inspect the new data sources

&#x20;   1. From the Gear Icon in the top right corner choose the data source dropdown
2. On the far right click Configure
3. Look at the new collections Venue, Door, and Click\_Event
4. Click on configure field to see the fields.



## Inspect the table Views

&#x20;   1. Select the Clicker Menu Item in the top left
2. Select Clicker Venues to see the venue data.
3. Select Clicker Doors to see the door data.
4. Select Clicker Click\_Event to see the click\_event data.



## Cleaning up

Close the page as normal in your browser, and make sure to run "docker compose down" in the cmd from earlier before closing it.





## Useful Docker Compose commands

### Start



docker compose up

### Stop and remove the containers



docker compose down

The PostgreSQL data is stored in the local `storage/` directory, so `docker compose down` does not delete that project data.

## Sources



&#x20;   - Docker Compose Quickstart: https://docs.docker.com/compose/gettingstarted/
- Getting Started: https://docs.nocobase.com/tutorials/v2/01-getting-started


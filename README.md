### Sprint 5 - See section on Proxy Server Configuration



# NocoBase Docker Compose Installation

This project runs **NocoBase** with **PostgreSQL** using Docker Compose.

## Requirements

Install Docker Desktop before starting. Docker Desktop includes Docker Engine and Docker Compose.

## Docker Compose basics

I used the official Docker Compose Quickstart to review the basics:



&#x20;   - A Compose file defines the services that make up an application.

* `docker compose up` creates and starts the services.
* `docker compose down` stops and removes the containers/network created by Compose.
* Ports map a port on the computer to a port inside a container.
* Volumes keep application/database data outside the container's temporary writable layer.





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



## Inspect the Entryway and Exitway pages

&#x20;   1. Select Entryway in the top menu.
2. Pick a Venue first, then pick one of that venue's Doors. The door list filters itself down to just that venue's doors once a venue is chosen.
3. Direction defaults to "in" on Entryway and "out" on Exitway, so mostly you just pick venue, door, and submit.
4. Submitting fires a workflow that looks up the door, then its venue, and adds or subtracts 1 from that venue's `Current\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\_Amount`. A "saved successfully" message confirms the submit, and the venue table on the page refreshes to show the updated count.
5. Exitway works the same way, just subtracting instead of adding.

## Inspect the workflow

&#x20;   1. Select Workflow from the left menu.
2. Open "Update venue count on click."
3. The trigger fires on new Click\_Event records only. From there it queries the Door, then the Venue, checks the Direction field, and runs a Calculation plus Update record on whichever branch (in/out) matches.
4. There's an Execution history tab on the workflow list for checking past runs and what each node did on a given run.

## Inspect the Manager pages

&#x20;   1. Select Manager in the top menu.
2. It's set up as a group, so Venues, Door, and Click Events show as tabs on the left instead of across the top.
3. These are read-only views for keeping an eye on current occupancy, doors, and recent click activity.
4. Venue also has a Percentage Full column, calculated from `Current\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\_Amount` divided by `Max\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\_People`.



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

* Getting Started: https://docs.nocobase.com/tutorials/v2/01-getting-started



 

## Proxy Server Configuration

Nocobase no longer runs on port 13000,   The docker.yml file has been updated to give the port assignments to Caddy.  Caddy responds to localhost:8080.


Caddy allows access to the entire Nocobase application for IP addresses specified in an allowed list inside the Caddyfile.  Unapproved IP addresses like the world wide web are only allowed access to certain parts of the application.


##### Simplest Caddyfile configuration


```caddyfile
{
	auto_https off
}

:8080 {
	reverse_proxy app:80
}
```

The simple Caddyfile does not protect access to the administrative URLs.

The Nocobase application has fixed UIDs that can be controlled.  The approach used in the Caddyfile allows special 
IP addresses to access the whole application.  Outside IP addresses that are not approved will only have access to 
the Clicker App tabs and the data manager tabs.

To test this on a local machine, the Caddyfile in the project directory needs to be edited.  

To test how the application behaves for the outside world, select the rule below and comment out the other @outside.

@outside not client_ip 111.11.1.1   # use this line to test the outside world


To test how the application behaves with a VPN or intranet, select the rule below and comment out the other @outside.
You will have to find your local client_ip.   This can be done by looking in the docker log file when Caddy is running.
The Caddyfile uses the "log" statement and you can search for "client_ip".

@outside not client_ip 111.11.1.1   # use this line to test the outside world


```caddyfile
# Nocobase site http://localhost:8080

{
	auto_https off
}

:8080 {
	log

	#@outside not client_ip 172.18.0.1  # use this line to test access behind a vpn or intranet
	@outside not client_ip 111.11.1.1   # use this line to test the outside world

	handle @outside {
                # Allow-list: from outside the intranet/VPN only what is listed here is allowed; 
                # everything else is refused.
                # A trailing * matches anything after it (an id, a tab, a file name).

         
                @allowed {

        		# The following rules were generated from Gemini searches and Google Chrome Debugger network tab inspection.

			# Pages: sign-in, Entryway, Exitway and the Manager pages (Manager, Venues, Doors, Click Events)
			path / /signin /admin
			path /admin/idhm8x63sz0* /admin/gfcj4qrm7mf*
			path /admin/ykwv3l7yzv5* /admin/wpjgq6ittgi* /admin/qdqd79fuosp* /admin/gigc1l2908l*

			# All scripts and styles, plus static assets, icons and the live-update connection (no data)
			path *.js *.css
			path /assets/* /static/* /favicon.ico /favicon/* /ws
			path /storage/uploads/* /files/*

			# Signing in and loading the app
			path /api/auth:* /api/authenticators:public*
        		path  /api/aiSettings:public*
			path /api/app:get* /api/pm:listEnabled /api/systemSettings:get /api/themeConfig:list
			path /api/roles:check /api/desktopRoutes:listAccessible /api/collections:listMeta 
        		path /api/dataSources:listEnabled
			path /api/environmentVariables /api/asyncTasks:list /api/myInAppMessages:count 
        		path /api/userWorkflowTasks:listMine
			path /api/aiEmployees:listByUser /api/aiConversations:unreadCounts 
        		path /api/blockTemplates:list /api/uiSchemaTemplates:list

			# Page layouts (read only)
			path /api/uiSchemas:get* /api/flowModels:find*

			# Data for Entryway, Exitway and the Manager pages
			path /api/venue:list /api/venue:get /api/door:list /api/door:get
			path /api/click_event:create /api/click_event:list /api/click_event:get

		}
                handle @allowed {
                        reverse_proxy app:80
                }

                # Everything else is refused
                handle {
                        respond "Only available from the intranet or VPN: {path}" 403
                }
        }

	# If it's not outside, it's inside and gets all the traffic
	handle {
		reverse_proxy app:80
	}
```


##### Useful Commands

###### Reload Caddyfile without restarting Caddy.
docker compose exec -w /etc/caddy caddy caddy reload

###### Completely restart Caddy
docker compose restart caddy

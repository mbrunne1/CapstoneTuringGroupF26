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

Go to the "Shared" tab and go into the most recent sprint folder (As of right now it is Sprint4).

Download the Sprint4matthewbrunner.zip file and extract it to a folder that you can access.

### 2\. Open the terminal

Inside this folder, click on the address bar (it should look something like "Users->Desktop->Sprint4matthewbrunner.zip" and type "cmd".

It should open a terminal and you need to verify that the directory location is within the said folder.

### 3\. Start the system

Make sure DockerDesktop is running.

From the directory that has the file docker-compose.yml, start the app with the following command.

Run: "docker compose up"



The first startup can take a few minutes because Docker needs to download the NocoBase and PostgreSQL images and NocoBase needs to initialize its database.

Leave this terminal running. The assignment specifically requires that the system work with `docker compose up`.



### 4\. Open NocoBase

Open a web browser and go to: http://localhost:8080

NocoBase should display its login page. (Port 13000 is no longer open. All traffic now goes through the Nginx proxy, see "Proxy server" below. To change the configuration, use http://localhost:8081 instead.)

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
4. Submitting fires a workflow that looks up the door, then its venue, and adds or subtracts 1 from that venue's `Current_Amount`. A "saved successfully" message confirms the submit, and the venue table on the page refreshes to show the updated count.
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
4. Venue also has a Percentage Full column, calculated from `Current_Amount` divided by `Max_People`.



## Testing using Selenium



Open cmd as in the previous steps, in the project folder (the one containing `compose.yml`), with Docker Desktop running. Then run:



&#x20;   docker compose --profile test run --rm tests



The first run downloads the Selenium and Python images, so it takes a few minutes. A passing run ends with `6 passed`.



The six tests sign in, open the Entryway page, record an entry, record an exit, submit an empty form, and open the Manager page. Tests 3 and 4 use the venue "UWG Coliseum" and add one Click\_Event row each run.



If you desire to watch the tests run, or to see why one fails instead of passes, open http://localhost:7900 in a browser before starting the command and connect using the password "secret".



If a run is stopped early with Ctrl+C, restart the test browser before the next run:



&#x20;   docker compose --profile test restart selenium



When finished, shut the test containers down:



&#x20;   docker compose --profile test down

## Proxy server: Nginx (Sprint 5)

NocoBase no longer publishes a port of its own. Every request goes through an Nginx reverse proxy (the `proxy` service in `compose.yml`), which has two doors:

| Door | Address | Who can reach it | What it allows |
| --- | --- | --- | --- |
| Public | http://localhost:8080 | Anyone who can reach the host | Sign in, Entryway/Exitway, Manager pages. Routes that change the configuration get `403 Forbidden`. |
| Intranet | http://localhost:8081 | Only the host computer itself (published on `127.0.0.1`), or through the VPN | Everything, including admin and configuration pages. |

Routes blocked on the public door: any write (POST/PUT/PATCH/DELETE) to the NocoBase configuration API (page layout, collections and fields, data sources, plugins, workflows, roles, users, system settings, file storages, themes) and the `/admin/settings` pages. Reads such as `list`, `get`, `check` and `query` stay allowed so pages still load.

Files: `nginx/conf.d/clicker.conf` (the two doors and the blocking rules) and `nginx/snippets/proxy-to-app.conf` (how requests are passed to NocoBase).

### Run it

```
docker compose up -d
```

### Verify it

Run these in cmd from the project folder:

```
curl.exe -s -o NUL -w "%{http_code}" http://localhost:8080/
curl.exe -s -o NUL -w "%{http_code}" -X POST http://localhost:8080/api/collections:create
curl.exe -s -o NUL -w "%{http_code}" http://localhost:8080/admin/settings/ui-editor
curl.exe -s -o NUL -w "%{http_code}" -X POST http://localhost:8081/api/collections:create
curl.exe -s -o NUL -w "%{http_code}" http://localhost:13000/
```

Expected: `200`, `403`, `403`, `401` (the intranet door let the request through to NocoBase, which asks for a login), and `000` (nothing is listening on 13000 anymore).

In a browser, sign in on both doors. On http://localhost:8081 you can open the UI Editor and change pages. On http://localhost:8080 the same attempt shows "Forbidden: this route changes the configuration and is only available from the intranet."

To check the Selenium tests still pass through the public door:

```
docker compose --profile test run --rm tests
```

A passing run ends with `6 passed`.

## Work from home (simulated VPN)

This simulates an employee at home who reaches the intranet proxy through a VPN. A `vpn` gateway container joins both the home network (`wfh`) and the intranet network (`nocobase`) and forwards port 8081 to the proxy's intranet door. A Chromium browser sits on the home network only.

1. Start it:

```
docker compose --profile wfh up -d
```

2. Open http://localhost:3000. This is the home computer's browser. Use the lower (inner) address bar.
3. In that browser, go to `http://vpn:8081`. The NocoBase admin loads, so admin functions work from home.
4. Try `http://app` and `http://proxy:8081` in the same browser. Both fail, because the home network has no direct route to the intranet. Only the VPN gateway reaches it.
5. Optional: `http://host.docker.internal:8080` is the public door, which still refuses configuration changes.
6. Stop it:

```
docker compose --profile wfh down
```

## Cleaning up

Close the page as normal in your browser, and make sure to run "docker compose --profile wfh --profile test down" in the cmd from earlier before closing it.





## Useful Docker Compose commands

### Start



docker compose up

### Stop and remove the containers



docker compose down

The PostgreSQL data is stored in the local `storage/` directory, so `docker compose down` does not delete that project data.

## Sources



&#x20;   - Docker Compose Quickstart: https://docs.docker.com/compose/gettingstarted/

* Getting Started: https://docs.nocobase.com/tutorials/v2/01-getting-started


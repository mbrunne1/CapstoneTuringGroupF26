Project/Clicker directions:

1. Download and install docker correctly using an online video/assistant to help (you can make sure that docker is correctly running by opening a regular "command prompt" in the windows search bar, and typing "docker --version" and "docker --install" if its not installed correctly)
2. Go to teams "Turing Group", then click on the "Shared" tab and within "Sprint 2" is "Sprint2matthewBrunner.zip". Download and export that file in order to use the storage folder / bind mount folder that has the data for the project in it
3. Make sure the folder that contains the compose.yml and storage folder is open (it should be in the same location as this readme.md)
4. Where the address bar is, showing you exactly where your folder is (it should look something like User->Desktop->Sprint2) click on that address bar and type "cmd" then
enter
5. Once it opens with the correct directory, run the command "docker compose up"
6. Once the cmd runs everything, you should be able to go to your browser and run the local host website using this web address: "http://localhost:13000"
7. The set log-in information is as such:
Username: admin@nocobase.com
Password: Admin29!
(Note: this is the password I selected for my compose, if it doesn't work for an external computer through a different IP address, attempt the default password: 	"admin123")
8. Using the buttons displayed, you can mess around and add or remove entries into the three tables that are currently visible for the project
9. You can exit the application at any point with no issues but to close the application in the command prompt as before, use command "docker compose down"


1. Install Docker Desktop and ensure virtualization is enabled  
2. Create a project directory by using these commands in the terminal  
   1. Mkdir my-project  
   2. Cd my-project  
3. In this folder add the compose.yml file   
4. Open your project directory using the terminal   
5. Then once inside the right directory run the command  
   1. “docker compose up”  
6. Once running, open your browser and copy the web address "[http://localhost:13000](http://localhost:13000)"   
7. This should take you to a log in page where you will fill out the following information:  
   1. Account: “admin@nocobase.com”   
   2. Password: “admin123” 
8. To inspect data sources
   1. click the gear icon in the top right, and choose the data source option.
   2. then click configure on the far right to view each of the collections. 
   3. click configure if you wish to access their fields and value types.
9. To exit the application, use the terminal command  
   1. “docker compose down”
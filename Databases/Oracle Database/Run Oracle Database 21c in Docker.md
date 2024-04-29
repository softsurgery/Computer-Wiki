# Pull the Docker Image

We can pull the image using this docker pull command. This will pull the latest update from the official container registry of Oracle. We are using the Express edition in this blog similarly other editions are also available.

```bash
docker pull container-registry.oracle.com/database/express:latest
```

Pull the Oracle DB Image

For me it did not ask for the Oracle credentials as the image has already been downloaded. But if you are doing it for the first time then it will prompt for your Oracle account credentials.

# Create & run the container

Once we have pulled the image, we can create a Docker container based on that image.

```d
docker container create `  
   -it ` # Run the container in interactive mode  
   --name [container-name] `  # Name of the container  
   -p [host-port]:1521 `  # Map the port from host to container for DB  
   -e ORACLE_PWD=[custom-pass] `  # Password for default user  
   container-registry.oracle.com/database/express:[version]  # Image  
   
```  
## Example  

```d
docker container create `  
   -it `  
   --name oracle-test `  
   -p 1521:1521 `  
   -e ORACLE_PWD=welcome123 `  
   container-registry.oracle.com/database/express:latest
```

Once the container is created, we can use the **docker start [container-name]** or **docker stop [container-name]** command to start or stop the container.

# Connect to the database

We can now connect to our newly created database using the default credentials in SQL Developer or any other SQL editors.

**Database Info**  
```
host: localhost  
port: 1521  
username: system  
password: welcome123  
sid: xe
```


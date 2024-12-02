# DockerUsage
Basic setting up projects with docker

Most guides about Docker focus heavily on the theoretical concepts of containerization and often fail to demonstrate its practical use. As someone who has experienced this frustration firsthand, I’ve created this concise guide to show you how to use Docker the practical way.

this example app will consist of four services:
  1. React - FrontEnd -> taken from my another repo LINK: https://github.com/PatrykWenz-4/dockerSite
  2. php - BackEnd for setting endpoints -> taken from my another repo LINK: https://github.com/PatrykWenz-4/dockerSite
  3. mysql - Database to store data.
  4. phpmyadmin - For menaging a database.

Main goals:
  1. Use Docker images for MySQL and phpMyAdmin to manage your database and administration tasks, eliminating the need for additional software like XAMPP.
  2. Containarize whole app so it can work on any system.

Remember that backend and frontend images have to be built
```
docker Dockerfile build
```

Step 1:
Create image of mysql database.
Create a docker-compose.yml (later it will be used for whole project) file and include.

```
  mysql:
    image: mysql:latest
    container_name: mysql-db
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: myappdb
      MYSQL_USER: myuser
      MYSQL_PASSWORD: mypassword
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql
```
And later we set up the volumu:
```
volumes:
  db_data:
```

Images like that can be found on [https://hub.docker.com/explore](https://hub.docker.com/search?badges=official)
The mysql image used: https://hub.docker.com/_/mysql.

image -> specifies what image we will use.
environment -> specifies the database and login credentials.
ports -> specify ports so outside connections is possible.
volumes -> Is a volume managed by Docker. "/var/lib/mysql" is the path inside the container where MySQL stores its data.


Start the docker-compose.yml file to start the project.

![image](https://github.com/user-attachments/assets/55f8028a-5deb-472b-b6c0-85789c985a4a)


![image](https://github.com/user-attachments/assets/6d550d39-2db0-4c3e-9e09-577e56291870)

In order to login as root we can write in cmd:
```
docker exec -it mysql-db bash

bash-5.1# mysql -u root -p

Enter password:
```
![image](https://github.com/user-attachments/assets/4edfed5f-df1e-4781-b107-94f800c00851)


Same is done for phpmyadmin

```
  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    ports:
      - "8081:80"
    environment:
      PMA_HOST: mysql
      MYSQL_ROOT_PASSWORD: root
```

And using the port we specified:

![image](https://github.com/user-attachments/assets/8c5a3fce-6b82-4248-b747-6ec35c8b2c95)

At last we containerize apps we created, so this time no image is needed from outside, we create one:
```
  react:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: react-frontend
    ports:
      - "3000:3000"
    volumes:
      - ./frontend:/app  
      - /app/node_modules 
    working_dir: /app 
    command: ["npm", "start"] 
    environment:
      - WATCHPACK_POLLING=true

  php:
    build:
      context: ./backend
      dockerfile: Dockerfile
    container_name: php-backend
    ports:
      - "8080:80"
    volumes:
      - ./backend:/var/www/html
    depends_on:
      - mysql

```

New commands:

    working_dir: /app 
    command: ["npm", "start"] 
    environment:
      - WATCHPACK_POLLING=true
      
- working_dir -> Sets the working directory inside the container to /app.
- command: ["npm", "start"]  -> Executes npm start to run the React application properly.
- WATCHPACK_POLLING=true -> thanks to this command the changes we make live in code, will work also on the container.
- depends_on -> means that image phpmyadmin will start only after sql image is set up. It is implemented so no erros occur.

Now the application is all set up.

start the up by typing:

```
docker-compose up
```

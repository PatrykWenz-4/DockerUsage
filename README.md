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

docker exec -it mysql-db bash
bash-5.1# mysql -u root -p
Enter password:

![image](https://github.com/user-attachments/assets/4edfed5f-df1e-4781-b107-94f800c00851)


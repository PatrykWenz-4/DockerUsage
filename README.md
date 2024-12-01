# DockerUsage
Basic setting up projects with docker

Those are my basic notes for setting up projects using a docker.

this example app will consist of four services:
  1. React - FrontEnd
  2. php - BackEnd for setting endpoints
  3. mysql - Database
  4. phpmyadmin - For menaginng a database


Start the docker-compose.yml file to start the project.

![image](https://github.com/user-attachments/assets/55f8028a-5deb-472b-b6c0-85789c985a4a)

mysql:

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

![image](https://github.com/user-attachments/assets/6d550d39-2db0-4c3e-9e09-577e56291870)

In order to login as root

C:\Users\patry>docker exec -it mysql-db bash
bash-5.1# mysql -u root -p
Enter password:

![image](https://github.com/user-attachments/assets/4edfed5f-df1e-4781-b107-94f800c00851)


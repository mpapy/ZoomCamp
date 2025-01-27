**Question 1. Understanding docker first run**
Answer: pip 24.3.1 

marekpapay@Marek-MacBook-Pro-2 first_week % docker run -it --entrypoint=bash python:3.12.8
root@103f028eabd7:/# pip --version
pip 24.3.1 from /usr/local/lib/python3.12/site-packages/pip (python 3.12)›
![image](https://github.com/user-attachments/assets/a806522a-5485-4c50-996e-dee5803f6397)

**Question 2. Understanding Docker networking and docker-compose**
Answer: db:5432

pgadmin goes to DB from port 80 to 5432. 

![image](https://github.com/user-attachments/assets/2a8db6b1-ec7a-47f7-a24e-15309aaff2f1)
services:
  pgdatabase:
    image: postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
    ports:
      - "5431:5432"
  pgadmin:
    image: dpage/pgadmin4
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
    ports:
      - "8080:80"


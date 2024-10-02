# DBMigrate-new

Intent is to migrate postgres DB Version 12 installed in AWS Ubuntu System to Azure Postgres DB Managed 

1 st step, we need to install Postgres 12 in Ubuntu

![image](https://github.com/user-attachments/assets/bc8b1abf-3241-4e6f-8d9c-3911922d2459)

Install postgres 12 using below steps
Installing PostgreSQL 12 on Ubuntu
Run the following command to create the file repository configuration:

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
Import the repository signing key by running the following command

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
Update the package lists apt a.k.a Advanced Packaging Tool by running the following command

sudo apt update

sudo apt -y install postgresql-12

![image](https://github.com/user-attachments/assets/05677fbc-1a37-4be8-9141-a4d09bbe975c)

![image](https://github.com/user-attachments/assets/1c13d090-4355-44eb-9672-0721b0f65c47)


![image](https://github.com/user-attachments/assets/464e8780-55ba-4c6a-ba31-a4771e3dd9c3)


![image](https://github.com/user-attachments/assets/e9f2f4d2-ca70-4994-b680-b9c1c199c755)

CREATE DATABASE mytestdb;

CREATE USER mytestuser WITH ENCRYPTED PASSWORD 'postgres';

GRANT ALL PRIVILEGES ON DATABASE mytestdb to mytestuser;

![image](https://github.com/user-attachments/assets/2c221259-9f3a-489c-b853-2cdca1772e86)

sudo nano /etc/postgresql/12/main/postgresql.conf

![image](https://github.com/user-attachments/assets/bbf29921-8c73-4869-968b-ce741f0e93c7)

sudo systemctl restart postgresql

![image](https://github.com/user-attachments/assets/d1d52971-e5de-4c8c-adc6-67eb745f9fdd)


![image](https://github.com/user-attachments/assets/2588e3bb-0b20-4358-a460-f4ea13ef2a59)






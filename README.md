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


Now we are ready with source postgres db in ubuntu

Next we will create postgres DB in Azure portal as destination DB




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


![image](https://github.com/user-attachments/assets/2df5f779-9e9d-4fbb-8ef2-99260c221308)


Server is ready

![image](https://github.com/user-attachments/assets/4870e45e-804f-4e73-999d-5d076a1ff628)


![image](https://github.com/user-attachments/assets/c398c5a5-ca56-4120-9e66-18069e7a618a)

![image](https://github.com/user-attachments/assets/9cb839ea-2cbb-4550-8119-1b50342f3adb)

DB server is ready now

Creating blank DB in Destination Server

![image](https://github.com/user-attachments/assets/3452a013-341d-48f7-8e34-bd7eb751c79c)




![image](https://github.com/user-attachments/assets/9f7fcbac-b25c-4bbc-a273-84dfc9a24e24)

Next we need to create Replicatio role

create role db_migrationuser with
login
superuser
createdb
createrole
inherit
replication
connection limit -1
password 'postgres'

grant postgres to db_migrationuser

new role created in onprem DB

![image](https://github.com/user-attachments/assets/ea14fbf0-36a1-4f4f-84f7-68e1fe2b3c8d)


![image](https://github.com/user-attachments/assets/6d8e933b-5415-4bae-8a78-7ce54e23c13d)



need to do some config changes

sudo nano /etc/postgresql/12/main/pg_hba.conf

![image](https://github.com/user-attachments/assets/38679a67-30d7-4299-a5fc-db213c07ebfe)

Use pg_dump -s command to create a schema dump file for a database.

pg_dump -O -h 20.29.58.169 -U postgres -d mytestdb -s > mytestdb_schema.sql;

![image](https://github.com/user-attachments/assets/7b9b47c3-8cec-40d4-8a7d-27275607de47)
This is the schema content

![image](https://github.com/user-attachments/assets/b7b17f5b-967f-4322-b3aa-a6518046b9e7)

Deploying this schema to Azure destinatio database

psql  -h vmpostgresazure.postgres.database.azure.com  -U postgres -d mytestdb  < mytestdb_schema.sql

![image](https://github.com/user-attachments/assets/9cb1a22a-5ca5-448f-8d02-dc6e3981d35c)


you can see schema created in destination Azure DB

![image](https://github.com/user-attachments/assets/aeb965b3-e8e2-4c18-8504-807bba0c7f2c)

Remember, only schema is loaded

![image](https://github.com/user-attachments/assets/ea97c260-c69b-43a7-b5cf-033d9673e5e7)

Next step is to create DMS for migration

![image](https://github.com/user-attachments/assets/a9656c72-9f10-47bd-8ed6-eaed6f3819b0)


![image](https://github.com/user-attachments/assets/58d04256-e770-4c85-86b7-40684589ef02)


![image](https://github.com/user-attachments/assets/fe74c067-c373-4652-818c-378381a27c11)

Created a migration project in DMS
![image](https://github.com/user-attachments/assets/829fe52c-277f-4305-bf5d-65539f0d3a5c)


![image](https://github.com/user-attachments/assets/854b83be-5bae-4775-bdef-695503c679ba)


![image](https://github.com/user-attachments/assets/a90f448a-366a-4c8d-b4fe-8f62e3cf653a)


I am migrating  only one database

![image](https://github.com/user-attachments/assets/cfd66773-4e67-4674-aa25-d1f7d567c516)


![image](https://github.com/user-attachments/assets/9c2523fe-3f95-4201-b9ed-a50d7b1e0960)


![image](https://github.com/user-attachments/assets/c9b0c318-8816-45d3-9811-8ffaaf1e4af5)

![image](https://github.com/user-attachments/assets/ec88cfe5-d813-4a42-8318-de0897b18c7c)

Migration initiated

![image](https://github.com/user-attachments/assets/0d30e3ae-66aa-4a6e-a57a-8104d468b84c)


![image](https://github.com/user-attachments/assets/16cd617e-0d5a-41a2-976a-38f07822fa8f)


![image](https://github.com/user-attachments/assets/de5c7fa7-0e72-4eb8-b531-cfd3445b2f89)

Migration is successfully completed for one table

![image](https://github.com/user-attachments/assets/f32b201d-606f-4cec-831c-8240da1e4edc)


Now I am going to add new rows in Ubuntu Onprem DB

![image](https://github.com/user-attachments/assets/f694547f-ec9b-4c72-8ff8-8a2ba8e26a99)

![image](https://github.com/user-attachments/assets/94cf6996-169a-416e-84ab-b939590c9440)


Immediately all new records migrated to Azure DB

![image](https://github.com/user-attachments/assets/5e581d50-d746-436f-8517-cf22b323e2be)


![image](https://github.com/user-attachments/assets/d3bc8e6f-4434-426c-97d2-3fce9166b35c)


Migration of table data and incriment load is working fine

Now I am going to edit this project and adding new table in ubuntu postgres DB

Created new table and added few records in source DB

![image](https://github.com/user-attachments/assets/764f4b4c-af70-4c72-99c6-08fb96790a2d)

![image](https://github.com/user-attachments/assets/5afe1258-bca9-4675-8fee-fe8364f70203)


We need to add new activity for the new migration and use pg_pump to push new schema changes to destination DB

![image](https://github.com/user-attachments/assets/7ccdd5f3-0211-4b4a-a978-f1d0090bee1c)

for existing tables, you will get error message, but schema will copy to destination DB

![image](https://github.com/user-attachments/assets/140852c9-efc4-44b7-9454-d08a75f3a9c9)
![image](https://github.com/user-attachments/assets/8d65d638-2f85-4ec3-89c4-50d51215d1a0)


![image](https://github.com/user-attachments/assets/816b83ed-d6cf-45e9-baac-4ce6e837716f)

Now come back and proceed with new activity to include new table

![image](https://github.com/user-attachments/assets/51288654-dc22-4300-bf8d-4e394b88a7b7)


If existing table, it will truncate data

![image](https://github.com/user-attachments/assets/93f07261-f5c1-4e95-9900-a75e1cb25ba1)

Both tables are loaded fully in destination DB
![image](https://github.com/user-attachments/assets/a1ff5594-9c05-4abf-a955-ac3ac70320f8)

![image](https://github.com/user-attachments/assets/d9521e5d-600a-4f8d-af39-b5fcf2af954b)

![image](https://github.com/user-attachments/assets/6e321bbb-6647-475b-9ea6-c5fc2c960993)


You can see list resources used for this live DB migration activities

![image](https://github.com/user-attachments/assets/9143d006-f846-4f71-ba3c-68d560892e0d)



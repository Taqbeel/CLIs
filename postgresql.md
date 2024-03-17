
# Postgresql Ubuntu

To install PostgreSQL, first refresh your server’s local package index:
    
    sudo apt update

Then, install the Postgres package along with a **-contrib** package that adds some additional utilities and functionality:

    sudo apt install postgresql postgresql-contrib

Ensure that the service is started:
 
    sudo systemctl start postgresql.service

By default, Postgres uses a concept called “roles” to handle authentication and authorization. These are, in some ways, similar to regular Unix-style users and groups.

Upon installation, Postgres is set up to use ident authentication, meaning that it associates Postgres roles with a matching Unix/Linux system account. If a role exists within Postgres, a Unix/Linux username with the same name is able to sign in as that role.

The installation procedure created a user account called ***postgres*** that is associated with the default Postgres role. There are a few ways to utilize this account to access Postgres. One way is to switch over to the ***postgres*** account on your server by running the following command:

    sudo -i -u postgres

Then you can access the Postgres prompt by running:

    psql

This will log you into the PostgreSQL prompt, and from here you are free to interact with the database management system right away.

To exit out of the PostgreSQL prompt, run the following:

    \q

This will bring you back to the postgres Linux command prompt. To return to your regular system user, run the **exit** command:

    exit

Another way to connect to the Postgres prompt is to run the **psql** command as the postgres account directly with **sudo:**

    sudo -u postgres psql

If you are logged in as the postgres account, you can create a new role by running the following command:

    createuser --interactive

If, instead, you prefer to use sudo for each command without switching from your normal account, run:

    sudo -u postgres createuser --interactive

Either way, the script will prompt you with some choices and, based on your responses, execute the correct Postgres commands to create a user to your specifications.

    Output
    Enter name of role to add: sammy
    Shall the new role be a superuser? (y/n) y

You can create the appropriate database with the createdb command.

If you are logged in as the postgres account, you would type something like the following: 

    createdb sammy

or
    
    CREATE DATABASE sammy;

If, instead, you prefer to use sudo for each command without switching from your normal account, you would run:

    sudo -u postgres createdb sammy

After the new “test_erp” database is created, connect to it.

    \c test_erp

Now you are ready to start creating tables where your data will be stored. Let’s create your first table with a primary key, and three client attributes.

    CREATE TABLE clients (id SERIAL PRIMARY KEY, first_name VARCHAR, last_name VARCHAR, role VARCHAR);

After the installation you may double-check that postgresql daemon is active.

    service postgresql status

PostgreSQL installation also creates a default database named “postgres” and connects you to it automatically when you first launch psql.

After first launching psql, you may check the details of your connection by typing **\conninfo** into the interpreter.

If you want to see a list of all the databases and users that are available on a server

    DBs: \l
    Users: \du

Since the default “postgres” user does not have a password, you should set it yourself.

    \password postgres

To connect your PostgreSQL database in your VPS with pgAdmin4, you can follow these general steps:

- Install pgAdmin4 on your local machine if you haven't already.

- Ensure that PostgreSQL is installed and running on your VPS.

- Allow remote connections to your PostgreSQL server by modifying the PostgreSQL configuration file. You can usually find this file at **/etc/postgresql/{version}/main/postgresql.conf** on Ubuntu. Look for the **listen_addresses** parameter and change it to listen on your **VPS's IP address**. For example, **listen_addresses = '192.168.1.100'**.

- In the same configuration file, locate the pg_hba.conf file and add an entry to allow access from your local machine.

For example:

    host    all             all             192.168.1.0/24            md5
  
or if you want to get access to this from anywhere you can try 

    host    all             all             192.168.1.0/24            md5

- Restart the PostgreSQL service to apply the changes.

      sudo systemctl restart postgresql

- Open pgAdmin4 on your local machine and create a new server connection. Provide the IP address of your VPS, the username, password, and the database name.

- Test the connection to ensure that pgAdmin4 can successfully connect to your PostgreSQL server.

Please note that allowing remote connections to your database server may introduce security risks, so ensure that you have appropriate firewall rules and security measures in place to protect your database.

- Save the file and exit the editor.
  - Update firewall settings:
    - If you are using a firewall on your VPS, you will need to allow incoming traffic on the PostgreSQL port (default is 5432). You can use the following command to open the port:

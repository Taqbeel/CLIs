# CLIs
Commands for different technologies


# [Postgresql Ubuntu]: CLIs/blob/main/postgresql.md

[r2h]: http://github.com/github/markup/tree/master/lib/github/commands/rest2html

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

    

# React Native
    -   npx react-native init "ProjName" --version=x.xx.x
    -   react-native run-android
    -   react-native run-ios
    -   cd android && ./gradlew clean

# ReactJS
    -   yarn/npm start
    -   yarn/npm build

# NestJS
    -   npm i -g @nestjs/cli
    -   nest new ProjName
    -   yarn start:dev

# Amplify
    -   Install amplify cli globally: npm install -g @aws-amplify/cli
    -   Install Python 3.8.0 (add environment variable for python, pipenv and pipvenv which are suggested on 
        installation steps): https://www.python.org/downloads/release/python-380/
    -   Initialize amplify: amplify init
    -   Initialize environment name
    -   Provide authenticaion
    -   Choose editor
    -   Choose region
    -   Provide Keys for (FB, Google, Amazon)
    -   Check amplify status (should point to new environment): amplify status
    -   Push repo to amplify (wait for 15-20 minutes): amplify push

# EC2 (linux) - code-deploy-agent-commands
    -   #!/bin/bash
    -   sudo apt update
    -   sudo apt install ruby-full --yes
    -   sudo apt install wget
    -   cd /home/ubuntu
    -   wget https://aws-codedeploy-ap-south-1.s3.ap-south-1.amazonaws.com/latest/install
    -   chmod +x ./install
    -   sudo ./install auto > /tmp/logfile
    -   sudo service codedeploy-agent start

    # Note
    Missing credentials - please check if this instance was started with an IAM instance profile
    -   sudo service codedeploy-agent restart

# EC2 (linux) - optional-commands
    -   curl -s https://deb.nodesource.com/setup_16.x | sudo bash
    -   sudo apt install nodejs --yes
    -   sudo apt install npm --yes
    -   sudo npm install --global yarn
    -   sudo npm install pm2@latest -g

# buildspec.yml (root file)
    version: 0.0

    phases:
    #  build,install,post_build,pre_build
    install:
        commands:
        - sudo apt install yarn
        - cd server/
        - yarn install -f
        # Writing env var to .env file
    build:
        commands:
        - yarn run build
    artifacts:
    files:
        - "**/*"

# Appspec.yml (root file)
    version: 0.0
    os: linux
    files:
    - source: /
        destination: /home/ubuntu/app/
        overwrite: yes
    permissions:
    - object: /home/ubuntu/app/server/codedeploy
        pattern: "*.sh"
        owner: ubuntu
        mode: 775
        type:
        - file
    hooks:
    BeforeInstall:
        - location: server/codedeploy/before_install.sh
        runas: ubuntu
        timeout: 300
    ApplicationStart:
        - location: server/codedeploy/app_start.sh
        runas: ubuntu
        timeout: 300

# before_install.sh (root/server/codedeploy)
    sudo rm -rf /home/ubuntu/app

# app_start.sh (root/server/codedeploy)
    #give permission for everything in the app directory
    sudo chmod -R 777 /home/ubuntu/app

    cd /home/ubuntu/app/server
    sudo yarn install -f
    # sudo npm run test

    pm2 kill
    pm2 start /home/ubuntu/app/server/dist/main.js
    pm2 start 0

# Web3 in React Native
    -   reference: https://levelup.gitconnected.com/tutorial-how-to-set-up-web3js-1-x-with-react-native-0-6x-2021-467b2e0c94a4
    -   npm i --save react-native-crypto react-native-randombytes
    -   npm i --save-dev rn-nodeify@latest
    -   ./node_modules/.bin/rn-nodeify --install
    -   npm i base-64
    -   update: metro.config.js
    -   `const extraNodeModules = require('node-libs-browser');
        module.exports = {
            resolver: {
                extraNodeModules,
            },
            transformer: {
                    getTransformOptions: async () => ({
                    transform: {
                    experimentalImportSupport: false,
                    inlineRequires: false,
                    },
                }),
            },
        };`
    -   update: package.json
    -   `"postinstall": "./node_modules/.bin/rn-nodeify --install 'crypto,buffer,react-native-randombytes,vm,stream,http,https,os,  url,net,fs' --hack"`
    -   yarn install
    -   update: shim.js (newly created in root)
    -   `import {decode, encode} from 'base-64'
        if (!global.btoa) global.btoa = encode
        if (!global.atob) global.atob = decode
        if (typeof __dirname === 'undefined') global.__dirname = '/'
        if (typeof __filename === 'undefined') global.__filename = ''
        if (typeof process === 'undefined') {
            global.process = require('process')
        } else {
            const bProcess = require('process')
            for (var p in bProcess) {
                if (!(p in process)) {
                    process[p] = bProcess[p]
                }
            }
        }
        process.browser = false
        if (typeof Buffer === 'undefined') global.Buffer = require('buffer').Buffer
        if (typeof location === 'undefined') global.location = { port: 80, protocol: 'https:' }
        const isDev = typeof __DEV__ === 'boolean' && __DEV__
        process.env['NODE_ENV'] = isDev ? 'development' : 'production'
        if (typeof localStorage !== 'undefined') {
            localStorage.debug = isDev ? '*' : ''
        }
        // If using the crypto shim, uncomment the following line to ensure
        // crypto is loaded first, so it can populate global.crypto
        require('crypto')`
    -   npm i --save web3
    -   update: app.js
    -   import: `import './shim'`
    -   you can use now Web3 in your app......
    -   usage as follows
    -   `import Web3 from 'web3'; 
        const web3 = new Web3('http://localhost:7545');
        const newWallet = web3.eth.accounts.wallet.create(1);
        const newAccount = newWallet[0];
        console.log(newAccount);`

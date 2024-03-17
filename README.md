# CLIs
Commands for different technologies


## [Postgresql Ubuntu](/postgresql.md)

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

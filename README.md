## Nullion Explorer

### Installation

##### Node.js

The recommended way to install Node.js is by using the Node Version Manager (NVM):

```
sudo apt update
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 20.9.0
```

##### MongoDB

It is recommended to follow the install instructions at the official mongo website since they will be updated more often and have specific instructions for many different operating systems: [https://www.mongodb.com/docs/manual/administration/install-community/](https://www.mongodb.com/docs/manual/administration/install-community/).

Below are instructions to install the latest v7.x version of MongoDB on Ubunutu 22.04 (run one line at a time):

```
sudo apt-get install gnupg curl
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
```

Once MongoDB is installed it is recommended to run the following cmds to start the database server and add it as a service to ensure it starts up automatically after a reboot:

```
sudo systemctl start mongod
sudo systemctl enable mongod.service
```

#### Database Setup

Open the MongoDB cli (The legacy mongo shell `mongo` was deprecated in MongoDB v5.0 and removed in MongoDB v6.0. Newer installs must now use `mongosh`):

```
mongosh
```

Select database:

**NOTE:** `explorerdb` is the name of the database where you will be storing local explorer data. You can change this to any name you want, but you must make sure that you set the same name in the `settings.json` file for the `dbsettings.database` setting.

```
use explorerdb
```

Create a new user with read/write access:

```
db.createUser( { user: "exploreruser", pwd: "Nd^p2d77ceBX!L", roles: [ "readWrite" ] } )
```

Exit the mongo shell:

```
exit
```

#### Download Source Code

```
git clone https://github.com/Nemo-Nullis/explorer
```

#### Install Node Modules

```
cd explorer && npm install --only=prod
```

#### Configure Explorer Settings

```
cp ./settings.json.template ./settings.json
```

*Make required changes in settings.json*

**NOTE:** You can further customize the site by adding your own javascript code to the `public/js/custom.js` file and css rules to the `public/css/custom.scss` file. Adding changes to `custom.js` and `custom.scss` is the preferred method of customizing your site, without affecting the ability to receive explorer code updates in the future.

### Start/Stop the Explorer

#### Start Explorer (Use for Testing)

You can launch the explorer in a terminal window that will output all warnings and error msgs with one of the following cmds (be sure to run from within the explorer directory):

```
npm start
```

or (useful for crontab):

```
cd /path/to/explorer && /path/to/npm run prestart && /path/to/node --stack-size=10000 ./bin/cluster
```
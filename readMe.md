# Ximera Install Notes

## Ubuntu Install
### Step 1: Install Node.js and NPM
Down load version 7.x
```
sudo apt-get install libcurl4-gnutls-dev libgit2-dev
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_7.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
sudo apt-get install nodejs
sudo ln /usr/bin/nodejs /usr/bin/node
```

Download the newest version (may not work with ximera)
```
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install nodejs
```

### Step 2: Install Gulp
```
sudo npm install gulp-cli -g
sudo npm install gulp -D
```

### Step 3: Install MongoDB
[Click here for a quick list of mongo commands](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/#start-mongodb)

#### 1. Import the public key used by the package management system
```
sudo apt-get install wget
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
```
The operation should respond with an OK.

#### 2. Create a list file for MongoDB
navigate to the etc/apt/sources.list.d directory and create the list file for MongoDB
```
cd ~/../../etc/apt/sources.list.d/
sudo touch mongodb-org-4.2.list
```
```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

#### 3. Reload local package database
```
sudo apt-get update
```

#### 4. Install the MongoDB packages
```
sudo apt-get install -y mongodb-org
```

#### 5. Download the sample database
[Download the sample database](https://drive.google.com/file/d/0B-Xh-RAGRDU8WHAxeUJfVGpTSk0/edit) and then extract it. Afterwards, run the following commands:

```
sudo service mongod start
mongorestore --db ximera /home/matt/Downloads/database/test
```

### Step 4: Install Redis
```
sudo apt update
sudo apt install redis-server
```

then update this file:
```
sudo nano /etc/redis/redis.conf
```
change supervised no to supervised systemd
```
# If you run Redis from upstart or systemd, Redis can interact with your
# supervision tree. Options:
#   supervised no      - no supervision interaction
#   supervised upstart - signal upstart by putting Redis into SIGSTOP mode
#   supervised systemd - signal systemd by writing READY=1 to $NOTIFY_SOCKET
#   supervised auto    - detect upstart or systemd method based on
#                        UPSTART_JOB or NOTIFY_SOCKET environment variables
# Note: these supervision methods only signal "process is ready."
#       They do not enable continuous liveness pings back to your supervisor.
supervised systemd
```
restart the Redis service to reflect the changes you made to the configuration file
```
sudo systemctl restart redis.service
```
finally, check that the Redis service is running:
```
sudo systemctl status redis
```
If it is running without any errors, this command will produce output similar to the following:
```
● redis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2018-06-27 18:48:52 UTC; 12s ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 2421 ExecStop=/bin/kill -s TERM $MAINPID (code=exited, status=0/SUCCESS)
  Process: 2424 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 2445 (redis-server)
    Tasks: 4 (limit: 4704)
   CGroup: /system.slice/redis-server.service
           └─2445 /usr/bin/redis-server 127.0.0.1:6379
```

### Step 5: Install build-essential
this includes the g++ compiler
```
sudo apt install build-essential
```

### Step 6: Install Node.js Legacy
```
sudo apt-get install nodejs-legacy
```

### Step 7: CLone the repository
```
git clone https://github.com/kisonecat/ximera
```

### Step 8: Create the .env with content inside the ximera directory
```
 XIMERA_MONGO_URL=127.0.0.1
 XIMERA_MONGO_DATABASE=ximera
 XIMERA_COOKIE_SECRET=thisismysecretcookieyoushouldchangethis
 COURSERA_CONSUMER_KEY=thisisacourserakey
 COURSERA_CONSUMER_SECRET=courserasecretkey
 LTI_KEY=myltikey
 LTI_SECRET=myltisecret
 GITHUB_WEBHOOK_SECRET=githubwebhooksecret
```

### Step 9: Install the node dependencies
```
npm install
```
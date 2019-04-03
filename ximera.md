# Installing Ximera
**Table of Contents**
* [Windows Installation Guide](#user-content-windows-installation-guide)
* [Resolved Errors](#user-content-common-Errors)
   * [Can't find Python executable](#user-content-cant-find-python-executable)
* [Unresolved Errors](#user-content-unresolved-errors)
  * [MongoDB](#user-content-mongodb)
  * [Node](#user-content-node)
* [Unresolved Warnings](#user-content-unresolved-warnings)

## Windows Installation Guide

### Step 1
Download and install [nodejs](https://nodejs.org/en/)

### Step 4
Download and install [mongodb](https://www.mongodb.com/download-center/community)
   1. Select MongoDB Community Server with the following settings:
      * **Version** - current release
      * **OS** - Windows 64-bit x64
      * **Package** - ZIP
      * Click download
   2. Extract the contents and rename the parent folder to something more readable (i.e. mongodb)
   3. Create another folder and name it `mongodb-data`
   4. Move these 2 folders into your User directory
   5. Run the following cmd in the command window. Before you do, however, change the username in the path to your username (i.e. change /Users/matthew.mckee to /Users/your.username)
      * `/Users/matthew.mckee/mongodb/bin/mongod.exe --dbpath=/Users/matthew.mckee/mongodb-data`
   6. MongoDB should now have an active session. In your cmd prompt, press `ctrl + c` to end the session for now. To start a new session, run the cmd you just ran from the previous step
   
### Step 3
Clone the ximera server repository by running `git clone https://github.com/kisonecat/ximera`

### Step 4
Create a batch file with the following lines.

  * **Note:** for the following steps, you will need to run this batch file in every new cmd prompt tab you open as it temporarily sets the variables
  * **Note:** for NodejsPath, NPMPath, and MongoDBPath - change the path names to where those files belong on your PC
  * Follow this guide for [creating and saving a batch file](https://www.tutorialspoint.com/batch_script/batch_script_files.htm)

```
 SET NodejsPath=C:\Program Files\nodejs
 SET NPMPath=C:\Program Files\nodejs
 SET MongoDBPath=C:\Users\matthew.mckee\mongodb\bin
 SET Path=%path%%NodejsPath%;%NPMPath%;%MongoDBPath%
 SET XIMERA_MONGO_URL=127.0.0.1
 SET XIMERA_MONGO_DATABASE=test
 SET XIMERA_COOKIE_SECRET=thisismysecretcookieyoushouldchangethis
 SET COURSERA_CONSUMER_KEY=thisisacourserakey
 SET COURSERA_CONSUMER_SECRET=courserasecretkey
 SET LTI_KEY=myltikey
 SET LTI_SECRET=myltisecret
 SET GITHUB_WEBHOOK_SECRET=githubwebhooksecret
```

### Step 5
Open the command window, run the batch file and install the node dependencies:
  1. Run the batch file you just created by typing in the name of the batch file
  1. Change to the ximera server folder: `cd ximera`
  1. Run `npm install`

### Step 6
In the same command window tab, run a new instance of mongodb. If this is a new command window tab, be sure to run the batch file again. run the same cmd from earlier, `/Users/matthew.mckee/mongodb/bin/mongod.exe --dbpath=/Users/matthew.mckee/mongodb-data`

Leave this tab open. Whenever you close it or close this mongodb session you will need to repeat this step.

### Step 7
Open a new command window tab. Run the batch file again. Afterwards, download and import the test database into mongodb.
   1. Download the [test database](https://drive.google.com/file/d/0B-Xh-RAGRDU8WHAxeUJfVGpTSk0/edit)
   1. Extract the test database
   1. Import the test database by running: `mongorestore -d db_name folder_path`
      * example: `mongorestore -d test C:\Users\matthew.mckee\Downloads\test`

### Step 8
In the same command window tab, start the app
   1. run the batch file again if this is a new tab
   1. ensure you are in the ximera directory
   1. run `node app.js`
   
### Step 9
Open your browser and go to localhost:3000

## Resolved Errors
### Can't find Python executable
If you haven't got python installed along with all the node-gyp dependencies, simply open Powershell or Git Bash with administrator privileges and run: `npm install --global --production windows-build-tools`

Next, install node-gyp: `npm install --global node-gyp`

Finally, restart you command window and run python. You should now see the python shell (exit out with `ctrl + z`). Run npm install within the ximera directory.

## Unresolved Errors
### MongoDB

* Running `mongorestore` successfully imported the test database, however, received the following error in the execution output:
```
2019-04-02T14:25:06.036-0400 Failed: test.courses: error creating indexes for test.courses: createIndex error: The field 'safe' is not valid for an index specification. Specification: { safe: null, name: "slug_1", ns: "test.courses", background: true, key: { slug: 1 }
```

### Node

* Missing Private key file
```
Missing private key file C:\Users\matthew.mckee\Documents\projects\xronos\ximera\private_key.pem
```

* Can't find module '../build/Release/hash'
```
module.js:549
    throw err;
    ^

Error: Cannot find module '../build/Release/hash'
    at Function.Module._resolveFilename (module.js:547:15)
    at Function.Module._load (module.js:474:25)
    at Module.require (module.js:596:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (C:\Users\matthew.mckee\Documents\projects\xronos\ximera\node_modules\xxhash\lib\xxhash.js:4:13)
    at Module._compile (module.js:652:30)
    at Object.Module._extensions..js (module.js:663:10)
    at Module.load (module.js:565:32)
    at tryModuleLoad (module.js:505:12)
    at Function.Module._load (module.js:497:3)
    at Module.require (module.js:596:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (C:\Users\matthew.mckee\Documents\projects\xronos\ximera\routes\state.js:11:14)
    at Module._compile (module.js:652:30)
    at Object.Module._extensions..js (module.js:663:10)
    at Module.load (module.js:565:32)
```

## Unresolved Warnings
* acron-dynamic-import@4.0.0 requires a peer
```
WARN acorn-dynamic-import@4.0.0 requires a peer of acorn@^6.0.0 but none is installed. You must install peer dependencies yourself.
```

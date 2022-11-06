# DSS front-end compilation documentation

## start the process

### 1. Install Node.js

Download Node.js to your computer and install it.

Download address: [http://nodejs.cn/download/](http://nodejs.cn/download/) (The latest stable version may have incompatibility problems, you can use the tested versions v10.16.2, v- 14.15.0) Back-end students suggest that it be done under Linux

**This step is only required for the first use. **

### Second, the installation project

Execute the following command in the terminal command line:

```shell script
cd DataSphereStudio/web
lerna bootstrap
````

Instruction introduction:
1. Go to the root directory of the project package: `cd DataSphereStudio/web`
2. Dependencies required to install the project: `lerna bootstrap`
3. If the lerna command is not installed, you can install it through the `npm install lerna -g` command

### Three, packaging project

You can package the project and generate the compressed code by executing the following command on the terminal command line:

```shell script
npm run build
````

After the command is successfully executed, a folder named "dist" will appear in the project root directory, which is the packaged code. You can put this folder directly into your static server.

### Fourth, run the project

If you want to run the project on a local browser and change the code to see the effect, you need to execute the following command in the terminal command line:

```shell script
npm run serve
````

Access the app in a browser (Chrome is recommended) via the link: ```http://localhost:8080/```

When you run the project in this way, the effect of your code changes will be dynamically reflected on the browser.

**Note: Because the project adopts front-end and back-end separate development, when running on the local browser, you need to set the browser to cross-domain to access the back-end interface:**

Here is how the chrome browser sets up cross-domain:

- Cross-domain configuration under windows system:
    1. Close all chrome browsers.
    2. Create a new chrome shortcut, right-click "Properties", select "Target" in the "Shortcut" tab, and add ```--args --disable-web-security --user-data-dir=C:\ MyChromeDevUserData````
    3. Open chrome browser via shortcut

- Cross-domain configuration under mac system:

Execute the following command in the terminal command line (you need to replace yourname in the path, if it does not work, please check the location of the MyChromeDevUserData folder on your machine and copy the path to ```--user-data-dir=`` in the following command ` behind)

```shell script
open -n /Applications/Google\ Chrome.app/ --args --disable-web-security --user-data-dir=/Users/yourname/MyChromeDevUserData/
````
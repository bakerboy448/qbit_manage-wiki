
# Unraid Installation - Docker
Thankfully, getting qbit_manager working on unRAID is a fairly simple task. unRAID works mostly with docker containers, so the pre-built container available on docker hub works perfectly with a little configuration. To install a container from docker hub, you will need community applications - a very popular plugin for unRAID servers. If you don't already have this installed, you can install it [here](https://forums.unraid.net/topic/38582-plug-in-community-applications/)

## Basic Installation
1. Head to the Apps tab of unRAID (Community Applications), and search qbit_manage in the upper left search box. 
2. Once you have searched for qbit_manage you can simply select it from the list of containers and select install.
3. The template should show all variables that can be edited.
4. Fill out your desired location for your application data.
5. Fill out your location for all your download folders.
6. Select what options you want to enable or disable (true/false).
   <Insert Image here>
7. Hit Apply, and allow unRAID to download the docker container.
8. Navigate to the Docker tab in unRAID, and stop the qbit_manage container if it has auto-started.
9.  Create config.yml and library.yml files as-per the documentation in the Host Path you set (/mnt/user/appdata/qbit_manage in the example)
10. Once finished, run the container. Voila! Logs are located in yourhostpath/logs.
# Unraid Installation localhost
Here we are going to talk about using qBit Manager on unRAID

**qBit Management**
First, we are going to need [Nerd Pack](https://forums.unraid.net/topic/35866-unraid-6-nerdpack-cli-tools-iftop-iotop-screen-kbd-etc/). <br>
This can be also downloaded from the **Apps** store

Nerd pack will be located in the settings tab
When you open it up you'll see a bunch of packages that you can install. <br> We'll need:

* `python-pip`

* `python3`

* `python-setuptools`

To get this running in unRAID go ahead and download the repo to your computer. 

Then take all the data from the zip file and place it somewhere on your server.

An example of this would be: `/mnt/user/data/scripts/qbit/`

Now we need to install the requirements for this script. 

Head back over to **User Scripts**

Create a new script: An example of this would be `install-requirements`

In the new text field you'll need to place:
```bash
#!/bin/bash
echo "Installing required packages"
python3 -m pip install -r /mnt/user/path/to/requirements.txt 
echo "Required packages installed"
```
Replace `path/to/` with your path example mines `/data/scripts/qbit/` or `/mnt/user/data/scripts/qbit/requirements.txt`

Now click **Save Changes**

Now to set a schedule for this bash script to run. 

Select **At First Array Start Only** This will run this script every time the array starts on every boot

Now we need to edit the config file that came with the zip file.
<br>The config file should be pretty self-explanatory. 
<br>The only thing that must be followed is that **ALL** categories that you see in your qBit **MUST** be added to the config file with associated directories, each directory must be unique for each category.

> If you'd like a guide on setting up cross-seed on unRAID please visit [here](https://github.com/Drazzilb08/cross-seed-guide)
  
Now we need to go back to **User Scripts** and create our script to run this script

**Add a new script**

  You can name yours something like `auto-manage-qbittorrent`
  Here is an example script:
  ```bash
  #!/bin/bash
echo "Running qBitTorrent Management"
python3 /mnt/user/data/scripts/qbit/qbit_manage.py -c /mnt/user/data/scripts/qbit/config.yml -ms -l /mnt/user/data/scripts/qbit/activity.log
echo "qBitTorrent Management Completed"
```
However, at the core, you'll want 
```
python3 /<path to script>/qbit_manage.py -c /<path to config>/config.yml -ms -l /<path to where you want log file>/activity.log
```
if you want to change the arguments such as the `-ms` a full list of arguments can be seen by using the `-h` command.

  
  Once you've got the config file set up you should be all set. 
  Don't forget to set a cron schedule mines <br>`*/30 * * * *` <-- Runs every 30 min
  
**Final note:**<br>
If you're wanting to do a test run please use the `--dry-run` argument anywhere w/in the call to test how things will look. Please do this before running a full run.

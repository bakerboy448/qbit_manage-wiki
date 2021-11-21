# Docker Installation
# Nix Installation
Nix Installation

* Download the script

```bash
wget -O qbit_manage.py 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/qbit_manage.py'
```

* Make it executable

```bash
chmod +x qbit_manage.py
```

* Get & Install Requirements

```bash
wget -O requirements.txt 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/requirements.txt'
pip install -r requirements.txt
```

* Get Example Config

```bash
wget -O config.yml.sample 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/config.yml.sample'
```

* Create Config

```bash
cp config.yml.sample config.yml
nano -e config.yml
```

* Updating

```bash
wget -O qbit_manage.py 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/qbit_manage.py'
chmod +x qbit_manage.py
wget -O requirements.txt 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/requirements.txt'
pip install -r requirements.txt
wget -O config.yml.sample 'https://raw.githubusercontent.com/StuffAnThings/qbit_manage/master/config.yml.sample'
diff -ui config.yml config.yml.sample
```

# Unraid Installation
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

# Other Local Installations

* Requires `python 3`. Dependencies must be installed by running:

```bash
pip install -r requirements.txt
```

If there are issues installing dependencies try:

```bash
pip install -r requirements.txt --ignore-installed
```

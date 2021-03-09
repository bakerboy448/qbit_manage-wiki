Here we are going to talk about using qBit Manager on unRAID

## qBit Management
First we are going to need [Nerd Pack](https://forums.unraid.net/topic/35866-unraid-6-nerdpack-cli-tools-iftop-iotop-screen-kbd-etc/). <br>
This can be also download from the **Apps** store

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

Below you can see an example of one. Note you'll need to add **ALL** of your categories.
```yaml
qbt:
  host: '<qbit address>:8080'
  user: 'user'
  pass: 'password'

directory:
  # Do not remove these
  # Cross-seed var: </your/path/here/> This is where Cross-Seed outputs its torrents
  cross_seed: '/mnt/user/path/to/output'
# Category/Pathing Parameters
cat:
  # <Category Name> : <save_path> #Path of your save directory. Can be a keyword or full path
  bib-upload: 'bib-upload'
  comics: 'comics'
  completed-ebooks: 'completetd-ebooks'
  completed-movies: 'completed-movies'
  completed-music: 'completed-music'
  completed-series: 'completed-series'
  ebooks: 'ebooks'
  movies: 'movies'
  music: 'music'
  ptp-upload: 'ptp-upload'
  series: 'series'
  tmp: 'tmp'


# Tag Parameters
tags:
  # <Tracker URL Keyword>: <Tag Name>
  beyond-hd: Beyond-HD
  animebytes.tv: AnimeBytes
  blutopia: Blutopia
  hdts: HD-Torrents
  landof.tv: BroadcasTheNet
  passthepopcorn: PassThePopcorn
  stackoverflow: IPTorrents
  empirehost: IPTorrents
  routing.bgp: IPTorrents
  morethantv: MoreThanTV
  myanonamouse: MyAnonaMouse
  opsfet: Orpheus
  torrentleech: TorrentLeech
  tleechreload: TorrentLeech
  tv-vault: TV-Vault
  ```
  **NOTE:** The cross-seed would be the output of any cross-seed script that you may have running. This script was originally designed to work with this [Cross-Seed](https://github.com/mmgoodnow/cross-seed) script working in `Search` mode  (since at this time qBit integration is not supported *yet*)

  If you'd like a guide on setting up cross-seed on unRAID please visit [here](https://github.com/Drazzilb08/cross-seed-guide)
  
  Now we need to go back to **User Scripts** and create our script to run this script

  **Add a new script**

  You can name yours something like: `auto-manage-qbittorrent`
  Here is my script:
  ```bash
  #!/bin/bash
echo "Running qBitTorrent Management"
python3 /mnt/user/data/scripts/qbit/qbit_manage.py -c /mnt/user/data/scripts/qbit/config.yml -ms -l /mnt/user/data/scripts/qbit/activity.log
echo "qBitTorrent Management Completed"
```
However, at the core you'll want 
```
python3 /<path to script>/qbit_manage.py -c /<path to config>/config.yml -ms -l /<path to where you want log file>/activity.log
```
if you want to change the arguments such as the `-ms` a full list of arguments can be seen below
```
A mix of scripts combined for managing qBittorrent.

optional arguments:
  -h, --help            show this help message and exit
  -c CONFIG, --config-file CONFIG
                        This is used if you want to use a different name for your config.yml. Example: tv.yml
  -l LOGFILE, --log-file LOGFILE
                        This is used if you want to use a different name for your log file. Example: tv.log
  -m, --manage          Use this if you would like to update your tags, categories, remove unregistered torrents,
                        AND recheck/resume paused torrents.
  -s, --cross-seed      Use this after running cross-seed script to add torrents from the cross-seed output folder
                        to qBittorrent
  -re, --recheck        Recheck paused torrents sorted by lowest size. Resume if Completed.
  -g, --cat-update      Use this if you would like to update your categories.
  -t, --tag-update      Use this if you would like to update your tags.
  -r, --rem-unregistered
                        Use this if you would like to remove unregistered torrents.
  --dry-run             If you would like to see what is gonna happen but not actually delete or tag/categorize
                        anything.
  --log LOGLEVEL        Change your log level.
 ```
  
  Once you've got the config file set up you should be all set. 
  Don't forget to set a cron schedule mines <br>`*/30 * * * *` <-- Runs every 30 min
  
**Final note:**<br>
If you're wanting to do a test run please use the `--dry-run` argument anywhere w/in the call to test how things will look. Please do this before running a full run.

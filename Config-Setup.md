
# Overview
The script utilizes a YAML config file to load information to connect to the various APIs you can connect with.

By default, the script looks at /config/config.yml for the Configuration File unless otherwise specified.

A template Configuration File can be found in the repo [config/config.yml.template](https://github.com/StuffAnThings/qbit_manage/blob/master/config/config.yml.sample).

**WARNING** <br>
> As this software is constantly evolving and this wiki might not be up to date the sample shown here might not might not be current. Please referr to the repo for the most current version.

## Config File

**Sample**
```yaml
# qBittorrent parameters
qbt:
  host: "localhost:8080"
  user: "username"
  pass: "password"

directory:
  # Do not remove these
  # Cross-seed var: </your/path/here/> #Output directory of cross-seed
  # root_dir var: </your/path/here/> #Root downloads directory used to check for orphaned files, noHL, and RecycleBin.
  # <OPTIONAL> remote_dir var: </your/path/here/> # Path of docker host mapping of root_dir.
                              # Must be set if you're running qbit_manage locally and qBittorrent/cross_seed is in a docker
  cross_seed: "/your/path/here/"
  root_dir: "/data/torrents/"
  remote_dir: "/mnt/user/data/torrents/"

# Category/Pathing Parameters
cat:
  # <Category Name> : <save_path> #Path of your save directory. Can be a keyword or full path
  movies: "/data/torrents/Movies"
  tv: "TV"

# Tag Parameters
tags:
  # <Tracker URL Keyword>: <Tag Name>
  animebytes.tv: AnimeBytes
  avistaz: Avistaz
  beyond-hd: Beyond-HD
  blutopia: Blutopia
  cartoonchaos: CartoonChaos
  digitalcore: DigitalCore
  gazellegames: GGn
  hdts: HDTorrents
  landof.tv: BroadcasTheNet
  myanonamouse: MaM
  passthepopcorn: PassThePopcorn
  privatehd: PrivateHD
  tleechreload: TorrentLeech
  torrentdb: TorrentDB
  torrentleech: TorrentLeech
  tv-vault: TV-Vault

#Tag Movies/Series that are not hard linked
nohardlinks:
  # Mandatory to fill out directory parameter above to use this function (root_dir/remote_dir)
  # This variable should be set to your category name of your completed movies/completed series in qbit. Acceptable variable can be any category you would like to tag if there are no hardlinks found
  movies-completed:
    #<OPTIONAL> exclude_tags var: Will exclude the following tags when searching through the category.
    exclude_tags:
      - Beyond-HD
      - AnimeBytes
      - MaM
    #<OPTIONAL> cleanup var: WARNING!! Setting this as true Will remove and delete contents of any torrents that are in paused state and has the NoHL tag
    cleanup: false
    #<OPTIONAL> max_ratio var: Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading
    max_ratio: 4.0
    #<OPTIONAL> seeding time var: Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding
    max_seeding_time: 86400

  #Can have additional categories set with separate ratio/seeding times defined.
  series-completed:
    #<OPTIONAL> exclude_tags var: Will exclude the following tags when searching through the category.
    exclude_tags:
      - Beyond-HD
      - BroadcasTheNet
    #<OPTIONAL> cleanup var: WARNING!! Setting this as true Will remove and delete contents of any torrents that are in paused state and has the NoHL tag
    cleanup: false
    #<OPTIONAL> max_ratio var: Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading
    max_ratio: 4.0
    #<OPTIONAL> seeding time var: Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding
    max_seeding_time: 86400

#Recycle Bin method of deletion will move files into the recycle bin (Located in /root_dir/.RecycleBin) instead of directly deleting them in qbit
#By default the Recycle Bin will be emptied on every run of the qbit_manage script if empty_after_x_days is defined.
recyclebin:
  enabled: true
  #<OPTIONAL> empty_after_x_days var: Will automatically remove all files and folders in recycle bin after x days. (Checks every script run)
  #                                   If this variable is not defined it, the RecycleBin will never be emptied. 
  #                                   WARNING: Setting this variable to 0 will delete all files immediately upon script run!
  empty_after_x_days: 60

# Orphaned files are those in the root_dir download directory that are not referenced by any active torrents.
orphaned:
    # File patterns that will not be considered orphaned files. Handy for generated files that aren't part of the torrent but belong with the torrent's files
    exclude_patterns:
        - "**/.DS_Store"
        - "**/Thumbs.db"
        - "**/@eaDir"
        - "/data/torrents/temp/**"
```
## **List of variables**<br>

### **qbt:**
---
This section defines your qBittorrent instance.

| Variable | Expected Input/Definition |
| :------------ | :------------  |
| `host:`| IP address of your qB installation.|
| `user:` | The user name of your qB's webUI.|
| `pass:` | Thee password of your qB's webUI.|

<br>

### **directory:**
---
This section defines the directories that qbit_manage will be looking into for various parts of the script.

| Variable | Expected Input/Definition |
| :------------ | :------------  |
| `cross_seed:` | Output directory of cross-seed, originally the application [cross-seed](https://github.com/mmgoodnow/cross-seed) was incapable of injecting cross-seed torrent into qB, this was built to inject them for the applicaiton. This is no longer required if you're using injects with that software. However, you can find othere uses for this as it is more of a watch directory now.|
| `root_dir` | Root downloads directory used to check for orphaned files, noHL, and RecycleBin. This directory is where you place all your downloads. This will need to be how qB views the directory where it places the downloads. This is required if you're using qbit_managee and/or qBittorrent within a container.|
| `remote_dir` | Path of docker host mapping of root_dir, this must be set if you're running qbit_manage locally and qBittorrent/cross_seed is in a docker. Essentially this is where your downloads are being kept on the host. 

## **cat:**
---
This section defines the categories that you are currently using and the path's that are associated with them <br>
<br>
> **NOTE** ALL categories must be defined, if it is in your qBit, then it **MUST** be defined here, if not the script will throw errors.

The syntax for all the categories are as follows
```yaml
category: <path>/<to>/category
```
Alternatively you can use key words to define the path.
```yaml
category: TV
```
This can be used if in qBit your path is somemething like this. `/data/.torrents/TV/`

> **Note:** Using path is going to be best, just incase you have some paths that are very similar, examples would be TV and 4K TV or Movies and 4K moviees. 

### **tags:**
---
This section defines the tags that will be used, the way this script is set up is to use tags based upon the tracker's URL.<br>
This is done so by using a Tracker URL Keyword: Tag Name system.<br>

If you are unsure what key word to use. Simply select a torrent within qB and down at the bottom you should see a tab that says `Trackers` within the list that is populated there are ea list of trackers that are associateed with this torrent, select a key word from there and add it to the config file. Make sure this key word is unique enough that the script will not get connfused with any other tracker.

### **nohardlinks:**
---
Hardlinking data allows you to have your data in both the torrent directory and your media direectory at the same time without using double the amount of data. 

If you're needing information regarding hardlinks here are some excellent resources. <br>
* https://www.youtube.com/watch?v=AMcHsQJ7My0
* https://trash-guides.info/Hardlinks/Hardlinks-and-Instant-Moves/
* https://en.wikipedia.org/wiki/Hard_link

<br>
  Mandatory to fill out [directory parameter](#-directory) above to use this function (root_dir/remote_dir) <br>
  
  Beyond this you'll need to use one of the categories above, in the example we use a `completed-` directory where torrents are stored for seeding. 

  Next section you can use the `exclude:` function to exclude certain tags, this is useful to exclude certain trackers from being scanned for hardlinking purposes

  Next is the `cleanup:` function, the only values here are `true` and `false` this will clean up non-hardlinked torrent data/torrent files once it's ratio/seed time has been met if set.

  Which brings us to the ratio/seed time section. This will set a torrent's ratio and/or seed time to the desired setting. 
  > Note: Seed time is in minutes

  ### **recyclebin:**
---
  Recycle Bin method of deletion will move files into the recycle bin (Located in /root_dir/.RecycleBin) instead of directly deleting them in qbit. 

  This is very useful if you're hesitant about using this script to delete information off your system hingswithout first checking it. Plus with the ability of this script to remove trumped/unregistered torrents there is a very small chance that something may happen to cause the script to go to town on  your library. With the recycling bin in place your data is secure (unless the bin is emptied before this issue is caught). All you'd need to do to recover would be to place the data back into the correct directory, redownload the torrent file from the tracker and recheck the torrent with the tracker from the UI. 

| Variable | Expected Input/Definition |
| :------------ | :------------  |
|`enable:`| `true` or `false` |
| `empty_after_x_days:` | integer |

> Note: The more time you place for the `empty_after_x_days:` variable the better, allowing you more time to catch any mistakes by the script. 

### **orphaned:**
---
This section allows for the exclusion of certain files from being considered "Orphaned"

This is handy when you have automatically generated files that certain OSs decide to make. `.DS_Store` Is a primary example, for those who use MacOS.

Docker Installation

A simple Dockerfile is available in this repo if you'd like to build it yourself. The official build is also available from dockerhub [here](https://hub.docker.com/r/bobokun/qbit_manage): <br>

`docker run -it -v <PATH_TO_CONFIG>:/config:rw bobokun/qbit_manage`

* The -v <PATH_TO_CONFIG>:/config:rw mounts the location you choose as a persistent volume to store your files.
  * Change <PATH_TO_CONFIG> to a folder where your config.yml and other files are.
  * The docker image defaults to running the config named config.yml in your persistent volume.
  * Use quotes around the whole thing if your path has spaces i.e. -v "<PATH_TO_CONFIG>:/config:rw"

* Fill out your location for your downloads downloads folder (`Root_Dir`).
   1. qbit_manage needs to be able to view all torrents the way that your qbittorrent views them. 
      1. Example: If you have qbittorrent mapped to `/mnt/user/data/:/data` This means that you **MUST** have qbit_managed mapped the same way.
      2. Furthermore, the config file must map the root directory you wish to monitor. This means that in our example of `/data` (which is how qbittorrent views the torrents) that if in your `/data` directory you drill down to `/torrents` that you'll need to update your config file to `/data/torrents`
   2. This could be different depending on your specific setup.
   3. The key takeaways are 
      1. Both qbit_manage needs to have the same mappings as qbittorrent
      2. The config file needs to drill down (if required) further to the desired root dir.
* `remote_dir`: is not required and can be commented out with `#` 

Below is a list of the docker enviroment variables
| Docker Environment Variable |Description | Default Value |
| :------------  | :------------ | :------------ |
| QBT_RUN |Run without the scheduler. Script will exit after completion. | False |
| QBT_SCHEDULE  | Schedule to run every x minutes. (Default set to 30)  | 30 |
| QBT_CONFIG  | This is used if you want to use a different name for your config.yml. `Example: tv.yml`  | config.yml |
| QBT_LOGFILE | This is used if you want to use a different name for your log file. `Example: tv.log` | activity.log |
| QBT_CROSS_SEED | Use this after running [cross-seed script](https://github.com/mmgoodnow/cross-seed) to add torrents from the cross-seed output folder to qBittorrent  | False |
| QBT_RECHECK | Recheck paused torrents sorted by lowest size. Resume if Completed.  | False |
| QBT_CAT_UPDATE |  Use this if you would like to update your categories.  | False |
| QBT_TAG_UPDATE |  Use this if you would like to update your tags. (Only adds tags to untagged torrents) | False |
| QBT_REM_UNREGISTERED |  Use this if you would like to remove unregistered torrents. (It will the delete data & torrent if it is not being cross-seeded, otherwise it will just remove the torrent without deleting data) | False |
| QBT_REM_ORPHANED | Use this if you would like to remove orphaned files from your `root_dir` directory that are not referenced by any torrents. It will scan your `root_dir` directory and compare it with what is in qBittorrent. Any data not referenced in qBittorrent will be moved into `/data/torrents/orphaned_data` folder for you to review/delete. | False |
| QBT_TAG_NOHARDLINKS | Use this to tag any torrents that do not have any hard links associated with any of the files. This is useful for those that use Sonarr/Radarr that hard links your media files with the torrents for seeding. When files get upgraded they no longer become linked with your media therefore will be tagged with a new tag noHL. You can then safely delete/remove these torrents to free up any extra space that is not being used by your media folder. | False |
| QBT_SKIP_RECYCLE | Use this to skip emptying the Reycle Bin folder (`/root_dir/.RecycleBin`). | False |
| QBT_DRY_RUN |   If you would like to see what is gonna happen but not actually move/delete or tag/categorize anything. | False |
| QBT_LOG_LEVEL |   Change the ouput log level. | INFO |
| QBT_DIVIDER |   Character that divides the sections (Default: '=') | = |
| QBT_WIDTH |   Screen Width (Default: 100) | 100 |


Here is an example of a docker compose
```yaml
version: "3.7"
services:
  qbit_manage:
    container_name: qbit_manage
    image: bobokun/qbit_manage
    volumes:
      - /mnt/user/appdata/qbit_manage/:/config:rw
      - /mnt/user/data/torrents/:/data/torrents:rw
    environment:
      - QBT_RUN=false
      - QBT_SCHEDULE=30
      - QBT_CONFIG=config.yml
      - QBT_LOGFILE=activity.log
      - QBT_CROSS_SEED=false
      - QBT_RECHECK=false
      - QBT_CAT_UPDATE=false
      - QBT_TAG_UPDATE=false
      - QBT_REM_UNREGISTERED=false
      - QBT_REM_ORPHANED=false
      - QBT_TAG_NOHARDLINKS=false
      - QBT_SKIP_RECYCLE=false
      - QBT_DRY_RUN=false
      - QBT_LOG_LEVEL=INFO
      - QBT_DIVIDER==
      - QBT_WIDTH=100
    restart: unless-stopped
```
You will also need to define not just the config volume but the volume to your torrents, this is in order to use the recycling bin, remove orphans and the no hard link options

Here we have `/mnt/user/data/torrents/` mapped to `/data/torrents/` furthermore in the config file associated with it the root_dir is mapped to `/data/torrents/`

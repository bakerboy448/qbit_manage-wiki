
# Overview
The script utilizes a YAML config file to load information to connect to the various APIs you can connect with.

By default, the script looks at /config/config.yml for the Configuration File unless otherwise specified.

A template Configuration File can be found in the repo [config/config.yml.sample](https://github.com/StuffAnThings/qbit_manage/blob/master/config/config.yml.sample).

**WARNING** <br>
> As this software is constantly evolving and this wiki might not be up to date the sample shown here might not might not be current. Please referr to the repo for the most current version.

# Config File

**Sample**
```yaml
# This is an example configuration file that documents all the options.
# It will need to be modified for your specific use case.

# qBittorrent parameters
qbt:
  host: "localhost:8080"
  user: "username"
  pass: "password"

directory:
  # Do not remove these
  # Cross-seed var: </your/path/here/> # Output directory of cross-seed
  # root_dir var: </your/path/here/> # Root downloads directory used to check for orphaned files, noHL, and RecycleBin.
  # <OPTIONAL> remote_dir var: </your/path/here/> # Path of docker host mapping of root_dir.
  # Must be set if you're running qbit_manage locally and qBittorrent/cross_seed is in a docker
  cross_seed: "/your/path/here/"
  root_dir: "/data/torrents/"
  remote_dir: "/mnt/user/data/torrents/"

# Category & Path Parameters
cat:
  # <Category Name> : <save_path> # Path of your save directory. Can be a keyword or full path
  movies: "/data/torrents/Movies"
  tv: "TV"

# Tag Parameters
tags:
  # <Tracker URL Keyword>:    # <MANDATORY> This is the keyword in the tracker url
  # <MANDATORY> Set tag name
  #   tag: <Tag Name>
  # <OPTIONAL> Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading. -2 means the global limit should be used, -1 means no limit.
  #   max_ratio: 5.0
  # <OPTIONAL> Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding. -2 means the global limit should be used, -1 means no limit.
  #   max_seeding_time: 129600
  # <OPTIONAL> Will limit the upload speed KiB/s (KiloBytes/second) (-1 sets the limit to infinity)
  #   limit_upload_speed: 150
  # <OPTIONAL> Set this to the notifiarr react name. This is used to add indexer reactions to the notifications sent by Notifiarr
  #   notifiarr: <notifiarr indexer>
  animebytes.tv:
    tag: AnimeBytes
    notifiarr: animebytes
  avistaz:
    tag: Avistaz
    max_ratio: 5.0
    max_seeding_time: 129600
    limit_upload_speed: 150
    notifiarr: avistaz
  beyond-hd:
    tag: Beyond-HD
    notifiarr: beyondhd
  blutopia:
    tag: Blutopia
    notifiarr: blutopia
  cartoonchaos:
    tag: CartoonChaos
  digitalcore:
    tag: DigitalCore
    max_ratio: 5.0
    notifiarr: digitalcore
  gazellegames:
    tag: GGn
    limit_upload_speed: 150
  hdts:
    tag: HDTorrents
    max_seeding_time: 129600
  landof.tv:
    tag: BroadcasTheNet
    notifiarr: broadcasthenet
  myanonamouse:
    tag: MaM
  passthepopcorn:
    tag: PassThePopcorn
    notifiarr: passthepopcorn
  privatehd:
    tag: PrivateHD
    notifiarr:
  tleechreload:
    tag: TorrentLeech
    notifiarr: torrentleech
  torrentdb:
    tag: TorrentDB
    notifiarr: torrentdb
  torrentleech:
    tag: TorrentLeech
    notifiarr: torrentleech
  tv-vault:
    tag: TV-Vault

# Tag Movies/Series that are not hard linked
nohardlinks:
  # Mandatory to fill out directory parameter above to use this function (root_dir/remote_dir)
  # This variable should be set to your category name of your completed movies/completed series in qbit. Acceptable variable can be any category you would like to tag if there are no hardlinks found
  movies-completed:
    # <OPTIONAL> exclude_tags var: Will exclude the following tags when searching through the category.
    exclude_tags:
      - Beyond-HD
      - AnimeBytes
      - MaM
    # <OPTIONAL> cleanup var: WARNING!! Setting this as true Will remove and delete contents of any torrents that are in paused state and has the NoHL tag
    cleanup: false
    # <OPTIONAL> max_ratio var: Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading
    max_ratio: 4.0
    # <OPTIONAL> seeding time var: Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding
    max_seeding_time: 86400

  # Can have additional categories set with separate ratio/seeding times defined.
  series-completed:
    # <OPTIONAL> exclude_tags var: Will exclude the following tags when searching through the category.
    exclude_tags:
      - Beyond-HD
      - BroadcasTheNet
    # <OPTIONAL> cleanup var: WARNING!! Setting this as true Will remove and delete contents of any torrents that are in paused state and has the NoHL tag
    cleanup: false
    # <OPTIONAL> max_ratio var: Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading
    max_ratio: 4.0
    # <OPTIONAL> seeding time var: Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding
    max_seeding_time: 86400

# Recycle Bin method of deletion will move files into the recycle bin (Located in /root_dir/.RecycleBin) instead of directly deleting them in qbit
# By default the Recycle Bin will be emptied on every run of the qbit_manage script if empty_after_x_days is defined.
recyclebin:
  enabled: true
  # <OPTIONAL> empty_after_x_days var: Will automatically remove all files and folders in recycle bin after x days. (Checks every script run)
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
    - "**/*.!qB"

# Apprise integration with webhooks
# Leave Empty/Blank to disable
apprise:
  # Mandatory to fill out the url of your apprise API endpoint
  api_url: http://apprise-api:8000
  # Mandatory to fill out the notification url/urls based on the notification services provided by apprise. https://github.com/caronc/apprise/wiki
  notify_url:

# Notifiarr integration with webhooks
# Leave Empty/Blank to disable
notifiarr:
  # Mandatory to fill out API Key
  apikey: ####################################
  # <OPTIONAL> Set to a unique value (could be your username on notifiarr for example)
  instance:

# Webhook notifications:
# Possible values:
# Set value to notifiarr if using notifiarr integration
# Set value to apprise if using apprise integration
# Set value to a valid webhook URL
# Leave Empty/Blank to disable
webhooks:
  error: https://mywebhookurl.com/qbt_manage
  run_start: notifiarr
  run_end: apprise
  function:
    cross_seed: https://mywebhookurl.com/qbt_manage
    recheck: notifiarr
    cat_update: apprise
    tag_update: notifiarr
    rem_unregistered: notifiarr
    rem_orphaned: notifiarr
    tag_nohardlinks: notifiarr
    empty_recyclebin: notifiarr
```
# **List of variables**<br>

## **qbt:**
---
This section defines your qBittorrent instance.

| Variable | Definition | Required
| :------------ | :------------  | :------------
| `host`| IP address of your qB installation.|<center>✅</center>
| `user` | The user name of your qB's webUI.| <center>❌</center>
| `pass` | Thee password of your qB's webUI.| <center>❌</center>

<br>

## **directory:**
---
This section defines the directories that qbit_manage will be looking into for various parts of the script.

| Variable | Definition | Required
| :------------ | :------------  | :------------
| `cross_seed` | Output directory of cross-seed, originally the application [cross-seed](https://github.com/mmgoodnow/cross-seed) was incapable of injecting cross-seed torrent into qB, this was built to inject them for the applicaiton. This is no longer required if you're using injects with that software. However, you can find othere uses for this as it is more of a watch directory now.| QBT_CROSS_SEED
| `root_dir` | Root downloads directory used to check for orphaned files, noHL, and remove unregistered. This directory is where you place all your downloads. This will need to be how qB views the directory where it places the downloads. This is required if you're using qbit_managee and/or qBittorrent within a container.| QBT_REM_ORPHANED / QBT_TAG_NOHARDLINKS / QBT_REM_UNREGISTERED
| `remote_dir` | Path of docker host mapping of root_dir, this must be set if you're running qbit_manage locally (not required if running qbit_manage in a container) and qBittorrent/cross_seed is in a docker. Essentially this is where your downloads are being kept on the host. |<center>❌</center>

## **cat:**
---
This section defines the categories that you are currently using and the path's that are associated with them.<br>
> **NOTE** ALL categories must be defined, if it is in your qBit, then it **MUST** be defined here, if not the script will throw errors.

| Configuration | Definition | Required
| :------------ | :------------  | :------------
| `key`| Name of the category|<center>✅</center>
| `value` | Save Path of the category| <center>✅</center>

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

## **tags:**
---
This section defines the tags that will be used, the way this script is set up is to use tags based upon the tracker's URL.<br>
| Configuration | Definition | Required
| :------------ | :------------  | :------------
| `key`| Tracker URL Keyword |<center>✅</center>

| Variable | Definition | Default Values| Required
| :------------ | :------------  | :------------ | :------------
| `tag` | The Tracker Tag| Tracker URL | <center>✅</center>
| `max_ratio` | Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading. (`-2` : Global Limit , `-1` : No Limit) | None | <center>❌</center>
| `max_seeding_time` | Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding. (`-2` : Global Limit , `-1` : No Limit)| None |  <center>❌</center>
| `limit_upload_speed` | Will limit the upload speed KiB/s (KiloBytes/second) (`-1` : No Limit)| None | <center>❌</center>


If either max_ratio or max_seeding_time is set to `-2` then the the global share limits will be used, `-1` then no share limits will be used.

If you are unsure what key word to use. Simply select a torrent within qB and down at the bottom you should see a tab that says `Trackers` within the list that is populated there are ea list of trackers that are associateed with this torrent, select a key word from there and add it to the config file. Make sure this key word is unique enough that the script will not get connfused with any other tracker.

## **nohardlinks:**
---
Hardlinking data allows you to have your data in both the torrent directory and your media direectory at the same time without using double the amount of data. 

If you're needing information regarding hardlinks here are some excellent resources. <br>

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/AMcHsQJ7My0/0.jpg)](https://www.youtube.com/watch?v=AMcHsQJ7My0)

* [Trash-Guides: Hardlinks and Instant Moves (Atomic-Moves)](https://trash-guides.info/Hardlinks/Hardlinks-and-Instant-Moves/) 
* [Wikipedia: Hardlinks](https://en.wikipedia.org/wiki/Hard_link)

Mandatory to fill out [directory parameter](#directory) above to use this function (root_dir/remote_dir)   
Beyond this you'll need to use one of the [categories](#cat) above as the key, and should be pointed to the `completed-` directory of your torrents.

| Configuration | Definition | Required
| :------------ | :------------  | :------------
| `key`| Category name of your completed movies/completed series in qbit. |<center>✅</center>

| Variable | Definition | Default Values| Required
| :------------ | :------------  | :------------ | :------------
| `cleanup` | True = Remove the non-hardlinked torrent data/contents once share limits have been met, False = Use (Automatic Torrent Management) to handle seeding limits | False | <center>✅</center>
| `max_ratio` | Will set the torrent Maximum share ratio until torrent is stopped from seeding/uploading. (`-2` : Global Limit , `-1` : No Limit) | None | <center>❌</center>
| `max_seeding_time` | Will set the torrent Maximum seeding time (min) until torrent is stopped from seeding. (`-2` : Global Limit , `-1` : No Limit)| None |  <center>❌</center>
| `limit_upload_speed` | Will limit the upload speed KiB/s (KiloBytes/second) (`-1` : No Limit)| None | <center>❌</center>
| `exclude_tags` | List of tags to exclude from the check. This is useful to exclude certain trackers from being scanned for hardlinking purposes | None | <center>❌</center>

## **recyclebin:**
---
  Recycle Bin method of deletion will move files into the recycle bin (Located in /root_dir/.RecycleBin) instead of directly deleting them in qbit. 

  This is very useful if you're hesitant about using this script to delete information off your system hingswithout first checking it. Plus with the ability of this script to remove trumped/unregistered torrents there is a very small chance that something may happen to cause the script to go to town on  your library. With the recycling bin in place your data is secure (unless the bin is emptied before this issue is caught). All you'd need to do to recover would be to place the data back into the correct directory, redownload the torrent file from the tracker and recheck the torrent with the tracker from the UI. 

| Variable  | Definition | Default Values| Required
| :------------ | :------------  | :------------ | :------------
|`enable`| `true` or `false` | `true` | <center>✅</center>
| `empty_after_x_days` | Will delete Recycle Bin contents if the files have been in the Recycle Bin for more than x days. (Uses date modified to track the time)| None | <center>❌</center>

> Note: The more time you place for the `empty_after_x_days:` variable the better, allowing you more time to catch any mistakes by the script. If the variable is set to `0` it will delete contents immediately after every script run. If the variable is not set it will never delete the contents of the Recycle Bin.

## **orphaned:**
---
This section allows for the exclusion of certain files from being considered "Orphaned"

This is handy when you have automatically generated files that certain OSs decide to make. `.DS_Store` Is a primary example, for those who use MacOS.

| Variable  | Definition | Default Values| Required
| :------------ | :------------  | :------------ | :------------
|`exclude_patterns`| List of [patterns](https://commandbox.ortusbooks.com/usage/parameters/globbing-patterns) to exclude certain files from orphaned | None | <center>❌</center>



<br><br><br><br><br>
# *---FEATURE CURRENTLY IN DEVELOP BRANCH---*
## **apprise:**
---
[Apprise](https://github.com/caronc/apprise) integration is used in conjunction with webhooks to allow notifications via apprise-api.

| Variable  | Definition | Default Values| Required
| :------------ | :------------  | :------------ | :------------
|`api_url`| Apprise API Endpoint URL | N/A | <center>✅</center>
|`notify_url`| [Notification Services URL](https://github.com/caronc/apprise/wiki)  | N/A | <center>✅</center>

## **notifiarr:**
---
[Notifiarr](https://notifiarr.com/) integration is used in conjunction with webhooks to allow discord notifications via Notifiarr.

| Variable  | Definition | Default Values| Required
| :------------ | :------------  | :------------ | :------------
|`apikey`| Notifiarr API Key | N/A | <center>✅</center>

## **webhooks:**
---
Provide webhook notifications based on event triggers

| Variable  | Notification Sent | Default Values| Required
| :------------ | :------------  | :------------ | :------------
|[error](#error-notifications)| When errors occur during the run | N/A | <center>❌</center>
|[run_start](#run-start-notifications)| At the beginning of every run  | N/A | <center>❌</center>
|[run_end](#run-end-notifications)| At the end of every run | N/A | <center>❌</center>
|[cross_seed](#cross-seed-notifications)| During the cross-seed function | N/A | <center>❌</center>
|[recheck](#recheck-notifications)| During the recheck function| N/A | <center>❌</center>
|[cat_update](#category-update-notifications)| During the category update function | N/A | <center>❌</center>
|[tag_update](#tag-update-notifications)| During the tag update function | N/A | <center>❌</center>
|[rem_unregistered](#remove-unregistered-torrents-notifications)| During the removing unregistered torrents function | N/A | <center>❌</center>
|[rem_orphaned](#remove-orphaned-files-notifications)| During the removing orphaned function| N/A | <center>❌</center>
|[tag_nohardlinks](#tag-no-hardlinks-notifications)| During the tag no hardlinks function | N/A | <center>❌</center>
|[empty_recyclebin](#empty-recycle-bin-notifications)| When files are deleted from the Recycle Bin | N/A | <center>❌</center>


### **Error Notifications**
Payload will be sent on any errors
```yaml
{
  "function": "run_error",     // Webhook Trigger keyword
  "title": str,                // Title of the Payload
  "body": str,                 // Error Message of the Payload
  "critical": bool,            // Critical Error
  "type": str                  // severity of error
}
```
### **Run Start Notifications**
Payload will be sent at the start of the run
```yaml
{
  "function": "run_start",     // Webhook Trigger keyword
  "title": str,                // Title of the Payload
  "body": str,                 // Message of the Payload
  "start_time": str,           // Time Run is started Format "YYYY-mm-dd HH:MM:SS"
  "dry_run": bool              // Dry-Run
}
```
### **Run End Notifications**
Payload will be sent at the end of the run
```yaml
{
  "function": "run_end",                    // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "start_time": str,                        // Time Run started Format "YYYY-mm-dd HH:MM:SS"
  "end_time": str,                          // Time Run ended Format "YYYY-mm-dd HH:MM:SS"
  "run_time": str,                          // Total Run Time "H:MM:SS"
  "torrents_added": int,                    // Total Torrents Added
  "torrents_deleted": int,                  // Total Torrents Deleted
  "torrents_deleted_and_contents": int,     // Total Torrents + Contents Deleted
  "torrents_resumed": int,                  // Total Torrents Resumed
  "torrents_rechecked": int,                // Total Torrents Rechecked
  "torrents_categorized": int,              // Total Torrents Categorized
  "torrents_tagged": int,                   // Total Torrents Tagged
  "remove_unregistered": int,               // Total Unregistered Torrents Removed
  "orphaned_files_found": int,              // Total Orphaned Files Found
  "torrents_tagged_no_hardlinks": int,      // Total noHL Torrents Tagged
  "torrents_untagged_no_hardlinks": int,    // Total noHL Torrents untagged
  "files_deleted_from_recyclebin": int      // Total Files Deleted from Recycle Bin
}
```

### **Cross-Seed Notifications**
Payload will be sent when adding a cross-seed torrent to qBittorrent if the original torrent is complete
```yaml
{
  "function": "cross_seed",             // Webhook Trigger keyword
  "title": str,                         // Title of the Payload
  "body": str,                          // Message of the Payload
  "torrent_name": str,                  // Torrent Name
  "torrent_category": str,              // Torrent Category
  "torrent_save_path": str,             // Torrent Download directory
  "torrent_add_tag": "cross-seed",      // Total Torrents Added
}
```
Payload will be sent when there are existing torrents found that are missing the cross-seed tag
```yaml
{
  "function": "tag_cross_seed",         // Webhook Trigger keyword
  "title": str,                         // Title of the Payload
  "body": str,                          // Message of the Payload
  "torrent_name": str,                  // Torrent Name
  "torrent_category": str,              // Torrent Category
  "torrent_add_tag": "cross-seed",      // Tag Added
}
```

### **Recheck Notifications**
Payload will be sent when rechecking/resuming a torrent that is paused
```yaml
{
  "function": "recheck",             // Webhook Trigger keyword
  "title": str,                      // Title of the Payload
  "body": str,                       // Message of the Payload
  "torrent_name": str,               // Torrent Name
  "torrent_category": str,           // Torrent Category
  "torrent_tracker": str,            // Torrent Tracker URL
  "notifiarr_indexer": str,          // Notifiarr React name/id for indexer
}
```

### **Category Update Notifications**
Payload will be sent when updating torrents with missing category
```yaml
{
  "function": "cat_update",          // Webhook Trigger keyword
  "title": str,                      // Title of the Payload
  "body": str,                       // Message of the Payload
  "torrent_name": str,               // Torrent Name
  "torrent_new_cat": str,            // New Torrent Category
  "torrent_tracker": str,            // Torrent Tracker URL
  "notifiarr_indexer": str,          // Notifiarr React name/id for indexer
}
```

### **Tag Update Notifications**
Payload will be sent when updating torrents with missing tag
```yaml
{
  "function": "tag_update",                 // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "torrent_new_tag": str,                   // New Torrent Tag
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
  "torrent_max_ratio": float,               // Set the Max Ratio Share Limit
  "torrent_max_seeding_time": int,          // Set the Max Seeing Time (min) Share Limit
  "torrent_limit_upload_speed": int         // Set the the torrent upload speed limit
}
```

### **Remove Unregistered Torrents Notifications**
Payload will be sent when Unregistered Torrents are found
```yaml
{
  "function": "rem_unregistered",          // Webhook Trigger keyword
  "title": str,                            // Title of the Payload
  "body": str,                             // Message of the Payload
  "torrent_name": str,                     // Torrent Name
  "torrent_category": str,                 // Torrent Category
  "torrent_status": str,                   // Torrent Tracker Status message
  "torrent_tracker": str,                  // Torrent Tracker URL
  "notifiarr_indexer": str,                // Notifiarr React name/id for indexer
}
```
Payload will be sent when Potential Unregistered Torrents are found
```yaml
{
  "function": "potential_rem_unregistered",          // Webhook Trigger keyword
  "title": str,                                      // Title of the Payload
  "body": str,                                       // Message of the Payload
  "torrent_name": str,                               // Torrent Name
  "torrent_category": str,                           // Torrent Category
  "torrent_add_tag": "issue",                        // Tag Added
  "torrent_status": str,                             // Torrent Tracker Status message
  "torrent_tracker": str,                            // Torrent Tracker URL
  "notifiarr_indexer": str,                          // Notifiarr React name/id for indexer
}
```

### **Remove Orphaned Files Notifications**
Payload will be sent when Orphaned Files are found and moved into the orphaned folder
```yaml
{
  "function": "rem_orphaned",          // Webhook Trigger keyword
  "title": str,                        // Title of the Payload
  "body": str,                         // Message of the Payload
  "orphaned_files": list,              // List of orphaned files
  "orphaned_directory": str,           // Folder path where orphaned files will be moved to
  "total_orphaned_files": int,         // Total number of orphaned files found
}
```
### **Tag No Hardlinks Notifications**
Payload will be sent when no hard links are found for any files in a particular torrent
```yaml
{
  "function": "tag_nohardlinks",            // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "torrent_add_tag": 'noHL',                // Add `noHL` to Torrent Tags
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
  "torrent_max_ratio": float,               // Set the Max Ratio Share Limit
  "torrent_max_seeding_time": int,          // Set the Max Seeing Time (min) Share Limit
  "torrent_limit_upload_speed": int         // Set the the torrent upload speed limit
}
```
Payload will be sent when hard links are found for any torrents that were previously tagged with `noHL`
```yaml
{
  "function": "untag_nohardlinks",          // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "torrent_remove_tag": 'noHL',             // Remove `noHL` from Torrent Tags
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
  "torrent_max_ratio": float,               // Set the Max Ratio Share Limit
  "torrent_max_seeding_time": int,          // Set the Max Seeing Time (min) Share Limit
  "torrent_limit_upload_speed": int         // Set the the torrent upload speed limit
}
```

Payload will be sent when `cleanup` flag is set to true and `noHL` torrent meets share limit criteria.
```yaml
{
  "function": "cleanup_tag_nohardlinks",    // Webhook Trigger keyword
  "title": str,                             // Title of the Payload
  "body": str,                              // Message of the Payload
  "torrent_name": str,                      // Torrent Name
  "torrent_category": str,                  // Torrent Category
  "cleanup": True,                          // Cleanup flag
  "torrent_tracker": str,                   // Torrent Tracker URL
  "notifiarr_indexer": str,                 // Notifiarr React name/id for indexer
}
```
### **Empty Recycle Bin Notifications**
Payload will be sent when files are deleted from the Recycle Bin folder
```yaml
{
  "function": "empty_recyclebin",         // Webhook Trigger keyword
  "title": str,                           // Title of the Payload
  "body": str,                            // Message of the Payload
  "files": list,                          // List of files that were deleted from the Recycle Bin
  "empty_after_x_days": str,              // Number of days that the files will be kept in the Recycle Bin
  "size_in_bytes": int,                   // Total number of bytes deleted from the Recycle Bin
}
```
# About

Telegram bot for organizing raids and sharing quests in Pokemon Go. Developers are welcome to join https://t.me/PokemonBotSupport

# Screenshots

#### Example raid poll with the ex-raid notice:
![Example raid poll](/screenshots/raid-poll-example-with-ex-raid-message.png?raw=true "Example raid poll")

#### Example raid poll showing the users teams & levels (if they've set it), status (late, cancel and done), attend times and preferred pokemons (if raid boss is still a raid egg) the users voted for:
![Example raid poll](/screenshots/raid-poll-example-with-late.png?raw=true "Example raid poll")
![Example raid poll](/screenshots/raid-poll-example-with-cancel.png?raw=true "Example raid poll")
![Example raid poll](/screenshots/raid-poll-example-with-done.png?raw=true "Example raid poll")

#### Example quest with reward:
![Example quest](/screenshots/quest-example.png?raw=true "Example quest")

#### Example quest with pokemon encounter:
![Example quest](/screenshots/quest-example-with-pokemon-encounter.png?raw=true "Example quest")

# Installation and configuration

## Webserver

Preferrably apache2 with php7 and https certificate ( https://www.letsencrypt.org )

## Bot token

Start chat with https://t.me/BotFather and create bot token.

Bot Settings: 
 - Enable Inline mode
 - Allow Groups
   - Group Privacy off

## Database

Create database named for your bot ID (first part of your Telegram bot token).

Set database password to second part of your Telegram bot token.

Only allow localhost access.

Import `raid-pokemon-bot.sql` as default DB structure and `raid-boss-pokedex.sql` for the raid bosses. You can find these files in the sql folder.

Command DB structure: `mysql -u USERNAME -p DATABASENAME < raid-pokemon-bot.sql`

Command raid bosses: `mysql -u USERNAME -p DATABASENAME < raid-boss-pokedex.sql`

## Config

Copy config.php.example to config.php and edit (values explained further).

Enter the details for the database connection to the config.php file.

## Log files

Create log dir, e.g. /var/log/tg-bots/ and set it writeable by webserver.

Edit config.php and set `CONFIG_LOGFILE`.

Use https://www.miniwebtool.com/sha512-hash-generator/ and set `CONFIG_HASH` to hashed value of your bot token (preferably lowercase).

## Proxy

In case you are running the bot behind a proxy server, set `CURL_USEPROXY` to `true`.

Add the proxy server address and port at `CURL_PROXYSERVER`.

Authentication against the proxy server by username and password is currently not supported.

## Webhooks

Set Telegram webhook via webhook.html, e.g. https://your-hostname/bot-dir/webhook.html

## Languages

You can set several languages for the bot. Available languages are (A-Z):
 - DE (German)
 - EN (English)
 - NL (Dutch)
 - PT-BR (Brazilian Portugese)

Set `LANGUAGE` for the prefered language the bot will answer users when they chat with them. Leave blank that the bot will answer in the users language. If the users language is not supported, e.g. ZH-CN (Chinese), the bot will always use EN (English) as fallback language.

Set `RAID_POLL_LANGUAGE` to the prefered language for raid polls.

Set `QUEST_LANGUAGE` to the prefered language for quests.

So if you want to have the bot communication based on the users language, the raid polls in German and the quests in Dutch for example:

`define('LANGUAGE', '');`

`define('RAID_POLL_LANGUAGE', 'DE');`

`define('QUEST_LANGUAGE', 'NL');`

## Timezone and Google maps API

Set `TIMEZONE` to the timezone you wish to use for the bot. Predefined value from the example config is "Europe/Berlin".

Optionally you can you use Google maps API to lookup addresses of gyms based on latitude and longitude

Therefore get a Google maps API key and set it as `GOOGLE_API_KEY` in your config.

To get a new API key, navigate to https://console.developers.google.com/apis/credentials and create a new API project, e.g. raid-telegram-bot

Once the project is created select "API key" from the "Create credentials" dropdown menu - a new API key is created.

After the key is created, you need to activate it for both: Geocoding and Timezone API

Therefore go to "Dashboard" on the left navigation pane and afterwards hit "Enable APIs and services" on top of the page.

Search for Geocoding and Timezone API and enable them. Alternatively use these links to get to Geocoding and Timezone API services:

https://console.developers.google.com/apis/library/timezone-backend.googleapis.com

https://console.developers.google.com/apis/library/geocoding-backend.googleapis.com

Finally check the dashboard again and make sure Google Maps Geocoding API and Google Maps Time Zone API are listed as enabled services.

## Raid creation

There are several options to customize the creation of raid polls:

Set `RAID_VIA_LOCATION` to true to allow raid creation from a location shared with the bot.

Set `RAID_EGG_DURATION` to the maximum amount of minutes a user can select for the egg hatching phase.

Set `RAID_POKEMON_DURATION_SHORT` to the maximum amount of minutes a user can select as raid duration for already running/active raids.

Set `RAID_POKEMON_DURATION_LONG` to the maximum amount of minutes a user can select as raid duration for not yet hatched raid eggs.

Set `RAID_DURATION_CLOCK_STYLE` to customize the default style for the raid start time selection. Set to true, the bot will show the time in clocktime style, e.g. "18:34" as selection when the raid will start. Set to false the bot will show the time until the raid starts in minutes, e.g. "0:16" (similar to the countdown in the gyms). Users can switch between both style in the raid creation process.

## Raid times

There are several options to configure the times related to the raid polls:

Set `RAID_LOCATION` to true to send back the location as message in addition to the raid poll.

Set `RAID_SLOTS` to the amount of minutes which shall be between the voting slots.

Set `RAID_LAST_START` to the minutes for the last start option before the a raid ends.

Set `RAID_ANYTIME` to true to allow attendance of the raid at any time. If set to false, users have to pick a specific time.

## Raid poll design and layout

There are several options to configure the design and layout of the raid polls:

Set `RAID_VOTE_ICONS` to true to show the icons for the status vote buttons.

Set `RAID_VOTE_TEXT` to true to show the text for the status vote buttons.

Set `RAID_LATE_MSG` to true to enable the message hinting that some participants are late.

Set `RAID_LATE_TIME` to the amount of minutes the local community will may be wait for the late participants.

## Raid sharing

You can share raid polls with any chat in Telegram via a share button.

Sharing raid polls can be restricted, so only moderators or users or both can be allowed to share a raid poll.

Therefore it is possible, via a comma-separated list, to specify the chats the raid polls can be shared with.

For the ID of a chat either forward a message from the chat to a bot like @RawDataBot or search the web for another method ;)

A few examples:

#### Restrict sharing for moderators and users to chats -100111222333 and -100444555666

`define('SHARE_RAID_MODERATORS', false);`

`define('SHARE_RAID_USERS', false);`

`define('SHARE_RAID_CHATS', '-100111222333,-100444555666');`

#### Allow moderators to share with any chat, restrict sharing for users to chat -100111222333

`define('SHARE_RAID_MODERATORS', true);`

`define('SHARE_RAID_USERS', false);`

`define('SHARE_RAID_CHATS', '-100111222333');`

## Raid overview

The bot allows you to list all raids which got shared with one or more chats as a single raid overview message to quickly get an overview of all raids which are currently running and got shared in each chat. You can view and share raid overviews via the /list command - but only if some raids are currently active and if these active raids got shared to any chats!

To keep this raid overview always up to date when you have it e.g. pinned inside your raid channel, you can setup a cronjob that updates the message by calling the overview_refresh module.

You can either refresh all shared raid overview messages by calling

`curl -k -d '{"callback_query":{"data":"0:overview_refresh:0"}}' https://localhost/bot_subdirectory/index.php?apikey=111111111:AABBccddEEFFggHHiijjKKLLmmnnOOPPqq`

or just refresh the raid overview message you've shared with a specific chat (e.g. -100112233445):

`curl -k -d '{"callback_query":{"data":"0:overview_refresh:-100112233445"}}' https://localhost/bot_subdirectory/index.php?apikey=111111111:AABBccddEEFFggHHiijjKKLLmmnnOOPPqq`

To delete a shared raid overview message you can use the /list command too.

With the `RAID_PIN_MESSAGE` in config.php you can add a custom message inside the raid overview message which will be attached to the bottom of the raid overview message.

## Quest creation

There are several options to customize the creation of quests:

Set `QUEST_VIA_LOCATION` to true to allow quest creation from a location shared with the bot.

Set `QUEST_LOCATION` to true to send back the location as message in addition to the quest.

Set `QUEST_STOPS_RADIUS` to the amount in meters the bot will search for pokestops around the location shared with the bot.

Set `QUEST_HIDE_REWARDS` to true to hide specific reward types, e.g. berries or revives. Specify the reward types you want to hide in `QUEST_HIDDEN_REWARDS` separated by comma. 

Example to hide pokeballs, berries, potions and revives: `define('QUEST_HIDDEN_REWARDS', '2,3,8,10');`

Every ID/number for all the available reward types:

| Reward ID | Reward type |
|-----------|-------------|
| 1         | Pokemon     |
| 2         | Pokeball    |
| 3         | Berry       |
| 4         | Stardust    |
| 5         | Rare candy  |
| 7         | Fast TM     | 
| 7         | Charged TM  | 
| 8         | Potion      | 
| 9         | XP          | 
| 10        | Revive      | 


## Cleanup

The bot features an automatic cleanup of telegram raid poll messages as well as cleanup of the database (attendance and raids tables). Also quests can be cleaned up from telegram and the database.

To activate cleanup you need to change the config and create a cronjob to trigger the cleanup process as follows:

Set the `CLEANUP` in the config to `true` and define a cleanup secret/passphrase under `CLEANUP_SECRET`.

Activate the cleanup of telegram messages and/or the database for raids by setting `CLEANUP_RAID_TELEGRAM` / `CLEANUP_RAID_DATABASE` to true and for quests via `CLEANUP_QUEST_TELEGRAM` / `CLEANUP_QUEST_DATABASE`.

For raids: Specify the amount of minutes which need to pass by after raid has ended before the bot executes the cleanup. Times are in minutes in `CLEANUP_RAID_TIME_TG` for telegram cleanup and `CLEANUP_RAID_TIME_DB` for database cleanup. The value for the minutes of the database cleanup `CLEANUP_RAID_TIME_DB` must be greater than then one for telegram cleanup `CLEANUP_RAID_TIME_TG`. Otherwise cleanup will do nothing and exit due to misconfiguration!

For quests: The cleanup process will automatically detect old quests which are not from the present day.

Finally set up a cronjob to trigger the cleanup. You can also trigger telegram / database cleanup per cronjob: For no cleanup use 0, for cleanup use 1 and to use your config file use 2 or leave "telegram" and "database" out of the request data array. Please make sure to always specify the cleanup type which can be `raid` or `quest`.

A few examples for raids - make sure to replace the URL with yours:

#### Cronjob using cleanup values from config.php for raid polls: Just the secret without telegram/database OR telegram = 2 and database = 2

`curl -k -d '{"cleanup":{"type":"raid","secret":"your-cleanup-secret/passphrase"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

OR

`curl -k -d '{"cleanup":{"type":"raid","secret":"your-cleanup-secret/passphrase","telegram":"2","database":"2"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

#### Cronjob to clean up telegram raid poll messages only: telegram = 1 and database = 0 

`curl -k -d '{"cleanup":{"type":"raid","secret":"your-cleanup-secret/passphrase","telegram":"1","database":"0"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

#### Cronjob to clean up telegram raid poll messages and database: telegram = 1 and database = 1

`curl -k -d '{"cleanup":{"type":"raid","secret":"your-cleanup-secret/passphrase","telegram":"1","database":"1"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

#### Cronjob to clean up database and maybe telegram raid poll messages (when specified in config): telegram = 2 and database = 1

`curl -k -d '{"cleanup":{"type":"raid","secret":"your-cleanup-secret/passphrase","telegram":"2","database":"1"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

A few examples for quests - make sure to replace the URL with yours:

#### Cronjob using cleanup values from config.php for quests: Just the secret without telegram/database OR telegram = 2 and database = 2

`curl -k -d '{"cleanup":{"type":"quest","secret":"your-cleanup-secret/passphrase"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

OR

`curl -k -d '{"cleanup":{"type":"quest","secret":"your-cleanup-secret/passphrase","telegram":"2","database":"2"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

#### Cronjob to clean up telegram quest messages only: telegram = 1 and database = 0 

`curl -k -d '{"cleanup":{"type":"quest","secret":"your-cleanup-secret/passphrase","telegram":"1","database":"0"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

#### Cronjob to clean up database and telegram quest messages: telegram = 1 and database = 1

`curl -k -d '{"cleanup":{"type":"quest","secret":"your-cleanup-secret/passphrase","telegram":"1","database":"1"}}' https://localhost/index.php?apikey=111111111:AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPP123`

# Access permissions

## Public access

When no telegram id, group, supergroup or channel is specified in `BOT_ADMINS` and/or `BOT_ACCESS`, the bot will allow everyone to use it (public access).

Example for public access: `define('BOT_ACCESS', '');`

## Restricted access

With BOT_ADMINS and BOT_ACCESS being used to restrict access, there are several access roles / types. When you do not configure BOT_ACCESS, everyone will have access to your bot (public access).  

Set `BOT_ADMINS` and `BOT_ACCESS` to id (-100123456789) of one or multiple by comma separated individual telegram chat names/ids, groups, supergroups or channels.

Please note, when you're setting groups, supergroups or channels only administrators (not members!) from these chats will gain access to the bot! So make sure this requirement is fulfilled or add their individual telegram usernames/ids instead.

Example for restricted access:  
`define('BOT_ADMINS', '111222333,111555999');`

`define('BOT_ACCESS', '111222333,-100224466889,-100112233445,111555999');`

## Access overview

With your `MAINTAINER_ID` and as a member of `BOT_ADMINS` you have the permissions to do anything. **For performance improvements, it's recommended to add the MAINTAINER and all members of BOT_ADMINS as moderator via /mods command!** 

As a member of `BOT_ACCESS` you can create raid polls, update your own raid polls' pokemon and change the gym team of your last raid poll. `BOT_ACCESS` members who are moderators too, can also change the gym name and update pokemon from other users raid polls. Not that members of `BOT_ACCESS` are not allowed to create polls for ex-raids, only the `MAINTAINER_ID` and the `BOT_ADMINS` have the right to create them.

Telegram Users can only vote on raid polls, but have no access to other bot functions (unless you configured it for public access).


| Access:   |            |                                  | MAINTAINER_ID | BOT_ADMINS | BOT_ACCESS | BOT_ACCESS | Telegram |
|-----------|------------|----------------------------------|---------------|------------|------------|------------|----------|
| Database: |            |                                  |               |            | Moderator  | User       | User     |
|           | **Area**   | **Action and /command**          |               |            |            |            |          |
|           | Raid poll  | Vote                             | Yes           | Yes        | Yes        | Yes        | Yes      |
|           |            | Create `/start`, `/raid`, `/new` | Yes           | Yes        | Yes        | Yes        |          |
|           |            | Create ex-raid `/start`          | Yes           | Yes        |            |            |          |
|           |            | List `/list`                     | Yes           | Yes        | Yes        | Yes        |          |
|           |            | Overview `/list`                 | Yes           | Yes        |            |            |          |
|           |            | Delete ALL raid polls `/delete`  | Yes           | Yes        | Yes        |            |          |
|           |            | Delete OWN raid polls `/delete`  | Yes           | Yes        | Yes        | Yes        |          |
|           |            |                                  |               |            |            |            |          |
|           | Pokemon    | ALL raid polls `/pokemon`        | Yes           | Yes        | Yes        |            |          |
|           |            | OWN raid polls `/pokemon`        | Yes           | Yes        | Yes        | Yes        |          |
|           |            |                                  |               |            |            |            |          |
|           | Gym        | Name `/gym`                      | Yes           | Yes        | Yes        |            |          |
|           |            | Team `/team`                     | Yes           | Yes        | Yes        | Yes        |          |
|           |            |                                  |               |            |            |            |          |
|           | Moderators | List `/mods`                     | Yes           | Yes        |            |            |          |
|           |            | Add `/mods`                      | Yes           | Yes        |            |            |          |
|           |            | Delete `/mods`                   | Yes           | Yes        |            |            |          |
|           |            |                                  |               |            |            |            |          |
|           | Pokedex    | Manage raid pokemon `/pokedex`   | Yes           | Yes        |            |            |          |
|           |            |                                  |               |            |            |            |          |
|           | Quests     | Create `/quest`                  | Yes           | Yes        | Yes        | Yes        |          |
|           |            | List `/listquest`                | Yes           | Yes        | Yes        | Yes        |          |
|           |            | Delete ALL quests `/deletequest` | Yes           | Yes        |            |            |          |
|           |            | Delete OWN quests `/deletequest` | Yes           | Yes        | Yes        | Yes        |          |
|           |            | Quests in DB by ID `/willow`     | Yes           | Yes        |            |            |          |
|           |            |                                  |               |            |            |            |          |
|           | Help       | Show `/help`                     | Yes           | Yes        | Yes        | Yes        |          |


# Usage

## Bot commands
### Command: No command - just send your location to the bot

The bot will guide you through the creation of a raid poll or a quest based on the settings in the config file.

In case of a raid poll the bot will ask you for the raid level, the pokemon raid boss, the time until the raids starts and the time left for the raid. Afterwards you can set the gym name and gym team by using the /gym and /team commands.

In case of a quest the bot will ask you for the quest type and quest action to be done and the reward which will be given upon quest fulfillment.

### Command: /start

The bot will guide you through the creation of the raid poll by asking you for the gym, raid level, the pokemon raid boss, the time until the raid starts and the time left for the raid. Afterwards you can set the gym team by using the /team command.

#### Screenshots
#### Send `/start` to the bot to create a raid by gym selection:
![Command: /start](/screenshots/command-start.png?raw=true "Command: /start")

#### Select the gym via the first letter:
![Command: /start](/screenshots/commands-start-select-gym-first-letter.png?raw=true "Command: /start")
![Command: /start](/screenshots/commands-start-select-gym-letter-d.png?raw=true "Command: /start")

#### Select the raid level and raid boss:
![Command: /start](/screenshots/commands-start-select-raid-level.png?raw=true "Command: /start")
![Command: /start](/screenshots/commands-start-select-raid-boss.png?raw=true "Command: /start")

#### Select the start time (clock time or minutes) and the duration of the raid:
![Command: /start](/screenshots/commands-start-select-starttime-clock.png?raw=true "Command: /start")
![Command: /start](/screenshots/commands-start-select-starttime-minutes.png?raw=true "Command: /start")

![Command: /start](/screenshots/commands-start-select-raid-duration.png?raw=true "Command: /start")

#### Raid poll is created. Delete or share it:
![Command: /start](/screenshots/commands-start-raid-saved.png?raw=true "Command: /start")

### Command: /help

The bot will answer you "This is a private bot" so you can verify the bot is working and accepting input.


### Command: /mods

The bot allows you to set some users as moderators. You can list, add and delete moderators from the bot. Note that when you have restricted the access to your bot via BOT_ADMINS and BOT_ACCESS, you need to add the users as administrators of a chat or their Telegram IDs to either BOT_ADMINS or BOT_ACCESS. Otherwise they won't have access to the bot, even though you have added them as moderators! 


### Command: /raid

Create a new raid by gomap-notifier or other input. The raid command expects 8 parameters and an optional 9th parameter as input seperated by comma.

Additionally the raid command checks for existing raids, so sending the same command multiple times to the bot will result in an update of the pokemon raid boss and gym team and won't create duplicate raids.

Parameters: Pokemon raid boss id, latitude, longitude, raid duration in minutes, gym team, gym name, district or street, district or street, raid pre-hatch egg countdown in minutes (optional)

Example input: `/raid 244,52.516263,13.377755,45,Mystic,Brandenburger Tor,Pariser Platz 1, 10117 Berlin,30`


### Command: /pokemon

Update pokemon of an existing raid poll. With this command you can change the pokemon raid boss from e.g. "Level 5 Egg" to "Lugia" once the egg has hatched.

Based on your access to the bot, you may can only change the pokemon raid boss of raid polls you created yourself and cannot modify the pokemon of raid polls from other bot users.


### Command: /pokedex

Show and update any pokemon raid boss. You can change the raid level (select raid level 0 to disable a raid boss), pokemon CP values and weather information of any pokemon raid boss.

#### Screenshots
#### Manage pokemons / raid bosses via the `/pokedex` command:

![Command: /pokedex](/screenshots/command-pokedex.png?raw=true "Command: /pokedex")

#### All raid bosses:

![Command: /pokedex](/screenshots/commands-pokedex-all-raid-bosses.png?raw=true "Command: /pokedex")

#### Select and edit a specific pokemon / raid boss:

![Command: /pokedex](/screenshots/commands-pokedex-list-raid-boss-pokemon.png?raw=true "Command: /pokedex")
![Command: /pokedex](/screenshots/commands-pokedex-edit-raid-boss-pokemon.png?raw=true "Command: /pokedex")

#### Edit the raid level:

![Command: /pokedex](/screenshots/commands-pokedex-set-raid-level.png?raw=true "Command: /pokedex")
![Command: /pokedex](/screenshots/commands-pokedex-saved-new-raid-level.png?raw=true "Command: /pokedex")

#### Edit the CP values, e.g. Max CP:

![Command: /pokedex](/screenshots/commands-pokedex-enter-max-cp.png?raw=true "Command: /pokedex")
![Command: /pokedex](/screenshots/commands-pokedex-save-max-cp.png?raw=true "Command: /pokedex")
![Command: /pokedex](/screenshots/commands-pokedex-saved-new-max-cp.png?raw=true "Command: /pokedex")

#### Edit the weather:

![Command: /pokedex](/screenshots/commands-pokedex-set-weather.png?raw=true "Command: /pokedex")


### Command: /new

The bot expects latitude and longitude seperated by comma and will then guide you through the creation of the raid poll.

This command was implemented since the Telegram Desktop Client does not allow to share a location currently.

Example input: `/new 52.514545,13.350095`


### Command: /list 

The bot will allow you to get a list of the last 20 active raids, share and delete all raids which got shared to channels as a raid overview.

#### Screenshots
#### List existing raid polls with the `/list` command:

![Command: /list](/screenshots/command-list.png?raw=true "Command: /list")

![Command: /list](/screenshots/commands-list-active-raids.png?raw=true "Command: /list")

#### Share overview message with all raids shared to channel "Chat-Name" to the channel:

![Command: /list](/screenshots/commands-list-share-overview.png?raw=true "Command: /list")

#### Delete the shared overview message:

![Command: /list](/screenshots/commands-list-delete-overview.png?raw=true "Command: /list")

### Command: /delete

Delete an existing raid poll. With this command you can delete a raid poll from telegram and the database. Use with care!

Based on your access to the bot, you may can only delete raid polls you created yourself and cannot delete raid polls from other bot users.

#### Screenshots
#### Delete an existing raid poll with the `/delete` command:

![Command: /delete](/screenshots/command-delete.png?raw=true "Command: /delete")

![Command: /delete](/screenshots/commands-delete-raid-deleted.png?raw=true "Command: /delete")

### Command: /team

The bot will set the team to Mystic/Valor/Instinct for the last created raid based on your input.

Example input: `/team Mystic`


### Command: /gym

The bot will set the name of gym to your input.

Example input: `/gym Siegessäule`

### Command: /quest

Create a quest by searching for the Pokestop name in the database. The bot will answer with all pokestops matching the name, e.g. "PokeCity Stop".

Example input: `/quest PokeCity Stop`


### Command: /listquest

The bot will allow you to get a list of the quests from today, share and delete all quests.


### Command: /deletequest

Delete an existing quest. With this command you can delete a quest from telegram and the database. Use with care!

Based on your access to the bot, you may can only delete quests you created yourself and cannot delete quests from other bot users.


### Command: /willow

Get a list of all available quests and their ID from the database.

## Map:

If you like to use the map, you need to put in an mapbox token in map/index.php. The sprite's are on copyright, that's why they are not in the icons map. Get sprite's and call them: "id_1.png" where 1 is the pokedex number.

# Debugging

Check your bot logfile and other related log files, e.g. apache/httpd log, php log, and so on.

# Updates

Currently constantly new features, bug fixes and improvements are added to the bot. Since we do not have an update mechanism yet, when updating the bot, please always do the following:
 - Add new config variables which got added to the config.php.example to your own config.php!
 - If new tables and/or columns got added or changed inside raid-pokemon-bot.sql, please add/alter these tables/columns at your existing installation!

# TODO

* New gyms: Adding gyms to database without creating a raid via /raid
* Delete incomplete raids automatically: When a bot user starts to create a raid via /start, but does not finish the raid creation, incomplete raid data is stored in the raids table. A method to automatically delete them without interfering with raids just being created would be nice.

# SQL Files

The following commands are used to create the raid-pokemon-bot.sql and raid-boss-pokedex.sql files. Make sure to replace USERNAME and DATABASENAME before executing the commands.

#### raid-pokemon-bot.sql

Export command: `mysqldump -u USERNAME -p --no-data --skip-add-drop-table --skip-add-drop-database --skip-comments DATABASENAME | sed 's/ AUTO_INCREMENT=[0-9]*\b/ AUTO_INCREMENT=100/' > sql/raid-pokemon-bot.sql`

#### pokedex-pokemon.sql

Export command: `mysqldump -u USERNAME -p --skip-extended-insert --skip-comments DATABASENAME pokemon > sql/pokedex-pokemon.sql`

#### quests-rewards-encounters.sql

Export command: `mysqldump -u USERNAME -p --skip-extended-insert --skip-comments DATABASENAME questlist rewardlist encounterlist quick_questlist > sql/quests-rewards-encounters.sql`

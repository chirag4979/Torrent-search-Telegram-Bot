# Torrent-search-Telegram-Bot
Multipurpose Telegram bot designed to work with my [Torrent-search-API](https://github.com/ArtiomSu/Torrent-search-API).

This bot is capable of:
1. [Greeting new members](#greeting-new-members)
2. [Admin tasks](#admin-tasks) such as
  * ban/unban
  * make admin
  * generate invite link
  * restrict/unrestrict users
3. [Internal permission managment](#internal-permission-managment)
  * request/grant permissions
  * see all granted permissions
  * remove permissions
  * allow access to admin functions
  * allow access to torrent searching
  * allow access to permission mangement
4. [Torrent searching](#torrent-searching)
  * search for torrents and sort them
  * get magnet link
5. [Specific things](#specific-things) (Misq)  

# Setup
0. Clone the repo.
1. Create a `.env` file and fill in what you need. This is the complete example
```
BOT_TOKEN=XXXXXXXX:XXXXXXXXXXXXXXXXXXXXXXXXXXX #this is your telegram bot api key
API_URL=http://10.0.0.69:9876 #this is where Torrent-search-API is running. 
BOT_NAME=@t80search_bot # this is what the bot will respond too
LOGGING_CHANNEL=123454234 # if you enable logging it will send logs to this telegram channel
ROOT_USER=1234567 # This your telegram user id
YOUTUBE_CHANNEL=12345 # you can ignore this unless you want to forward your own custom message
YOUTUBE_CHANNEL_PINNED_MSG_ID=12345 # you can ignore this unless you want to forward your own custom message
```
2. `npm install` to download the nodejs modules
3. `npm start` to start the bot
4. Add the bot to your group and set it as admin

# help pages
The bot has 4 built in help pages if you get stuck, simply write the following into telegram
1. `@t80search_bot -h`
```
t80search_bot V0.0.5    

        -t gets you into the torrent functions
        -t -h to see help page for torrent

        -a gets you into the admin functions
        -a -h to see help page for admin

        -p gets you into permissions manager
        -p -h shows help for permissions manager

        -p_ask allows you to request permissions
        -p_ask -t for example allows you to request
                  permissions to use torrent search

        Current requestable permissions are:
        ____________________________________
        -t for torrent searching
        -a for admin functions
        -p allows you to grant or remove
           permissions from users

        whois id lets you see info about users
              replying to a user with whois will
              do the same

        pinned forwards the saved message
```
2. `@t80search_bot -t -h` user will need to have -t permissions to view this
```
t80search_bot V0.0.5 Torrents help    

        currently only 1337x.to is used.....

        options:
        -l n:  limits results to n results
               default is 6
        -s n: sort results by parameters (must be number)
            *   1 - by size decending (default)
            *   2 - by size ascending
            *   3 - by time descending
            *   4 - by time ascending
            *   5 - by seeders descending
            *   6 - by seeders ascending
            *   7 - by leechers descending
            *   8 - by leechers ascending
        -d n: shows url and magnet link for torrent this is only possible after searching. n will be displayed in the first column of search results (0 - n..)
        -h : displays this page

        basic usage: @t80search_bot ubuntu 16.04
        advanced usage - find ubuntu with most seeders and show only top 3 results
        @t80search_bot -s 5 -l 3 ubuntu 16.04

        get magnet link - use 0 for first result, 1 for second...
        @t80search_bot -d 1
```
3. `@t80search_bot -p -h` user will need to have -p permissions to view this
```
t80search_bot V0.0.5 Permissions    

        -l lists all permission requests
        -lg lists all granted permissions
        -g allows you to grant permissions so like
        -g -t 123456789 grants the user whose id is 123456789 torrent access
        -r removes permission from user in the same way as granting
        -h displays this help
```
4. `@t80search_bot -a -h` user will need to have -a permissions to view this
```
t80search_bot V0.0.5 Admin    

        These can either be replies to a message
        or just send the id directly

        ban (id) bans a user from the group
        unban (id) unbans a user from the group
        promote (id) makes a user admin
        gen_link generates invite link
        restrict (id) user cant do anything
        unrestrict (id) user is back to normal
        -h displays this help
```
## Greeting new members
When a user joins the group there will be a welcome message. For default channels it will look like this
```
Testing_user_ths Welcome to testbots

    _    _ _____ 
   | |  | |_   _|
   | |__| | | |  
   |  __  | | |  
   | |  | |_| |_ 
   |_|  |_|_____|
```
For my channel it will look like this
```
Testing_user_ths Welcome to Terminal Heat Sink Group

    _______ _    _  _____ 
   |__   __| |  | |/ ____|
      | |  | |__| | (___  
      | |  |  __  |\___ \ 
      | |  | |  | |____) |
      |_|  |_|  |_|_____/ 
                          
Click 👉 This 👈 (https://t.me/Terminal_Heat_Sink_Group/1514) For all guides, links to downloads and so on. It 
should have everything you need :)
```
## Admin tasks
Admin tasks work based on user id, or replies. 
##### ban
So to ban a user find a message they posted and reply with this
`@t80search_bot -a ban` if you know the user id you want to ban you dont need to reply to message you can just sent this on telegram
`@t80search_bot -a ban 123456`
##### unban
same as ban but unbans a user so they can join again
##### promote
Allows you to make another user an admin, this also works for other bots, so you can either reply to a message of the user or use their id
`@t80search_bot -a promote 12345`
##### gen_link
Generates an invite link that people can use
`@t80search_bot -a gen_link`
##### restrict
This will restrict a user from typing or doing anything, works just like ban but user doesnt leave the group
`@t80search_bot -a restrict 12345`
##### unrestrict
opposite of restrict

## Internal permission managment
Currently there are 3 different types of permissions in the bot.
1. `-t` allows the user to user torrenting features
2. `-p` allows user to grand and take away permissions from other users
3. `-a` allows user to use admin functionality
By default the root user will have access to all 3.

If a user wants some permissions they will need to ask for it like so
`@t80search_bot -p_ask -t` this will let the user know you want -t permissions. You can also ask for admin like this `@t80search_bot -p_ask -a` and so on. The bot will respond with
```
Test
Request submitted wait for approval
```
Then whoever has -p permissions can see which permisions are requested with `@t80search_bot -p -l`
And the bot will respond with something like this
```
Permissions requests

   -t || Test || 123456
```
To grant this permission use this `@t80search_bot -p -g -t 123456`.
The bot will respond with
```
Permission request granted
```
## Torrent searching
To search for a torrent you can simply run `@t80search_bot -t Ubuntu`. This will return a list with the largest file size first by default, to get the torrent you can either click the link or use the bot to get the magnet link.
##### refined searches
This is sorting and limiting

`@t80search_bot -t -s 1 -l 6 Ubuntu` this is the default what you would get by simply running `@t80search_bot -t Ubuntu`

`@t80search_bot -t -s 5 -l 1 Ubuntu` return 1 result that has the highest seeders.

See the help page for more sorting options `@t80search_bot -t -h`

##### getting magnet link
After you searched for something the bot will save your results until you search the next time.
to get the magnet link take note what number corresponds with your result, for example bellow is a search result
```
1: Ubuntu MATE 16.04.2 [MATE][armhf][img.xz][Uzerus]2 (https:........)

se: 260 | le: 2 | date: Mar. 7th '17 | size: 1.1 GB

_
2: A Practical Guide to Ubuntu Linux, 3rd Edition (PDF) (https........)

se: 81 | le: 3 | date: Aug. 12th '18 | size: 77.0 MB

_
3: Ubuntu Unleashed 2019 Edition (https:.........)

se: 71 | le: 2 | date: Oct. 14th '19 | size: 21.4 MB

_
4: Ubuntu 16.04.1 LTS Desktop 64-bit (https:..........)

se: 55 | le: 2 | date: Aug. 6th '16 | size: 1.4 GB

_
```
If you want the magnet link for the 4th result ubuntu 16.04.1 LTS then you can simply `@t80search_bot -t -d 4` and the bot will return the magnet link.

## Specific things
This is where some fun functions go, there will probably be a better category for these later on lol.
#### pinned
This forwards a message specified in the .env file. `@t80search_bot pinned`
#### whois
This gets user information. If you want to know someones id you can reply to them with `@t80search_bot whois` and the bot will reply with something like
```
id:         123456
is_bot:     true
first_name: torrent
username:   t80search_bot
status:     administrator
```
If you know the id if the user you dont need to reply to them you can just type `@t80search_bot whois 123456` and the same as above will occur.

# Telegram Group that this bot is active on
[Terminal_Heat_Sink_Group](https://t.me/Terminal_Heat_Sink_Group)

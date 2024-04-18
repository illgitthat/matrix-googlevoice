# Matrix Google Voice Bridge Bot

Since Google Voice has no API, and probably never will, this is a method for "pseudo-bridging" your Matrix server with Google Voice.

_**PLEASE NOTE** this is a draft adapted from my own use, so I may have left out something I did_ [_all along the way_](https://www.youtube.com/watch?v=IkA9b5UWr9g)_. Please ping me with an issue if I missed anything._

### How it works

*   Use Google Voice’s **Forward messages to email** option and watch inbox for new messages, then create new rooms for each sender
*   Watch these rooms for replies and route back through Gmail to Google Voice.

### Supported:

*   Text messages, incoming & outgoing (replies)
*   Incoming media (images, etc.)

### Not supported:

*   Group chats (probably never, because Google)
*   Outgoing media (apparently impossible via Gmail → Google Voice, because Google)
*   Backfilling history

### Planned:

*   Automatically grab avatars for contacts from your Gmail Contacts.

## Setup

1.  In **Google Voice > Settings > Messages,** make sure **Forward messages to email** is ON.
    1.  You may wish to archive these so they do not clutter your inbox using a Gmail inbox rule/filter.
    2.  `Matches: from:(*@txt.voice.google.com OR voice-noreply@google.com)` >> **Do this:** `Skip Inbox`
2.  Create a new account for your bot on your any Matrix server (e.g., matrix.org or a homeserver), then get the bot's `access_token`. (The simplest way to do this is using [Element](https://element.io/). Instructions [here](https://t2bot.io/docs/access_tokens/).)
3.  You must send the replies _from your own Gmail account_, so this requires authenticating your Gmail. So generate an **App Password** for Gmail. (Instructions [here](https://support.google.com/accounts/answer/185833).)
4.  You can run this bot on any machine with Internet and `node` – your homeserver, laptop, Pi, whatever. 
5.  On the machine where this bot will run:
    -  `git clone https://github.com/dzg/matrix-googlevoice`
    -  `npm install`
    -  `cp config.example.js config.js`
    -  Edit `config.js` with your parameters. See comments there for more info.
    -  Run `node matrix-googlevoice-bot.js` 
    -  Set it up to always run using your preferred method.
       -  This will depend on your operating system, but an example way to do this is using `./service.sh`

#### Notes

*   If you "leave" the room created by the bot, you might not be able to rejoin, and later you will not be able to receive messages from the same sender, because the room alias will still be reserved, which would require manually deleting the old alias.
*   Feel free to change the Room Topic, Name, or Avatar – but do _not_ delete the Alias.

## Extras

Some other things the bot can do:

*   `!name <string>` Set room name
*   `!botname <string>` Set bot name (in all rooms)
*   `!botnick <string>` Set bot nickname (in current room)
*   `!avatar <mxc or http URL>` Set room & bot room avatar to linked image (like a photo of the contact.) Example:
    `!avatar https://play-lh.googleusercontent.com/Gf8ufuFbtfXO5Y6JuZjnG0iIpZh21zNTqZ5aiAXO8mA38mvXzY-1s27FWbGlp51paQ`
*   `!show <mxc URL>` Display content of an MXC URL
*   `!restart` Restart IMAP & Matrix connections
*   `!echo <text>` Check if alive

## To do

*   Automatically search Google Contacts API for avatars
*   Add options for logging
*   Figure out sending media capability ... anyone know how? No method I've tried allows replying with image from Gmail.

## Changelog

#### 2024-04-18
* Fixed texts from short codes
* Added support for customized `imapSearchFolder` folder. This is helpful when Google Voice emails are set to auto archive (with a Gmail Filter)
* Bug fixes
#### 2022-04-19
* Fixed mail client multiple connection [issue](https://github.com/dzg/matrix-googlevoice/issues/1)
#### 2022-03-17
* Added `!restart` to restart connections
#### 2022-03-15
* `!avatar` now changes both the room avatar and the bot's avatar in the room
* Added `keepalive` for `Imap`; hopefully less disconnects
#### 2022-03-14
* Added `package.json` for `npm install` support
* Better comments in `config.example.js`
* Added `backdays` option to grab older emails


revised 4/19

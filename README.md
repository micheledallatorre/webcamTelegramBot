# webcamTelegramBot
Telegram Bot to show public webcam images

#Instructions to deploy the bot on www.openshift.com
* Follow tutorial as in https://github.com/ilbonte/node-telegram-bot-starter-kit to create openshift account (select Node.js 0.10 version)
* To generate ssh key on Windows use Git Bash and follow the instructions provided in https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#generating-a-new-ssh-key
* Clone the openshift repo on your local Windows machine using Git Bash:
```bash
git clone ssh://<REMOVED>.rhcloud.com/~/git/testbot.git/
cd testbot/
```

** Add all the dependencies you need, e.g.:
```bash
npm install -S node-telegram-bot-api 
npm-install -S request
```

* Edit the package.json file created, using name with the same name of the folder/project in openshift, e.g.
```javascript
{
  "name": "testbot", // THIS ONE HAS TO BE TESTBOT, NOT OPENSHIFT SAMPLE APPLICATION!
  "version": "1.0.0",
  "description": "OpenShift Sample Application",
  "keywords": [
    "OpenShift",
    "Node.js",
    "application",
    "openshift"
  ],
  "author": {
    "name": "OpenShift",
    "email": "ramr@example.org",
    "url": "http://www.openshift.com/"
  },
  "homepage": "http://www.openshift.com/",
  "repository": {
    "type": "git",
    "url": "https://github.com/openshift/origin-server"
  },

  "engines": {
    "node": ">= 0.6.0",
    "npm": ">= 1.0.0"
  },

  "dependencies": {
    "express": "~3.4.4"
  },
  "devDependencies": {},
  "bundleDependencies": [],

  "private": true,
  "main": "server.js"
}
```
* Edit the server.js file as follows:
```javascript
var TelegramBot = require('node-telegram-bot-api');

var token = '<REMOVED>'; // ENTER HERE YOUR TOKEN RECEIVED BY TELEGRAM @BotFather, cfr.  https://github.com/ilbonte/node-telegram-bot-starter-kit#get-your-token

// Setup polling way
var bot = new TelegramBot(token, {polling: true});

// Matches /echo [whatever]
bot.onText(/\/echo (.+)/, function (msg, match) {
  var fromId = msg.from.id;
  var resp = match[1];
  bot.sendMessage(fromId, resp);
});

// Any kind of message
bot.on('message', function (msg) {
  var chatId = msg.chat.id;
  // photo can be: a file path, a stream or a Telegram file_id
  var photo = 'cats.png';
  bot.sendPhoto(chatId, photo, {caption: 'Lovely kittens'});
});
```

* Push all changes to openshift
```bash
git add .
git commit -m 'My changes'
git push
```

* Wait for the bot to be deployed (application port 8080 not available error on openshift does not matter!) 

* Add your bot in telegram (search for its name, e.g @testbot) and test it by sending `/echo hello` and you should receive a message back stating `hello` (cfr. https://github.com/ilbonte/node-telegram-bot-starter-kit#enjoy)

* Now, simply edit your server.js! Have fun! :)



Many thanks to [@ilbonte](https://github.com/ilbonte) for his precious help! 

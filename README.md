# TwitterUrlCrawler bot
This TwitterUrlCrawler bot allows you to pull tweets from your feed or other users' timeline and store attached URLs in a Google Spreadsheet. It has never been easier to keep track of all the interesting URLs on Twitter out there. Check it out!

## Features:
- **Automatically crawl our own twitter feed or timelines of any account** Your bot automatically crawls your own feed or any twitter timeline of interest and picks out URLs
- **Confirm storing the URLs via Telegram commands** Whenever the bot found an URL in your feed or timeline, it asks you to store them via Telegram. After In-Telegram preview, easily confirm or decline storing the URL to your private list. 
- **Store URLs of interest to a Google Spreadsheet**: The Google end of the bot allows you to store URL to a Spreadsheet. 
- **Back up for URLs**: The bot also pulls a copy of the saved URLs as HTML into a Google Drive folder
- **Simple syntax**: The core element of the bot relies only on if-else statements and is understandable also to programming beginners
- **Easy to install, free of charge, 24/7 readiness**

## Initizalization

- Set up a telegram bot with @BotFather (relevant information: bot token), see here: https://core.telegram.org/bots
- Set up new Google Spreadsheet (relevant information: webApp url, Spreadsheet id)
- Set up Google Drive folder to store URLs (relevant information: folder id)
- Set up a Twitter API and developers account (relevant information: Consumer Key, Secret Token)

>> Setting up the Telegram and Google Apps Script part follows this YouTube tutorial: https://www.youtube.com/watch?v=mKSXd_od4Lg 
>> Setting up the Twitter API follows this YouTube tutorial: https://www.youtube.com/watch?v=2o_qt9cXicM

## Prepare Google Spread Sheet

The bot in its current specification is written to fill the attached spread sheet [Template](docs/TwitterUrlCrawler_timeline.xls). The provided template consistes of four worksheets:
- *Worksheet 1 "saved_list":* This is where the approved URLs are stored; the columns must match those of Worksheet 2 "current_input".
- *Worksheet 2 "current_input":* This is where the bot stores the current URL before you either confirm it (and it's pushed to worksheet 1) or decline it (and it's being deleted); the columns must match those of Worksheet 1 "saved_list".
- *Worksheet 3 "command_storage":* This is a temporary storage for commands that control the bot.
- *Worksheet 4 "twitter_log":* Here, the bot logs all twitter crawls and it helps you understand what happens within the bot.

*Suggestion*: Import the spreadsheet, see how the bot works and customize the bot to your needs afterwards.

## Import and fill individual details to Google Apps Script code

The bot runs on a Google Web App, using Google Apps script - a JavaScript based programming. To get the bot running, you can download the .gs.txt files, rename them to .gs (Google Apps Script) and import them to your Google Script Apps project.

As you see in the directory, there a three files that together build the whole bot:

### 1. GoogleAppsScript-funcs.gs

This script contains the core elements of the TwitterUrlCrawler such as the main(), storeTweet(), and trigger functions. The functions all work on the Google Web Apps site and effect changes in the Google Spreadsheet. The important parts to fill in with your information are emphasized with three asterices.

```
webAppUrl = *** YOUR GOOGLE WEBAPPS URL ***; you will receive the webAppUrl when you publish your script for the first time as WebApp (looks like this: https://script.google.com/macros/s/AKfydbw7KIMKGdLNpmPtdXJoQF7Ras61ItJq8Dztvjh9CNNXZ1EmJio/exec)
ss_id = *** YOUR GOOGLE SPREADSHEETS ID ***; you will find the ss_id in the URL of your spreadsheet (looks like this: "1Bsn7gZX5jZ_lTVucM5ebaDzmxtok3UtMNVUb8021h3e")
folder_id = *** YOUR GOOGLE DRIVE FOLDER ID ***; 
admin_mail = *** YOUR ADMIN EMAIL ***
```

### 2. Telegram_funcs.gs

This script consists of all Telegram related functions. Amongst others, it contains the following functions:

- getMe(): checking bot status
- setWebhook(): setting telegram webhook (to connect Google WebApp to bot)
- deleteWebhook(): deleting telegram webhook
- **doPost()**: interaction with bot (digest messages to bot and post reponses) --> this is the core of the bot's Telegram interaction!
- getHTMLfromURL(url): grab HTML from given URL, returns html
- StoreHtmlToDrive(html_input, file_title): stores HTML to Google Drive folder, set with folder_id
- isolateURL(string): isolates urls starting with HTTP and HTTPS from strings
- three send functions for replies of the telegram bot to its users

Make sure to fill in these information to customize the bot to your accounts:
```
token = *** YOUR TELEGRAM BOT TOKEN ***; you will receive the token from the Telegram botfather (looks like this: 1175871460:AAGDapaqpOGdYgsMqDL0lP213IKlMiqXGog)
admin_id = *** ADMIN TELEGRAM ID ***; if you want to receive status updates for your bot, please fill in your Telegram id (or the one of the admin). You'll find the id if you send your bot a message and look it up in the as sender_id... ;-) (looks like this: 325208322)
```

### 3. TwitterAPI_auth-funcs.gs

This script contains all TwitterAPI-related function. After filling in your Consumer Key and Access Token, you can modify the functions getTweetsHomeTimeline() and getTweetsForUser() to your needs. At the end of the script, the Authentification library (OAuth1) is added and should not be touched unless you really know what you are doing. Check out details for the library here: https://developers.google.com/google-ads/scripts/docs/examples/oauth10-library

Make sure to fill in these information to customize the bot to your Twitter developer account details. You'll find them on your Twitter Dev account after application:
```
CONSUMER_KEY = *** YOUR CONSUMER KEY ***;
CONSUMER_SECRET = *** YOUR CONSUMER SECRET KEY ***;
ACCESS_TOKEN = *** YOUR ACCESS TOKEN ***;
ACCESS_SECRET = *** YOUR ACCESS SECRET ***;
```

## Prepare Telegram bot

As a last step, add the commands to your telegram bots via the BotFather (https://core.telegram.org/bots#commands). To use the preset functions of the TwitterUrlCrawler bot, add the following commands:

```
save_input - saves URL
add_url- manually add URL
cancel - cancel current input
delete - delete last saved input
help - further information
```

Watch out: The bot does not work with inline commands!

## Set time trigger in Google Apps

As you want the bot to work automatically, you can set time-based trigger via Google Apps. The most comfortable way is to go via https://script.google.com/home/projects/ + your project id + /triggers. Here you can adjust all trigger settings. Make sure to execute the main() function.

If you want to do if fully manually, check out the trigger functions in the GoogleAppsScript-funcs sheet - not as comfortable but of course way cooler!

## Start off = customize to your own needs

Take a closer look at the doPost() function in the Telegram-funcs sheet, as it stores everything you want the bot to do on the telegram side. Whenever it comes to storing information in a Spreadsheet, you're well advised to check out the GoogleAppsScript-funcs sheet. With little programming skills, you can start adapting the TwitterUrlCrawler bot to your own needs. Make sure to always select the appropriate cells in the Spreadsheet as it works as your temporary (Worksheet 2 + 3) and permanent storage. 

Have fun!


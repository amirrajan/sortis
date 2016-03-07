## Sortis - A twitter client for power users, and a springboard for developers

Sortis is a twitter client for managing the tweets you've favorited. More importantly, it's a springboard to create your own twitter based applications (and learn tech along the way).

Tech: NodeJS, AngularJS, ExpressJS, Redis, Twitter Rest Api's

License: MIT

## It's for developers

This app isn't deployed anywhere for use by the public. It is meant to be deployed **by you, the developer**. Sortis is a single user application that you can deploy to the **NodeJS** provider of your choice (I've included instructions for deploying to Nodejitsu and Heroku). **You own the code and the data. Extend this app as you see fit. Make your own twitter mashups. Publish your code for others to learn, fork, extend and deploy**.

Sortis can be set up **freely** on cloud based offerings and trial accounts. You can run this entire app locally too if you don't want to subscribe to any (potentially) for-pay services.

## What Sortis does out of the box

I favorite quite a few tweets on twitter for different reasons. Things I want to read later, pairing sessions that I've scheduled, interesting projects that I've bookmarked...the list goes on. It suffices to say that the ability to just favorite a tweet wasn't enough anymore. Sortis, uses the tweets I've favorited as an "inbox" that I can then sort through, tag, search and archive.

Here is the inbox, this view uses [Twitter's Rest Apis](https://dev.twitter.com/docs/api/1.1) to retrieve tweets you've favorited:

<img src="inbox.png" />

Tweets can be sorted, tagged, and categorized. Once a tweet is sorted, the tweet is unfavorited from Twitter and stored entirely in your app (using [Redis](http://redis.io/)). Here is a screen shot of the sorted screen:

<img src="sorted.png" />

## Instructions for deploying your own Sortis

Go to http://nodejs.org and install NodeJS

Go to http://redis.io/download and install Redis

Then clone this repo:

    git clone https://github.com/amirrajan/sortis.git

And `cd` into the directory (all instructions below assume you are in the `sortis` directory:

    cd sortis

## Run Locally

Using a command prompt where `node` is available, run the following command to install all Sortis dependencies:

    npm install -g grunt
    npm install -g grunt-cli
    npm install (you may need to prefix this with sudo if you're on Mac/Linux)

Register for a Twitter developer account: https://dev.twitter.com/user/login?destination=home

Under My Applications (https://dev.twitter.com/apps), create a new application:

<img src="create-twitter-app.png" />

Set up the application with the following settings. Anywhere you see amirrajan, replace that with your own twitter handle:

<img src="twitter-app-settings.png" />

<img src="twitter-app-settings-2.png" />

Once your twitter account is set up, create a `.env` file by duplicating `env.sample` then update *just* the four Twitter-related values, plus the 'callBackUrl'. Here is the full `.env` file:

    consumerKey=XXXXXXXXXXXXXXXX
    consumerSecret=XXXXXXXXXXXXXXXX
    accessToken=XXXXXXXXXXXXXXXX
    accessTokenSecret=XXXXXXXXXXXXXXXX
    callBackUrl=XXXXXXXXXXXXXXXX
    password=XXXXXXXXXXXXXXXX
    mobile=+15555555555
    twilioAssignedPhoneNumber=+15555555555
    twilioAccountSid=XXXXXXXXXXXXXXXX
    twilioAuthToken=XXXXXXXXXXXXXXXX
    enableAuth=false

Here are the values you need to update:

    consumerKey=XXXXXXXXXXXXXXXX
    consumerSecret=XXXXXXXXXXXXXXXX
    accessToken=XXXXXXXXXXXXXXXX
    accessTokenSecret=XXXXXXXXXXXXXXXX
    callBackUrl=XXXXXXXXXXXXXXXX

Values are located on the detail page of Twitter:

<img src="twitter-secret.png" />

Make sure the redis server is running, by running this command:

    redis-server

At this point you'll should be able to run the app locally:

    grunt

To view the app, go to `http://localhost:3000`, the code has a lot of comments. Crack it open and read :-) (start with `\server.js`)

### Running the app "in the cloud"

### Securing the app with a two-phase auth

Given that this app is hard wired to your twitter account, it's best that you run the app with `enableAuth` set to `true`. With authorization enabled, you'll need to provide a password and an authorization code to access the site. If you get the password correct, a text message will be sent (via Twilio) to a mobile device you've specified in
`.env`.

### Register a free account with Twilio

Sign up with a free Twilio account: https://www.twilio.com/try-twilio

Update your `.env` file:

    password=[set a password (plain text)]
    mobile=[this will be your mobile number example: +15555551234]
    twilioAssignedPhoneNumber=[phone number twilio assigns you: +15555554240]
    twilioAccountSid=[value provided by Twilio]
    twilioAuthToken=[value provided by Twilio]
    enableAuth=true

Values are located here and here:

<img src="twilio-app-settings.png" />

<img src="twilio-app-settings-2.png" />

Once you've set these values up, try running the app locally with authorization enabled. After entering your password, you should receive a text message on your devise.

##Signing up and running Sortis on Heroku

From heroku.com, click Documentation, then click the Getting Started button, then click Node.js from the list of options on the left...which will take you here: https://devcenter.heroku.com/articles/nodejs

Install Heroku toolbelt from here: https://toolbelt.heroku.com/

Sign up via the website (no credit card required).

Login using the command line tool:

    heroku login

Create your heroku app:

    heroku create

Add redis to your app

    heroku addons:add redistogo:nano

The Redis connection in Heroku is provided by an enviornment variable `process.env.REDISTOGO_URL`

Git deploy your app:

    git push heroku master

Set the environment variables on the server:

    heroku config:set consumerKey=XXXXXXXXXXXXXXXX
    heroku config:set consumerSecret=XXXXXXXXXXXXXXXX
    heroku config:set accessToken=XXXXXXXXXXXXXXXX
    heroku config:set accessTokenSecret=XXXXXXXXXXXXXXXX
    heroku config:set callBackUrl=[root url for your heroku app]
    heroku config:set password=XXXXXXXXXXXXXXXX
    heroku config:set mobile=+15555555555
    heroku config:set twilioAssignedPhoneNumber=+15555555555
    heroku config:set twilioAccountSid=XXXXXXXXXXXXXXXX
    heroku config:set twilioAuthToken=XXXXXXXXXXXXXXXX
    heroku config:set enableAuth=false

Open the app (same as opening it in the browser):

    heroku open

And your app should be up on Heroku.

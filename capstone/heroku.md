# Deploying Rails on Heroku

### Add the Heroku Tools

```
$ brew tap heroku/brew && brew install heroku
$ heroku --version
```

### Login to Heroku
Now we can login to Heroku from the command line.  Here's what it looks like for me:
```bash
$ heroku login -i
heroku: Enter your login credentials
Email: matt@learnacademy.org
Password: **********
Logged in as matt@learnacademy.org
```

### Create a new app on Heroku
Once logged in, we can create an app.  Your's will be named something different than below.

```bash
$ heroku create
Creating app... done, ⬢ warm-fjord-57245
https://warm-fjord-57245.herokuapp.com/ | https://git.heroku.com/warm-fjord-57245.git
```

One more verification step that the remote git repo on Heroku was added to your local git config.

```bash
$ git config --list | grep heroku
remote.heroku.url=https://git.heroku.com/warm-fjord-57245.git
remote.heroku.fetch=+refs/heads/*:refs/remotes/heroku/*
```

### Adding your master.key
We need to add the master.key that your app uses to decrypt your `credentials.yml.enc` file.  Copy the value in `/config/master.key`, and use it in the following command:

```
$ heroku config:set RAILS_MASTER_KEY=<your-master-key>
```

### Update the database config
Let's tell the app how to find the database when its running in production. Heroku automatically setup a free postgresql database for us to get started with.  hey also, and helpfully, provided an environment variable with its url for us to use.  We need to update our `/config/database.yml` file to use that environment variable, and we're all set:

```yml
production:
  url: <%= ENV['DATABASE_URL'] %>
```

Finally, we're ready to push our code.

```
$ git push heroku main:main
```

And once that is done, we can migrate:
```
$ heroku run rails db:migrate
```

You can find your URL to past into your browser with this command:

```
$ heroku apps:info
```

Afterwards, you can follow your logs, and navigate to your application:

```
$ heroku logs --tail
```


[ Back to Syllabus ](../README.md#unit-ten-capstone-project-mvp)

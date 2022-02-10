## What is the Deployment

![](/images/heroku-0.jpeg)

> Software deployment is all of the activities that make a software system available for use. The general deployment process consists of several interrelated activities with possible transitions between them. These activities can occur at the producer side the consumer side or both.

## Heroku 

[Heroku](https://dashboard.heroku.com/) is a platform as a service (PaaS) that enables developers to build, run, and operate applications entirely in the cloud.

Heroku has a free plan price, you can create up to 5 apps free on the free plan, you find it from [here](https://www.heroku.com/pricing).

### Create Account On Heroku
You can create an account for free on Heroku.
1. From your browser, navigate to the Heroku [Signup](https://www.heroku.com/)
2. Fill out the data form and click on create free account

![](/images/heroku-1.png)

### Create APP On Heroku
1. From your browser, navigate to the [Heroku dashboard](https://id.heroku.com).
2. Click New.
3. Select Create a new app.
> When prompted for a name, type dhdev-UNIQUE_ID.  For some accounts, there is an option for the owner. Verify that option is set to Personal.
4. Click Create app
5. Once your app is created you are redirected to the Heroku dashboard. Click **Open App**.

### Connect GitHub Repo With Heroku APP
1. Once your app is created, you are redirected to the Deploy tab. Scroll down to the Deployment method and select GitHub.
2. Scroll down to the Connect to GitHub section and click Connect to GitHub.
3. If you have not already authorized Heroku to access your GitHub account, a window appears asking to authorize. Click Authorize Heroku.
4. Once your GitHub account is authorized, your username is displayed and you can search for repositories stored within your account.
5. In the repo-name input, type `educative-node-js-course`. Click **Search**.
6. Once the name of your repo appears, click Connect.

### Set The environment variables on Heroku

1. Navigate to the setting tap on your app
2. Scroll down to the config vars section and click on `Reveal config vars`
![](/images/herku-2.png)
1. Take all your vars from the `.env` file and set them on this section, except `PORT` it will take from Heroku automatically.
![](/images/heroku-2.png)

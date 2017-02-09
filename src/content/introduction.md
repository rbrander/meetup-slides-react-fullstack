## Implementing a full-stack solution using Express and create-react-app

####  by Rob Brander

---

## About me
```javascript
const presenter = {
  name: 'Rob Brander',
  email: 'rob.brander@rangle.io',
  github: 'rbrander'
}
```

Notes:
I've been developing react applications at Rangle for over a year. Most recently, I've been teaching React and Redux at Rangle.

---

## Background

I wanted to build a React app and host it on Heroku
- `create-react-app` was designed for building a front-end solution
- Using Node with Express, I could provide a back-end solution
- Heroku can be used to host your application for free
- Challenge: To deploy a full-stack react app using only one heroku process

Notes:
I wanted to build a React app and host it on Heroku

---

## Using `create-react-app`

- Installation: `npm install create-react-app -g`
- Create a project: `create-react-app myProject`
- Run dev server: `npm run start`
- Build for production: `npm run build`

Notes:
- review the commands
- I developed most of the front-end code until I needed back-end

This all started when I decided to use create-react-app for one of my side projects.  I was familiar with the basic commands like npm start and npm run build.  I started off by building out the front-end using React and went as far as I could before needing a back-end.

---

## Setting up Heroku
- Push source code to github
- Create a heroku instance
- Bind the github repo to the heroku instance
- Deploy the master branch
- Dyno runs `npm start`
- Heroku now running dev server

Notes:
- Before building out the back-end, I wanted to setup the deployment pipeline in Heroku.

---

## Heroku Dynos

- Dynos are processes that are run on your Heroku instance
- Web and Worker processes
- Free account permits one web process OR one worker process
- All HTTP traffic is sent to only the 'web' process

![](content/images/heroku_dyno_pricing.png)

Notes:
The challenge is that I was thinking I would need at least two, front and back.

---

## Production deploy
Instead of deploying the dev server, I wanted to deploy the production build and run it using a web server.
- Need to run `npm run build` on each deploy
- Run a web server to host the files in `build/`
- Setup Procfile to run `npm run prod`
```bash
$ echo "web: npm run prod" > Procfile
```
![](content/images/packagejson.png)

Notes:
- Currently running dev server; want to make it production
- Chose to use http-server to host the build folder
- Added `npm run prod` to run `http-server build`
- Added a postinstall script to run the build process
- Tell heroku to run prod (Procfile)

---

## Backend

[Simple Express Server](https://scotch.io/tutorials/build-a-restful-api-using-node-and-express-4)

Options:
- Run another site all together using a heroku instsance and a separate repo (CORS)
- Run the server as a 'worker' process (pay for the extra dyno)
- or find some way to merge the back-end with the front-end

Notes:
- I had a simple front-end site running,
- I knew I wanted to have a RESTful API running in the back-end.
- I wrote a simple API that served out a couple of end-points to test with
- When I ran, I couldn't run on same port (CORS)
- I checked heroku to see what it would take to run a server
- I would have to upgrade my dyno ($7/month)

So I had to figure out a way that I could run a back-end server and the front-end server at the same time, using the same port.

---

## The Solution

- The production code was already built
- Let the back-end host the front-end, statically!
```javascript
app.use(express.static('./build'));
```
- Add prefix for API routes
- Update the `npm run prod` to run the back-end server

Notes:

The solution hit me like a truck while looking at the express code I had written for the endpoints.  I just needed to setup one of my directories to serve up static content; the build directory!

Once I had setup my static folder, I wanted to ensure that I wouldn’t have any collisions on the path, so I implemented all of my endpoints with an ‘/api’ prefix.

Once that was done, all I had to do was change my deployment script for heroku to deploy the node server instead of running http-server. So the npm run prod command changed to run ‘node server/index.js’ rather than ‘http-server build’.

---

## Open challenge to the audience

Find a way to run the back-end server and the front-end using npm start (for dev)

Notes:
While this solution is great for production, it's painful for development.  I can run only one server at a time, which means I can do front-end dev with `npm start`.

---

## Thank you
```javascript
const presenter = {
  name: 'Rob Brander',
  email: 'rob.brander@rangle.io',
  github: 'rbrander'
}
```


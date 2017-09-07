---
layout: post
title: "Build your first progressive web app with React"
date: 2017-08-11 16:54:01 -0700
comments: true
categories: 
---

# Build your first progressive web app with React

Progressive Web Apps are the much-hyped future of the web. Let’s build one!

Google has been pushing PWA’s hard as the solution to many of the problems of
the modern web — particularly issues for mobile users.

PWA are essentially fast, performance-focused web applications that are
streamlined for mobile. They also can be saved to your smartphone’s home screen
and, from there, look and feel like a native app (including features like
offline access and push notifications).

Big players like
[Twitter](https://blog.twitter.com/2017/how-we-built-twitter-lite) and
[Flipboard](https://flipboard.com/) have recently launched PWA’s, which you can
try out by going to either [http://flipboard.com](http://flipboard.com/) or
[https://lite.twitter.com/](https://lite.twitter.com/) on your phone.

In this tutorial, you’ll build a simple PWA using React — giving you a
boilerplate from which to construct more complex applications.

### Getting Set Up

To start, let’s generate a basic React application with
[create-react-app](https://github.com/facebookincubator/create-react-app).

To do so, switch to the directory in which you want to save your app, and run
the following:
```
    npm install -g create-react-app
    create-react-app pwa-experiment
```
Then, let’s install React Router:
```
    cd pwa-experiment
    npm install --save react-router@3.0.5
```

Finally, copy this gist into your App.js. This will give us a simple layout with
navigation:

{% gist 32246c815fa522580b7bfd8553eb66c9 %}

Run *npm start* to test our your app. Not much to look at, but it’ll serve our
purposes just fine. Let’s get started converting this into a PWA.

### Step 1: Install Lighthouse

Lighthouse is a free tool from Google that evaluates your app based on their PWA
checklist.

Let’s [install it to
Chrome](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk),
and then evaluate our app. You can start a Lighthouse audit by clicking the
Lightouse icon in the top right corner of Chrome, and then clicking Generate
Report.

![](https://cdn-images-1.medium.com/max/2000/1*V6PeCwLINQCTumMaKGlIpg.png)

Ouch.

So far, our app is fast (since we have very little content) but fails in a
number of key areas.

You can view the checklist that Lighthouse uses [here](https://developers.google.com/web/progressive-web-apps/checklist).

Let’s work through the problems, one by one.

### Step 2: Set Up A Service Worker

A [service
worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) is
a bit of JavaScript that sits between our application and the network. We’re
going to use it to intercept network requests and serve up cached files — this
will allow our app to work offline.

To get started with a service worker, we need to do three things:

* Create a service-worker.js file in our public folder
* Register the worker via our index.html
* Set up caching

Let’s get to it.

First step is pretty self-explanatory. In pwa-experiment/public, create a blank
JavaScript file named service-worker.js.

Second step is a bit more involved. We want to check if the browser supports
service workers, and then register one by loading in service-worker.js.

To do so, let’s add a script tag to our public/index.html.
``` html
    <!doctype html>
    <html lang="en">
      <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
        <title>React App</title>
      </head>
      <body>
        <div id="root"></div>
      </body>
    </html>
```
The code is straightforward- if the navigator supports it, we wait for the page
to load and then register our worker by loading in the service-worker.js file.

Last step: set up caching!

We’re going to copy Addy Osmani’s service worker configuration from
[here](https://medium.com/@addyosmani/progressive-web-apps-with-react-js-part-3-offline-support-and-network-resilience-c84db889162c),
but also disable the cache for development purposes (and take the precautionary
step of deleting all caches when the service worker initializes).

In public/service-worker.js:

{% gist ff78cc64a9216e6d29d28d519c5c4e8d %}

Restart your app with *npm run start* and you should see the following in the
console:

For a deeper discussion into service workers and scope, see the below:

Let’s close DevTools and run the Lighthouse audit again:

We’re making progress! We now have a registered service worker. Since we
disabled the caching, we haven’t ticked the second box yet, but once we enable
caches (when we go live) it will work!

### Step 3: Add Progressive Enhancement

Progressive enhancement basically means your site will work without any
JavaScript loading.

Right now, our index.html just renders an empty div (#root), which our React app
then hooks into.

We want to instead display some basic HTML and CSS before the React app
initializes.

The easiest way to do so is to move some of our basic HTML structure *within
*that div#root. This HTML will be overwritten as soon as ReactDOM renders our
App component, but will give the user something other than a blank page to stare
at as the bundle.js loads.

Here’s our new index.html. Note both the styles in the head, and the HTML within
div#root.

(As an aside, we can now delete duplicate styles in App.css and index.css — just
to be clean.)

Does Lighthouse approve?

Yep!

### Step 4: Add To Home Screen Capability

We can skip the requirements about https — that will be taken care of once we
deploy.

Now, onto the feature that makes PWA’s particularly exciting: the ability for
the user to save them to their home screen, and then open them like an app.

To do, we need to add a manifest.json file to our public directory (copy the
uncommented version below this one).

    {
      // Short name is what appears on home screen
      "short_name": "My First PWA",
      // Name is what appears on splash screen
      "name": "My First Progressive Web App",
      // What appears on splash screen & home screen
      "icons": [
        {
          "src":"icon.png",
          "sizes": "192x192",
          "type": "image/png"
        }
      ],
      // So your site can tell it was opened from home screen
      "start_url": "/?utm_source=homescreen",
      // Match our app header background
      "background_color": "#222",
      // What the URL bar will look like
      "theme_color": "#222",
      // How the app will appear when it launches (see link below)
      "display": "standalone"
      // Read more: 
    }

Here’s the file without comments:

Here’s the icon if you need one (courtesy of my company,
[MuseFind](http://musefind.com/)), or create your own (must be 192 by 192
pixels):

Add the icon.png and manifest.json into your public folder, and then add the
following lines to your index.html:

      <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
        <!-- Add manifest -->
        <!-- Tell the browser it's a PWA -->
        <!-- Tell iOS it's a PWA -->
        <!-- Make sure theme-color is defined -->
        <title>React App</title>
      </head>

Alright, *now* how are we doing?

All we’re missing is caching and https. Let’s push this live!

### Step 5: Deploy Via Firebase

First, let’s turn on caching. Change *doCache* to **true** in your
service-worker.js.

Then, in the [Firebase console](https://console.firebase.google.com/), create a
new project called pwa-experiment.

Back in your project folder, run the following:

    npm install -g firebase-tools
    firebase login
    firebase init

After you complete the login and start the initiation, answer the following
questions:

When it asks **What Firebase CLI features do you want to setup for this
directory?**, use the spacebar to deselect all but Hosting.

Hit enter, then select pwa-experiment as the project.

When it asks **What do you want to use as your public directory?, **type *build*
and then hit enter.

For the single page app question, say no.

That should complete. Then, you can run the following:

    npm run build && firebase deploy

This tells create-react-app to build our project into the build/ folder, which
Firebase then deploys.

Firebase will give you back a URL. Let’s open that in Chrome, and then run our
Lighthouse audit one last time.

This time, we’ll be using HTTPS instead of HTTP, and take advantage of caching.

We did it!

As a last test, open it up on your phone and try saving it to your home screen.
Once opened from the home screen, it should feel like a native app.

### Where To Go From Here

The essence of a PWA is speed. In this tutorial, we skipped a lot of the
performance enhancement, since our app was so barebones.

As your app grows, however, our main.js file is going to grow and grow, and
Lighthouse will be less and less pleased with us.

Stay tuned (AKA follow me: [Scott Domes](https://medium.com/@scottdomes) or my
[Twitter](http://twitter.com/scottdomes)) for an in-depth article on optimizing
performance with React and React Router that will work for both PWA and
old-fashioned web apps.

For now, we have a working skeleton of PWA to build on — we’re ready for the
future of web apps.

### Done!

If this article has been helpful, recommend it by hitting the green heart or
(even better) share it.

Want to stay up to date/learn best practices for the future of web development?
Say hello to Progressive Web App Newsletter — subscribe below.

Here’s the final repo:

* [Web
Development](https://engineering.musefind.com/tagged/web-development?source=post)
* [JavaScript](https://engineering.musefind.com/tagged/javascript?source=post)
* [Progressive Web
App](https://engineering.musefind.com/tagged/progressive-web-app?source=post)
* [React](https://engineering.musefind.com/tagged/react?source=post)
* [ES6](https://engineering.musefind.com/tagged/es6?source=post)

Clapping shows how much you appreciated Scott Domes’s story.

### [Scott Domes](https://engineering.musefind.com/@scottdomes)

Lead Developer @ MuseFind. Curator of
[http://pwa-newsletter.com/](http://pwa-newsletter.com/).

### [Code == Life](https://engineering.musefind.com/?source=footer_card)

We here at MuseFind would like share our take on the world of programming.

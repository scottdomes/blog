---
layout: post
title: "Getting Started with React Native in 20 Minutes"
date: 2017-08-11 15:58:54 -0700
comments: true
categories: [React Native, JavaScript, Mobile App Development, iOS App Development, React]
---

One of the big selling points of React Native, aside from its technical
abilities, is its emphasis on good DX- Developer Experience. Not as often
discussed as UX, but when it comes down to it, a very important part of
delivering high quality apps quickly and efficiently.

So let’s put React Native’s DX to the test, and see if we can set up a simple
login app, with cookie management, in under 20 minutes.

![](https://cdn-images-1.medium.com/max/1600/1*5l_gnYCJYe1qAHuba5Aoew.jpeg)

### Our App

Nearly every type of app you’ll want to build will involve a login screen with
cookies- this app will therefore serve as a perfect foundation for whatever you
want to create.

We’ll also take a tour of the basics of React Native, building basic views and
components and getting familiar with React Native’s baked-in routing solution,
the Navigator.

If you’re familiar with React, you’ll have an easy time reading the code- but by
the end of this tutorial, you should be comfortable enough with React Native’s
structure to move on to more advanced topics.

### The Boring Setup

You need Node, Watchman, the React CLI, and Xcode. If you’re not on a Mac, look
up alternative instructions
[here](https://facebook.github.io/react-native/docs/getting-started.html#content).
Otherwise, just run the following.
```
    brew install node
    brew install watchman
    npm install -g react-native-cli
```
And then download Xcode, if you don’t have it: [Mac App
Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12). (If you have to
download it, that doesn’t count towards the 20 minutes, okay? Only fair.)

### The Fun Setup

Cd into the directory you want to keep your app, and let’s create it!
```
    react-native init LoginApp 
    cd LoginApp 
    react-native run-ios
```
Boom. You should have your iPhone emulator up and running, with your very boring
React Native app loaded up.

On your emulator, hit Cmd + D. This is your developer menu. It does lots of cool
stuff. For now, hit ‘Debug JS Remotely’ (which will launch a tab on your browser
where you can see the console output of your app) and ‘Enable Hot Reloading’.

Now you can open up index.ios.js in your IDE and play around with editing the
text, and watch it change instantly.

### The Views

For our app we will have two different “pages”- the Login view, and the
Dashboard. The idea is that you can’t see the dashboard unless you log in.

To get between the two different views, we’re going to use a React Native
component called NavigatorIOS. This will take care of our routing, in effect.

Now’s a good time to mention that React Native comes with a whole bunch of
awesome components. You can view the docs for NavigatorIOS, and all the rest,
[here](https://facebook.github.io/react-native/docs/navigatorios.html). Once our
20 minutes are up it’s a good idea to check them out, so you know what you don’t
have to build yourself.

### Initial Route

Let’s do some basic routing. In the root of your project, create a folder called
App, and then another folder called Views within it.
```
    mkdir App
    mkdir App/Views
```
Inside, let’s make a file called LoginView.js. Copy and paste the entirety of
index.ios.js into LoginView, then rename the class from LoginApp to LoginView,
and delete the last line.

So:
``` javascript
    export default class LoginApp extends Component {
```
becomes:
``` javascript
    export default class LoginView extends Component {
```
and delete:
``` javascript
    AppRegistry.registerComponent('LoginApp', () => LoginApp);
```
Save it, and we have our first component, which will serve as one of our two
views.

Back in index.ios.js, let’s delete everything within the render function, and
instead replace it with the NavigatorIOS component, which is going to wrap our
entire app and allow us to navigate between views.
``` javascript
    render() {
        return (
          <NavigatorIOS
            navigationBarHidden={true}
            style={styles.container}
            tintColor='#FF6600'
            initialRoute={{}}/>
        );
      }
```
And don’t forget to import the component:
``` javascript
    import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
      NavigatorIOS  // <-- Add me!
    } from 'react-native';
```
As you may have noticed, right now we are supplying an empty object to the
initialRoute prop of NavigatorIOS. What we need to do is instead provide an
object with the following structure:
``` javascript
    let initialRoute = {
      title: 'Login',
      component: LoginView
    }
```
Copy and paste this declaration to the top of the render function, and then plug
it into the NavigatorIOS. You’ll also need to import the LoginView- here’s our
full **index.ios.js** for reference:
``` javascript
    // index.ios.js

    import React, { Component } from 'react';
    import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
      NavigatorIOS
    } from 'react-native';

    export default class LoginApp extends Component {
      render() {
        return (
          <NavigatorIOS
            navigationBarHidden={true}
            style={styles.container}
            tintColor='#FF6600'
        );
      }
    }


    AppRegistry.registerComponent('LoginApp', () => LoginApp);
```
The app is now wrapped by the NavigatorIOS component, which takes care of
rendering the applicable component for each route. We can build an infinite
number of views and use the navigator to move between them.

Our app should now look… exactly the same. So let’s add another route.

### Second Route

Create another file in App/Views called **DashboardView.js**. It should look
very familiar:
``` javascript
    // DashboardView.js

    import React, { Component } from 'react';
    import {
      StyleSheet,
      Text,
      View
    } from 'react-native';

    export default class DashboardView extends Component {
      render() {
        return (
          <View style={styles.container}>
            <Text style={styles.welcome}>
              I'm the second route!
            </Text>
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
      },
      welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
      },
    });
```
Now we need some way to get from our initial LoginView route to the
DashboardView route. Here’s the thing about NavigatorIOS- every direct child is
automatically passed a *navigator* prop, which we can use to our advantage.

In **DashboardView.js:**


``` javascript
    export default class DashboardView extends Component {

    render() {
        return (
          <View style={styles.container}>
            <Text style={styles.welcome}>
              I'm the second route!
            </Text>
          </View>
        );
      }
    }
```
And in **LoginView.js:**
``` javascript
    // LoginView.js

    import React, { Component } from 'react';
    import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
    } from 'react-native';

    export default class LoginView extends Component {

    render() {
        return (
          <View style={styles.container}>
     
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
      },
      welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
      },
      instructions: {
        textAlign: 'center',
        color: '#333333',
        marginBottom: 5,
      },
    });
```
We now have a working routing system, where the user can switch back and forth
between views. Not much utility yet, but that comes next.

### Logging In

As you may have noticed, React Native doesn’t use the standard HTML elements
(anchor tags, p’s, div’s, etc) you may be used to. Instead, they use built-in
components like Text and View, which end up functioning largely the same.

We’re going to build our login form using TextInputs (inputs) and
TouchableHighlights (button-like components, which convert a single child into a
clickable element). This should be pretty understandable:
``` javascript
    // LoginView.js

    import React, { Component } from 'react';
    import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
    } from 'react-native';
    import DashboardView from './DashboardView'

    export default class LoginView extends Component {
      handleGoToDashboard() {
        this.props.navigator.push({
          title: 'Dashboard',
          component: DashboardView
        })
      }

    render() {
        return (
          <View style={styles.container}>
            <Text style={styles.instructions} onPress={this.handleGoToDashboard.bind(this)}>
              Go to Dashboard
            </Text>
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
      },
      welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
      },
      instructions: {
        textAlign: 'center',
        color: '#333333',
        marginBottom: 5,
      },
    });
```
React Native styles are a bit different, but that’s beyond the scope of this
tutorial. You can read more
[here](https://facebook.github.io/react-native/docs/style.html).

To register changes to our TextInputs, we’re going to use local component state:
``` javascript
    // LoginView.js

    ...

    export default class LoginView extends Component {

    ...

    render() {
        return (
          <View style={styles.container}>
            <TextInput 
              style={styles.input} 
             
    />
            <TextInput 
              style={styles.input} 
              
      
    />
            <TouchableHighlight 
              style={styles.button}>
                <Text>Submit</Text>
            </TouchableHighlight>
          </View>
        )
    }

    ...
```
onChangeText comes built-in to TextInput and operates much like onChange for an
input, except it provides the changed value as the first argument (instead of an
event).

In a real app, when the user submits the form, we’d send the username and
password to the server, check it out, get a token back, etc etc. We don’t need
to go through that trouble here. Here’s what we’re going to do to simulate the
logging in experience:

1.  When the user logs in, a token is created and stored in a cookie.
1.  Once that cookie is present, they are redirected to the dashboard.
1.  From the dashboard, they can logout, which deletes the cookie, and sends them
back to the login.
1.  If they reload the app without logging out, they’ll still have the cookie and be
able to go straight to the dashboard.

We’re running out of time, so let’s get going.

### Adding Cookies

To manage our React Native cookies, we’ll use react-native-cookies.
```
    npm install --save react-native-cookies
    react-native link
```
Note: You may have to rerun the *react-native run-ios *command after linking.

Linking simply adds the necessary package files into the native code of the
application.

Then, in LoginView, we import it:
``` javascript
    import CookieManager from 'react-native-cookies'
```
And add a way to submit the form:
``` javascript
    <TouchableHighlight 
      style={styles.button} 
      
    >
        <Text>Submit</Text>
    </TouchableHighlight>
```
And do the magic:
``` javascript
    handleSubmitLogin() {
        if (this.state.username && this.state.password) {
          this.setLoginCookie()
        }
      }

    setLoginCookie(data) {
        let date = new Date()
        date.setDate(date.getDate() + 7)
        CookieManager.set({
          name: 'login_cookie',
          value: this.state.username,
          domain: 'musefind.com',
          origin: 'musefind.com',
          path: '/',
          version: '1',
          expiration: date.toJSON()
        }, (err, res) => {
          console.log('Cookie set')
          console.log(res)
          console.log(err)
          this.handleGoToDashboard()    
         })
      }
```
We check to make sure they filled out the form, then grab the date a week from
today, and set up our cookie (named after the user for this simple test) with
that date as the expiration.

Now, when you fill out the form and hit submit you should see in your debugger
console ‘Cookie set’, and be redirected to the dashboard.

Great. We set a cookie. Now what?

### Detecting the Cookie

When the user opens the app, we want to check if they have a cookie set. If they
do, we want to take them straight to the dashboard.

This is exactly the kind of thing that belongs in componentWillMount, right in
our **index.ios.js**. This function will be called when the app is about to
render.
``` javascript
    // index.ios.js

    import LoginView from './App/Views/LoginView'

    export default class LoginApp extends Component {


    render() {
        return (
          <NavigatorIOS
            ref='nav'
            navigationBarHidden={true}
            style={styles.container}
            tintColor='#FF6600'
            initialRoute={
    }/>
        );
      }
    }
```
We use state to store the initialRoute, which means the app will re-render when
it changes.

However, if you test this out- **it won’t work**, even if you have the cookie
set. That’s because NavigatorIOS is rendering with the initialRoute before we
finish checking for the cookie. After that, it no longer cares about the
initialRoute prop.

In other words, once NavigatorIOS is rendered once, it no longer responds to the
initialRoute prop- so changing it does not change the route.

So we need to hold off on rendering the navigator until we’ve decided what route
to pass it. We’ll add another piece of state to let the app know when to render
the navigator.
``` javascript
    // index.ios.js

    export default class LoginApp extends Component {
      constructor(props) {
        super(props)
        this.state = {
          initialRoute: {
            title: 'Login',
            component: LoginView
          },
        }  
      }

    componentWillMount() {
        CookieManager.getAll((err, res) => {
          if (res.login_cookie) {
            this.setState({
              initialRoute: {
                title: 'Dashboard',
                component: DashboardView
              },
            })
        })
      }

    render() {
      }
    }
```
If the cookie hasn’t loaded, we just render an empty View (like an empty div).

Test it out, and you should be taken straight to the DashboardView. Wee!

### Logging Out

Logging out is real simple. In our **DashboardView.js:**
``` javascript
    // DashboardView.js

    import React, { Component } from 'react';
    import {
      AppRegistry,
      StyleSheet,
      Text,
      View,
    } from 'react-native';
    import LoginView from './LoginView'

    export default class DashboardView extends Component {

    render() {
        return (
          <View style={styles.container}>
          </View>
        );
      }
    }

    const styles = StyleSheet.create({
      container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#F5FCFF',
      },
      welcome: {
        fontSize: 20,
        textAlign: 'center',
        margin: 10,
      },
    });
```
We clear the cookies, and take them to the login. If you refresh the app with
Cmd + R after doing this, you’ll notice you are no longer taken to the
DashboardView- you’ve been kicked out! But if you login again and refresh, all
is gravy.

### Summing Up

Our app isn’t much to look at, but in building it we covered a lot of the basic
concepts of React Native. Hopefully you have a good idea of how to go from here-
how to add more Views, built with RN components.

Once you’re comfortable with RN components, the routing, and the debugging,
React Native will feel very similar to building a web app- in many ways, it’s
easier and faster to develop with.

Found this tutorial helpful? Do me a favour and hit the green heart below- much
appreciated.

*Thanks to *[Mybridge](https://medium.mybridge.co/@Mybridge)* for including this
article as one of the top ten React.js articles of November. See the full list
*[here](https://medium.mybridge.co/react-js-top-10-articles-in-november-b3c378ae8daa#.itu5da1nx)*.*

* [React Native](https://engineering.musefind.com/tagged/react-native?source=post)
* [JavaScript](https://engineering.musefind.com/tagged/javascript?source=post)
* [Mobile App
Development](https://engineering.musefind.com/tagged/mobile-app-development?source=post)
* [iOS App
Development](https://engineering.musefind.com/tagged/ios-app-development?source=post)
* [React](https://engineering.musefind.com/tagged/react?source=post)

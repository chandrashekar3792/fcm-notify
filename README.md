
fcm-notify
========
A Node.JS simple interface to Google's Firebase Cloud Messaging (FCM). Supports both android and iOS, including topic messages, and parallel calls.  
Aditionally it also keeps the callback behavior for the new firebase messaging service. 
## Installation

Via [npm][1]:

    $ npm install fcm-notify

## Usage

There are 2 ways to use this lib:
### The **classic** one 
   1. Generate a **Server Key** on your app's firebase console and pass it to the **FCM** constructor
   2. Create a _message object_ and call the **send()** function
#### Classic usage example:
```js
    var FCM = require('fcm-notify');
    var serverKey = 'YOURSERVERKEYHERE'; //put your server key here
    var fcm = new FCM(serverKey);

    var message = { //this may vary according to the message type (single recipient, multicast, topic, et cetera)
        to: 'registration_token', 
        collapse_key: 'your_collapse_key',
        
        notification: {
            title: 'Title of your push notification', 
            body: 'Body of your push notification' 
        },
        
        data: {  //you can send only notification or only data(or include both)
            my_key: 'my value',
            my_another_key: 'my another value'
        }
    };
    
    fcm.send(message, function(err, response){
        if (err) {
            console.log("Something has gone wrong!");
        } else {
            console.log("Successfully sent with response: ", response);
        }
    });
```

### The **new** one 
   1. Go to your [Service account tab][13] in your project's settings and download/generate your app's private key. 
   2. Add this file in your project's workspace
   3. Import that file with a `require('path/to/privatekey.json')` style call and pass the object to the **FCM** constructor
   4. Create a _message object_ and call the **send()** function
   
#### "New" usage example
```js
    var FCM = require('fcm-notify')
    
    var serverKey = require('path/to/privatekey.json') //put the generated private key path here    
    
    var fcm = new FCM(serverKey)

    var message = { //this may vary according to the message type (single recipient, multicast, topic, et cetera)
        to: 'registration_token', 
        collapse_key: 'your_collapse_key',
        
        notification: {
            title: 'Title of your push notification', 
            body: 'Body of your push notification' 
        },
        
        data: {  //you can send only notification or only data(or include both)
            my_key: 'my value',
            my_another_key: 'my another value'
        }
    }
    
    fcm.send(message, function(err, response){
        if (err) {
            console.log("Something has gone wrong!")
        } else {
            console.log("Successfully sent with response: ", response)
        }
    })
```

## Topic subscription on web clients

Web clients doesn't have a "native" way to subscribe/unsubscribe from topics other than manually requesting, managing and registering with the google's iid servers. To resolve this "barrier" your server can easily handle the web client's sub/unsub requests with this lib.

For more detailed information, please take a look at [Google InstanceID Reference][14].

*PS: For mobile clients you can still use the native calls to subscribe/unsubscribe with one-liner calls*
##### Android
```java
FirebaseMessaging.getInstance().subscribeToTopic("news");
```
##### iOS
```objective-c
[[FIRMessaging messaging] subscribeToTopic:@"/topics/news"];
```



### Subscribe Device Tokens to Topics

```js
var FCM = require('fcm-notify');
var serverKey = 'YOURSERVERKEYHERE'; //put your server key here
var fcm = new FCM(serverKey);

fcm.subscribeToTopic([ 'device_token_1', 'device_token_2' ], 'some_topic_name', (err, res) => {
    assert.ifError(err);
    assert.ok(res);
    done();
});
```

### Unsubscribe Device Tokens to Topics

```js
var FCM = require('fcm-notify');
var serverKey = 'YOURSERVERKEYHERE'; //put your server key here
var fcm = new FCM(serverKey);

fcm.unsubscribeToTopic([ 'device_token_1', 'device_token_2' ], 'some_topic_name', (err, res) => {
    assert.ifError(err);
    assert.ok(res);
    done();
});

```

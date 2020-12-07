
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
var FCM = require('fcm-node');
var serverKey = 'YOURSERVERKEYHERE'; //put your server key here
var fcm = new FCM(serverKey);

fcm.unsubscribeToTopic([ 'device_token_1', 'device_token_2' ], 'some_topic_name', (err, res) => {
    assert.ifError(err);
    assert.ok(res);
    done();
});

```


## Changelog
1.5.2 - fixed bug whre send's callback were called twice - *Thanks to [@cesardmoro][17] for this fix*
1.3.0 - Added proxy capabilities - *Thanks to [@crackjack][16] for this feature*
1.2.0 - Added topic subscriptions management for web clients - *Thanks to [@sofiapm][15] for this feature*   
1.1.0 - Support for the new firebase node.js sdk methods  
1.0.14 - Added example file to quick tests <br />
1.0.13 - Added a error response in case of TopicsMessageRateExceeded response <br />
1.0.12 - Refactored the client removing the Event Emitter's Logic to fix concurrency issues. Using pure callbacks now also avoids memory leak in specific scenarios with lots of parallel calls to <b>send</b> function. <br />
1.0.11 - \<FIX\> send function returning error objects when multicast messages (or individually targeted) returned both error and success keys on response message (even with error counter = 0 ) <br /> 
1.0.9 - Updated Documentation <br />
1.0.8 - \<FIX\> 'icon' field no longer required in notification<br /> 
1.0.7 - renaming repository<br />
1.0.6 - bugfix: send function was always returning an error object for multicast messages (multiple registration ids)<br />
1.0.5 - bugfix with UTF-8 enconding and chunk-encoded transfers<br />
1.0.1 - forked from fcm-push and extended to accept topic messages without errors<br />

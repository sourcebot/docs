### SlackBot
* ```constructor(opts)```
   * Constructs the SlackCore class with ```opts.token```
   * If ```opts.debug``` is defined, SlackBot will enter in debug mode.
* ```connect()```
   * Connects to Slack Web API
* ```requestSlack()```
   * Returns Slack API endpoint.
        * ```rtmStart()```
        * ```getChannelInfo(channelId)```
        * ```getUserInfo(userId)```
        * ```openDirectMessageChannel(userId)```

### Bot
* ```listen(message, callback)```
  * Listens for the message. The message can be an instance of RegExp or a plain String. Returns promise containing the response.
* ```send(opts)```
  * Sends a message to specified channel, Takes ```opts``` object as a parameter containing text and channel fields. Returns empty promise.
* ```startConversation(channelName, userId)```
  * Starts a conversation with the specified user in a specified channel. Takes user's slack id and the id of the channel. Returns promise containing a ```conversation``` object.
* ```startPrivateConversation(user)```
  * Starts private conversation between a user. Returns promise containing a ```conversation``` object.
* ```disconnect()```
  * Disconnects and removes all event listeners.

### Conversation
* ```ask(opts||message, callback)```
  * Sends the given ```opts``` Object or  ```question``` String and waits for a response. If ```opts.replyPattern``` is provided asks until the RegExp test succeeds, fires callback upon faulty replies with the ```faultyReply```. Returns a promise containing the ```response```.
  * ```let opts = {text: 'Question', replyPattern: new RegExp('')}```
* ```say(message)```
  * Sends the given String ```message```. Returns empty promise.
* ```askSerial(opts)```
  * Behaves same as ```ask()``` but this method takes an array of objects that are asked sequentially.

Optional values:
```javascript
let opts = {
   text: 'Question',
   replyPattern: new RegExp(''),
   callback: (faultyReply) => {
     return Promise.resolve()
   }
 }
```

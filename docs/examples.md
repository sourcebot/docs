# Examples

For full documentation visit [github.com/sourcebot](https://github.com/sourcebot).

## Hello World

```javascript
let SlackCore = require('sourcebot').Slack;
let SlackBot = new SlackCore({
  token: 'xoxb-17065016470-0O9T0P9zSuMVEG8yM6QTGAIB'
});


SlackBot
  .connect()
  .then((bot) => {
    bot
      .listen('hello', (response) => {
        bot.send({
          channel: response.channel,
          text: 'world'
        });
      })
  })
  .catch((err) => console.error(err.message))
```

## Conversation

```javascript
SlackBot
  .connect()
  .then((bot) => {
    bot
      .listen(new RegExp('start convo', 'i'), (response) => {

        bot
          .startConversation(response.channel, response.user)
          .then((conversation) => {
            return conversation
              .ask('How are you?')
              .then((reply) => {
                return conversation
                  .say('Good!')
                  .then(() => {
                    return conversation.ask('What are you doing now?')
                  })
                  .then((response) => {
                    return conversation.askSerial(['What?', 'Where?', 'When?']);
                  })
              })
          });
      })
  }).catch((err) => console.error(err.message));
```

## Private Conversation


```javascript
SlackBot
  .connect()
  .then((bot) => {
    bot
      .listen(new RegExp('start convo', 'i'), (response) => {
        bot
          .startPrivateConversation(response.user)
          .then((conversation) => {
            conversation
              .ask('Hello world')
              .then((response) => {
                conversation.say('You said ' + response.text);
              })
          })
      })
  }).catch((err) => console.error(err.message));

```

## Slack's API

```javascript
SlackBot
  .connect()
  .then((bot) => {
    bot
      .listen(new RegExp('start convo', 'i'), (response) => {

        SlackBot
          .requestSlack()
          .getChannelInfo(response.channel)
          .then((channelInfo) => {
            bot.send({
              channel: response.channel,
              text: 'Wow, wow, wow! We have ' + channelInfo.channel.members.length + ' users in here!'
            });

            const tasks = channelInfo.channel.members.map((member) => {
              return SlackBot.requestSlack().getUserInfo(member)
            });

            Promise
              .all(tasks)
              .then((users) => {
                users.forEach((item) => {
                  bot.send({
                    channel: response.channel,
                    text: 'Welcome <@' + item.user.id +'|' + item.user.name +'>, I\'ve missed you!'
                  });
                })
              })
          })
      })
  }).catch((err) => console.error(err.message));
```

## Serial Questions



```javascript
SlackBot
  .connect()
  .then((bot) => {
    bot
      .listen(new RegExp('start convo', 'i'), (response) => {

        bot
          .startConversation(response.channel, response.User)
          .then((conversation) => {
            return conversation
              .askSerial([
                {
                  text: 'How are you?',
                  replyPattern: new RegExp('\\bfine\\b', 'i') //Asks until the given response contains 'fine'.
                },
                {
                  text: 'Where are you?',
                  replyPattern: new RegExp('\\bistanbul\\b', 'i'),
                  callback: (faultyReply) => { //Fires up if the response does not contain 'istanbul'.
                    return conversation.say('Please indicate your city.');
                  }
                }
              ]).then((responses) => {
                console.log(responses);
              });
          });
      })
  }).catch((err) => console.error(err.message));
```

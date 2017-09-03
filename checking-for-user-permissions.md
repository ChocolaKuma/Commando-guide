# User Permissions

In the last chapter we talked about how to check for bot permissions, in this chapter, we're going to be using a `hasPermission` method to check whether or not the user is the owner before using the command.

The `hasPermission` method can be anything you want, as long as it returns `true` or `false`. When it returns `true`, the command will run. When it returns `false`, the user will not be allowed to use the command.

Doing this is quite simple.

```js
hasPermission(msg) {
    return this.client.isOwner(msg.author);
}
```

This will return `false` if the user is not the owner of the bot and `true` if they are.

The `hasPermission` method should be placed separate from the `run` method, like so \(if you wanted your say command to only be usable by the owner\):

```js
const { Command } = require('discord.js-commando');

module.exports = class SayCommand extends Command {
    constructor(client) {
        super(client, {
            name: 'say',
            aliases: ['copycat', 'repeat', 'echo', 'parrot'],
            group: 'group2',
            memberName: 'say',
            description: 'Replies with the text you provide.',
            examples: ['say Hi there!'],
            guildOnly: true,
            args: [
                {
                    key: 'text',
                    prompt: 'What text would you like the bot to say?',
                    type: 'string'
                }
            ]
        });    
    }

    hasPermission(msg) {
        return this.client.isOwner(msg.author);
    }

    run(msg, args) {
        const { text } = args;
        msg.delete();
        return msg.say(text);
    }
};
```

Simple.

And remember, it can be anything that returns true or false. For example, if we wanted to check if the user has the Ban Members permission:

```js
hasPermission(msg) {
    return msg.member.hasPermission('BAN_MEMBERS');
}
```

_Anything_ that returns `true` or `false` can be used.

Also, as of the newest update, we can also return a string for a custom response.

```js
hasPermission(msg) {
    if (!msg.member.hasPermissions('BAN_MEMBERS')) return 'You do not have the `Ban Members` Permission';
    else return true;
}
```

Neat, huh?


# twinetext
A Twine Sugarcube template for creating text-message-based interactive stories.


## Using This Template

Firstly, the **entire** template must be copied and used as boilerplate. This ensures that the special Passages (`StoryInit`, `PassageHeader` and so on) are included, along with their tags.

You should familiarise yourself with the [Sugarcube](https://www.motoslave.net/sugarcube/2/docs/) format for Twine 2, on which this template is based.

When creating new Passages based on the templates, make sure that you also replicate their tags, as these are important to the functionality. 

All CSS is included in the onboard Stylesheet, and can be tweaked to your heart's content. This template is designed for phone screens, and may look a bit janky on desktop. 

#### 1) Setting Up Using StoryInit

The `StoryInit` Passage is used to set up the initial list of chats, and their initial messages (i.e. those messages that are already in the chat when the player loads the game). 

You can set the name of the chat app itself with the `$appName` variable.

Define all your messages in the `$messageList` dictionary, using this template for each entry:

```
 1: { id: 1, name: "Jane", chatimage: "https://example.com/1", latestmsg: 1, lastread: 0,
        messages: [
            { id: 1, who: "player", text: "When's the car boot sale?" }
        ]
    }
```

As you can see, each message has:
- A unique `id`;
- A character `name` (who the player is chatting with);
- A profile picture for this character, saved as a URL (`chatimage`);
- An id of the latest message (`latestmsg`) that exists in the chat;
- The id of the `lastread` message.

There is also an array of existing `messages`, with:
- Again, a unique `id`;
- Whether the `character` or `player` sent it (`who`);
- The `text` of the actual message.

#### 2) The Menu Screen Passage

This needs the `<<menuScreen>><<menuScreen>>` custom widget, and nothing else. You can see how the widget is constructed in the `Custom Widgets` Passage, but it shouldn't need to be touched. 

The message list refreshes every second, so newly added Messages (see below) appear in the list.

#### 3) The Chat Screen Passage

This should only  contain `<<chatScreen $chattingwith>><</chatScreen>>`: the actual crafting of content happens in the `Current Chat` Passage.

#### 4) The Current Chat Passage

This is where the actual chatting is done - where you use custom widgets to define which messages arrive from characters, and how the player can respond.

There are three ways you can use this Passage:

1) Allow the player to send a free-written text to a character at any time, using the `<<textEntry>>` widget;
2) Only allow a player to respond to a message by choosing between pre-written responses, using the `widget` widget;
3) A mixture of the two, in which the `<<textEntry>>` widget is present at all times, apart from when a specific choice is to be made with the `widget` widget.

Note that there is no logic wired up for dealing with free-written texts from the player: that's on you!

You can also use conditionals and other variables to control exactly what appears in this Passage - think of it as where the player interacts with whatever the current correspondent is. 

##### Widgets

`<<addMessage>>`

Parameters:
- The name of the character in whose chat you want to add a message (make sure you use the full name as it appears in the data structure);
- Whether it is a message from the `"player"` or the `"character"`;
- The text of the message itself.

This adds a new message to the list of messages in one particular chat - it can either be from the player, or the character. It can be called anywhere, updating the list of messages globally.

`<<addMessageContact>>`

Parameters:
- The name of the character (as above);
- A URL linking to the character's profile image;
- The message text;
- Whether it is a message from the `"player"` or the `"character"` that starts the chat.

This works as above, except it allows you to add a new chat, with a new character, with an opening message. 

`<<addMessageInChat>>`

Parameters:
- The message text:
- How long it takes the character to type the message (expressed as seconds e.g. `"1s"`).

This is a way to add a Message in the current chat, simulating the character 'typing' for a number of seconds.


`




All message sending logic in the game itself is done in Passages whose Titles are then referenced by the script.
When players send a message, or make a choice as to message from the list, it is added to the messages list, then the latestmsg and last read are updated.
Characters can also send messages using the sendAsyncMessage widget, which updates the latestmsg without updated the last read.



- The <<textChoices>> macro has only one parameter: the delay (from the moment the Screen is loaded) before it appears. The choices are written inside, as <<chatChoice>> macros .  There should be space for two choices maximum. 

- The <<chatChoice>> macro has the following parameters:
	- The Passage that clicking a choice goes to. If you do not want to immediately go to another Passage, just write "".
    - The variable, and its value, that you want to set by clicking this choice. Again, these two parameters are optional: if you don't want to set a variable, just write both as "".
    - You can also optionally have a message from the player display after the choice has been clicked. If you want this, write the content of the message in the parameter; otherwise, leave it as "".
    - The text of the choice button itself.
    - A unique ID: should either be "choice1" or "choice2".

- If you don't want the choice to go to another Passage, you can optionally add more content inside the <<chatChoice>> macro, which will only display if that choice is clicked. This allows you to carry on a conversation between a player and a character depending on the player's choices. 

----


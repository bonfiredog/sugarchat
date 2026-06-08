<img src="http://bonfiredog.co.uk/resources/twinetext/samplelogo.png" style="width:20%;">

# `sugarchat`
A Twine Story template (Sugarcube) for creating interactive stories in a text messaging app interface. Players can interact with characters through choice-based selection or free text entry. Characters can send text, emojis and media. Can be integrated into a larger Twine work, or used for standalone projects.  

## Screenshots
<div style="display:flex;flex-direction:row;gap:1%;">
<img src="http://bonfiredog.co.uk/resources/twinetext/ss1.png" style="width:20%;" />

<img src="http://bonfiredog.co.uk/resources/twinetext/ss2.png" style="width:20%;" />

<img src="http://bonfiredog.co.uk/resources/twinetext/ss3.png" style="width:20%;" />

<img src="http://bonfiredog.co.uk/resources/twinetext/ss4.png" style="width:20%;" />
</div>

## Using This Template
Import the sample HTML file to your Twine 2 Story List, ensuring that you have the **Sugarcube** story format selected. Don't delete anything unless you know what you are doing, as all the Passages (in particular the special Passages `StoryInit`, `PassageHeader` and so on) are there for a reason.

When creating new Passages based on the templates, make sure that you also replicate their tags, as these are important to the functionality. 

You can play the sample as-is, and the Passage code is annotated so you can see how the samples work.

I've started puzzling out some 'flows' for particular types of interaction, but it's fairly minimal. You'll be making good use of variables and the `<<timed>>` macro to key messages up in order to construct an actual narrative, but I have tried to make the template as flexible as possible. 

Knowledge of the [Sugarcube](https://www.motoslave.net/sugarcube/2/docs/) story format for Twine 2 is vital.

All CSS is included in the embedded stylesheet, and can be tweaked to your heart's content. This template is designed for phone screens, and may look a bit janky on desktop. 

## Full Tutorial
### StoryInit
Set the name of your messaging app (`$appName`) and the `$playerName` here.

You should also create the initial `$messagelist`: a data structure that includes all the characters and their chat history that are *present at the start of the game*. The samples should be self-explanatory, but each character needs the following:

- A unique `id` (starting at 0);
- A unique `name`;
- A URL linking to a profile picture (`chatimage`);
- The `id` of the `latestmsg` (set this to the `id` of the latest message in the character's `messages` Array);
- The `id` of the `lastread` message (you can set this to be lower than the `id`, and a notification badge will appear in the message list when the player starts the game).
- A `messages` array, which contains a list of messages with the following variables:
	- Again, an `id` (starting at 0);
 	- A `who` marker (whether it is from the `"char"` or the `"player"`);
  	- The `text` of the message.
  	- An (optional) `attach` value, which defines any attachments that are part of messages. These need to be written as HTML markup (i.e. in an `<img`>, `<audio>`, `<video>` tag). I've only tested the appearance of images, so you may need to add some CSS to make audio and video look good.

### Menu Screen
Use the template as-is: The  `<<menuScreen>><<menuScreen>>` widget, with only a single parameter pointing to the URL of your fictional app's logo.

### Chat Screen
Again, the template provided shouldn't need editing, just include it as-is.

### Current Chat
When the player selects a chat to read, the `<<chatscreen>>` widget populates this Passage with all the messages (from both the character and the player) included in the `messages` array for that character. 

Other than this, all the functionality and interaction for the current chat is defined in the `Current Chat` passage. Any messages you add using the widgets below are added to the message list database for that character.

The sample contains some examples of how you might structure this: using conditionals to create different flows depending on which character you are currently chatting with, and where in the story you are. 

This template deliberately avoids defining story logic: this is up to you, and can be integrated into this template.

### Widget List

`addMessage(name, who, text, attach)`
This adds a new message to a chat that *you are not currently reading* (it doesn't work well for adding messages to the currently-open chat). Use `name` to identify which chat, `who` to define whether it's a message from the `char` or the `player`, and the `text` to set the actual content of the message. You can use the `$chattingwith` variable if you want to add a message to the currently-selected character. The `attach` is optional (see above).

**Example:** `<<addMessage "John" "char" "Are you there?" "<img src='http://example.com/img/meme.png'>">>`

---

`addMessageContact(name, chatimage, firstmessage, who, attach)`
This adds a message from a new contact, creating a new chat for them. Works the same as `addMessage`, but remember to set the URL to the `chatimage` for this new character. Also remember that the `name` must be unique. The `attach` is optional (see above).

**Example:** `<<addMessageContact "Sirius Ventures" "http://example.com/img/sirius.png" "Is this Henry? Your offer is waiting!" "char">>`

---

`addMessageInChat(text, typedelay, attach)`
This is used to add a message in the current open chat (no matter which character). You set the `text` of the message as usual, and you can also set an optional `typedelay` in seconds, which shows an indicator that the character is typing. It may be worth wrapping these in a `<<timed>>` widget, I seem to get better results from this. The `attach` is optional (see above).

**Example:** `<<addMessageInChat "Are you coming out tonight?" 2s>>`

---

`<<textEntry>><<textEntry>>`
Include this in CurrentChat to allow the player to freely type a message to the character. No logic hooked up to handle responses.

---

`playerChoices`
This widget allows the player to choose from a number of predefined options. Selecting one of these will send a particular message to the character.
Use the `chatchoice` widget to create the options, with a label. Any logic (such as the message to add in the chat, variable changes or any subsequent responses from the character) can be included inside the `chatchoice`.

**Example:**

```
<<playerChoices>>

<<chatchoice "Lie">>
<<addMessage $chattingwith "player" "I wasn't there. I was with Hannah.">>
<<includes "Lie Consequence">>
<</chatchoice>>

<<chatchoice "Flatter">>
<<addMessage $chattingwith "player" "Do you really think I would do that to you? I love you.">>
<<includes "Flatter Consequence">>
<</chatchoice>>

<<chatchoice "Insult">>
<<addMessage $chattingwith "player" "You're pathetic.">>
<<includes "Insult Consequence">>
<</chatchoice>>

<</playerChoices>>
```


## License
This is free and unencumbered software released into the public domain.

Anyone is free to copy, modify, publish, use, compile, sell, or
distribute this software, either in source code form or as a compiled
binary, for any purpose, commercial or non-commercial, and by any
means.

In jurisdictions that recognize copyright laws, the author or authors
of this software dedicate any and all copyright interest in the
software to the public domain. We make this dedication for the benefit
of the public at large and to the detriment of our heirs and
successors. We intend this dedication to be an overt act of
relinquishment in perpetuity of all present and future rights to this
software under copyright law.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS BE LIABLE FOR ANY CLAIM, DAMAGES OR
OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

For more information, please refer to <https://unlicense.org/>



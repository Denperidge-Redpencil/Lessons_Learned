# Learning.md: Integrating Rocketchat & Matrix
[blog announcement](https://matrix.org/blog/2022/05/30/welcoming-rocket-chat-to-matrix) - [docs](https://docs.rocket.chat/guides/administration/admin-panel/settings/federation/matrix-bridge)

Moving platforms sucks. Because there's no good way to do it. People are used to the workflows and UI's of the previous thing, and may not like the new thing. But using two different but functionally similar platforms is how you get miscommunications. This bridge might allow a slow transition! I wanna test if this perhaps could even import previous conversations. ~~If you're reading this and it hasn't been strikethroughed, I haven't yet.~~ But either way, it's worth checking out.

## Update: I have found things

See [denperidge-redpencil/rockatrix](https://github.com/Denperidge-Redpencil/rockatrix) to test it out for yourself!

---

## Matterbridge
As of writing, I *have* succesfully been able to integrate this.
I've screen recorded the footage below, but I'll summarise my findings here.

*tl;dr: no message history, new messages & message edits get synced with barely any delay and only a few issues/quirks.* 


- It works by making a gateway between two specific channels/rooms, and will send new messages to the other platform near immmediately. It will show a note of the source protocol as well as the username of who sent it, and of course the message contents.
- Attachments get synced as well.
- Message edits in matrix get synced to rocketchat, but rocketchat not to matrix. See [the docs](https://github.com/42wim/matterbridge/wiki/Features#message-edits-and-deletes) for more information.
- Formatting *generally* gets copied over well. Some issues with # headings, but **bold** and *italics* seem to go well both ways. 
- Besides making a bot user on both Rocket.chat & Matrix, there is no setup needed within the services in question.
- It needs to be actively run, and does *not* import the message history.
- Threads don't work but also don't not work. The messages still get sent, but not put in a thread on the other platform (they will just appear below).
- You can seemingly set up as many channel connections as wanted.
- The matrix & rocket.chat channel are allowed to have different names (cool), but need to be explicitly defined (less cool). There doesn't seem to be a way to say "if something is sent in any channel, send it to the channel with the same name on the other platform".

<video width="640" height="360" src="https://github.com/Denperidge-Redpencil/Learning.md/assets/27348469/424cb716-87f3-434d-93d0-8e16582e4dac" controls></video>


[Link for if the above footage doesn't embed](/assets/learning-md/Rocketchat-Matrix-Matterbridge-v2.mp4) - [Uncompressed footage](/assets/learning-md/Rocketchat-Matrix-Matterbridge-v2-uncompressed.mkv) - [First footage recording](/assets/learning-md/Rocketchat-Matrix-Matterbridge-v1.webm)

---

## Official bridge
As of writing, I have *not* succesfully integrated this yet. But I have tried. A lot. Automated setup's Traefik kept getting permission denied, manual setup didn't give errors but also no results.

### Rocketchat supports matrix but with an asterisk
- Some federated functionality is [locked behind enterprise editon and/or not encryptable](https://docs.rocket.chat/guides/administration/admin-panel/settings/federation/matrix-bridge/matrix-users-guide/create-a-federated-rooms#creating-a-multi-user-direct-message-using-slash-command-enterprise-edition-only)
- The Rocket.chat settings say the following: `No user should connect to the homeserver with third party clients, only Rocket.Chat`. I don't know if this refers to the people wanting to send messages through rocket.chat itself to Matrix users, or if they mean that all Matrix traffic should go through Rocket.chat.
    - The [docs](https://docs.rocket.chat/guides/administration/admin-panel/settings/federation/matrix-bridge/matrix-admin-guide/matrixbridge-configuration) say `We strongly recommend not connecting to this Matrix homeserver using other Matrix clients, if you want to do that, please configure another Matrix homeserver instance.`.
- When enabing federated functionality:
  ```
    From now on, you can invite federated users only to private rooms or discussions.
    Those channels are going to be replicated to the remote server, without the message history.
  ```

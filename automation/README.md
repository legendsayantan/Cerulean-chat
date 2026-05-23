## Automation via Broadcasts

You can control Cerulean externally by sending broadcast intents to the app. To successfully execute a broadcast action, Cerulean must be running, and you must authenticate the intent using your session ID and receiver key.

#### How to Send Broadcasts
You can trigger these intents programmatically via your own code, or by using popular Android automation applications. If you prefer a no-code approach, check out these apps on the Google Play Store:

* [Automate](https://play.google.com/store/apps/details?id=com.llamalab.automate)
> Example use cases are provided [here](https://github.com/legendsayantan/Cerulean-chat/tree/main/automation/automate)
* [MacroDroid](https://play.google.com/store/apps/details?id=com.arlosoft.macrodroid)
> Example use cases are provided [here](https://github.com/legendsayantan/Cerulean-chat/tree/main/automation/macrodroid)
* [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm)
> Example use cases are provided [here](https://github.com/legendsayantan/Cerulean-chat/tree/main/automation/tasker)

#### Configuration
You should send the broadcast to the following package :
```
dev.legendsayantan.cerulean
```
and the following receiver :
```
dev.legendsayantan.cerulean.broadcast.Receiver
```

#### Authentication Parameters
Every broadcast intent sent to Cerulean **must** include the following string extras for security validation:

| Extra Key | Type   | Description |
| :--- | :--- | :--- |
| `session` | String | Your active session ID, generally your country code + phone number (no symbols, no spaces). |
| `key`     | String | The broadcast receiver key found inside Automation Settings of Cerulean. |

---

#### Action: Send Message
**Intent Action:** `dev.legendsayantan.cerulean.send_message`

This action allows you to send text or media messages to a specific recipient. 

**Parameters Grid:**

| Extra Key / Data | Type | Required For | Description |
| :--- | :--- | :--- | :--- |
| `recipient` | String | All messages | The recipient's JID (Jabber ID), visible when you search for someone. |
| `msgtype` | String | All messages | The type of message to send. Accepted values: `text`, `image`, `audio`, `video`, or `document`. |
| `caption` | String | `text` | The actual text content of the message. For media, this acts as an optional caption. |
| `reply` | String | Optional | Use this to pass a message ID if you want to reply to a specific message. |
| `file_name` | String | Media messages | The name of the file being sent (Required for `image`, `audio`, `video`, and `document`). |
| `Intent.data` | URI | Media messages | The URI of the file to be uploaded. Passed as the primary data URI of the intent, not as an extra. |

---

#### Action: Adjust Settings
**Intent Action:** `dev.legendsayantan.cerulean.adjust_settings`

*(Note: This action does not do anything yet, coming soon.)*

### Listening to Cerulean Events

Cerulean can also broadcast its own internal events (such as new messages or status updates) to other apps. To receive these broadcasts, you must first explicitly enable the specific event types from the **Automation Settings** page within the Cerulean app.

#### Chat Event Broadcasts
When chat-related events occur, Cerulean fires the following intents:

**Intent Actions:**
* `dev.legendsayantan.cerulean.broadcast.new_message`
* `dev.legendsayantan.cerulean.broadcast.updated_message`
* `dev.legendsayantan.cerulean.broadcast.message_reaction`
* `dev.legendsayantan.cerulean.broadcast.message_receipt`
* `dev.legendsayantan.cerulean.broadcast.deleted_message`

**Chat Event Payload (Extras):**
Depending on the event, the broadcast may contain the following extras:

| Extra Key | Type | Description |
| :--- | :--- | :--- |
| `session` | String | The session ID emitting the event. |
| `chat` | String | The JID of the chat where the event occurred. |
| `id` | String | The unique message ID. |
| `sender` | String | The JID of the message sender. |
| `sender_name` | String | The visible contact name of the sender (if available). |
| `type` | String | The type of message (e.g., text, image). |
| `caption` | String | The text content or media caption. |
| `media_path` | String | Local path to the downloaded media file. |
| `quoted` | String | A preview of the quoted text, if the message is a reply. |
| `viewed` | Boolean | Returns true if the message has been viewed by you. |
| `receipts` | String Array | Array containing receipt data strings. |
| `reactions` | String Array | Array containing reaction data strings. |
| `conversation` | String | Previous conversation context (Only included if enabled in Automation Settings). |

---

#### Status Event Broadcasts
When status-related events occur, Cerulean fires the following intents:

**Intent Actions:**
* `dev.legendsayantan.cerulean.broadcast.new_status`
* `dev.legendsayantan.cerulean.broadcast.updated_status`
* `dev.legendsayantan.cerulean.broadcast.status_reaction`
* `dev.legendsayantan.cerulean.broadcast.status_receipt`
* `dev.legendsayantan.cerulean.broadcast.deleted_status`

**Status Event Payload (Extras):**
Depending on the event, the broadcast may contain the following extras:

| Extra Key | Type | Description |
| :--- | :--- | :--- |
| `session` | String | The session ID emitting the event. |
| `sender` | String | The JID of the user who posted the status. |
| `id` | String | The unique status ID. |
| `status_data` | String | Stringified details of the status update. |
| `receipt_data` | String | Stringified details of the status receipt. |
| `reactions` | String Array | Array containing reaction strings on the status. |

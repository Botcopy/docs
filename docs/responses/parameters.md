## Dialogflow Parameters Sent by Botcopy

Botcopy sends the following session parameters attached to every query it makes to Dialogflow CX and ES.

### Dialogflow CX

| **Name**       | **Description**                                | **Example Value**                                                             | **Sent by Default** |
| -------------- | ---------------------------------------------- | ----------------------------------------------------------------------------- | ------------------- |
| `bcBotId`      | The bot ID.                                    | `"63af7c3c8123ff00084e336b"`                                                  | Yes                 |
| `bcCurrentUrl` | The origin, pathname, and href of current URL. | `{origin: "https://test.com", path: "/hello", url: "https://test.com/hello"}` | Yes                 |
| `bcTime`       | The current time relative to the user.         | `"11:01:37 AM"`                                                               | Yes                 |
| `bcTimeZone`   | The timezone the user is in.                   | `"America/Chicago"`                                                           | Yes                 |
| `bcOriginIp`   | The IP address of the user.                    | `"123.123.123.123"`                                                           | No                  |
| `bcIsMobile`   | Identifies if a user is using a mobile device. | `true`                                                                        | No                  |

### Dialogflow ES

| **Name**              | **Description**                                     | **Example Value**                                                             | **Sent by Default** |
| --------------------- | --------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------- |
| `botcopy-botid`       | The bot ID.                                         | `{botId: "63af7c3c8123ff00084e336b"}`                                         | Yes                 |
| `botcopy-current-url` | The origin, pathname, and href of current URL.      | `{origin: "https://test.com", path: "/hello", url: "https://test.com/hello"}` | Yes                 |
| `botcopy-timezone`    | The current time and timezone relative to the user. | `{time: "11:01:37 AM", tz: "America/Chicago"}`                                | Yes                 |
| `botcopy-is-mobile`   | Identifies if a user is using a mobile device.      | `{isMobile: "true"}`                                                          | No                  |

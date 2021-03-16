# SMTP to Telegram


[![Docker Hub](https://img.shields.io/docker/pulls/elzurdo87/smtp_to_telegram_rpi.svg?style=flat-square)][Docker Hub]



[Docker Hub]:      https://hub.docker.com/r/elzurdo87/smtp_to_telegram_rpi

`smtp_to_telegram` is a small program which listens for SMTP and sends
all incoming Email messages to Telegram.

Say you have a software which can send Email notifications via SMTP.
You may use `smtp_to_telegram` as an SMTP server so
the notification mail would be sent to the chosen Telegram chats.

## Getting started

1. Create a new Telegram bot: https://core.telegram.org/bots#creating-a-new-bot.
2. Open that bot account in the Telegram account which should receive
   the messages, press `/start`.
3. Retrieve a chat id with `curl https://api.telegram.org/bot<BOT_TOKEN>/getUpdates`.
4. Repeat steps 2 and 3 for each Telegram account which should receive the messages.
5. Start a docker container:

```
docker run \
    --name smtp_to_telegram_rpi \
    -e ST_TELEGRAM_CHAT_IDS=<CHAT_ID1>,<CHAT_ID2> \
    -e ST_TELEGRAM_BOT_TOKEN=<BOT_TOKEN> \
    elzurdo87/smtp_to_telegram_rpi
```

Assuming that your Email-sending software is running in docker as well,
you may use `smtp_to_telegram:2525` as the target SMTP address.
No TLS or authentication is required.

The default Telegram message format is:

```
From: {from}\\nTo: {to}\\nSubject: {subject}\\n\\n{body}
```

A custom format might be specified as well:

```
docker run \
    --name smtp_to_telegram_rpi \
    -e ST_TELEGRAM_CHAT_IDS=<CHAT_ID1>,<CHAT_ID2> \
    -e ST_TELEGRAM_BOT_TOKEN=<BOT_TOKEN> \
    -e ST_TELEGRAM_MESSAGE_TEMPLATE="Subject: {subject}\\n\\n{body}" \
    elzurdo87/smtp_to_telegram_rpi
```

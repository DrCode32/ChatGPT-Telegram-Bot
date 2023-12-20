<p align="center">
<img src="./assets/logo-3071751.jpg">
</p>

# ChatGPT Telegram Bot

<p align="center">
    <a href="https://hub.docker.com/repository/docker/yym68686/chatgpt">
    <img src="https://img.shields.io/docker/pulls/yym68686/chatgpt?color=blue" alt="docker pull"></a>
</p>

The ChatGPT Telegram Bot is a powerful Telegram bot that utilizes the latest GPT models, including GPT3.5, GPT4, GPT4 Turbo, GPT4 Vision, DALLE 3, and the official Claude2.1 API. It enables users to engage in efficient conversations and information searches on Telegram.

Join the [Telegram Group](https://t.me/+_01cz9tAkUc1YzZl) chat to share your user experience or report Bugs.

## ✨ Features

- **Multiple AI Models**: Integrates a variety of AI models including GPT3.5, GPT4, GPT4 Turbo, GPT4 Vision, DALLE 3, and Claude2.1 API.
- **Powerful Online Search**: Supports online search with DuckDuckGo and Google, providing users with a robust information retrieval tool.
- **User-friendly Interface**: Allows flexible model switching within the chat window and supports streaming output for a typewriter-like effect.
- **Efficient Message Processing**: Asynchronously processes messages, answers questions in a multi-threaded manner, supports isolated dialogues, and provides unique dialogues for different users.
- **Document Interaction**: Supports Q&A for PDF and TXT documents. Users can upload files directly in the chat box for use.
- **Accurate Markdown Rendering**: Supports precise Markdown rendering of messages, utilizing another [project](https://github.com/yym68686/md2tgmd) of mine.
- **Convenient Deployment**: Supports one-click Zeabur, Replit deployment with true zero cost and idiot-proof deployment process. It also supports kuma anti-sleep, as well as Docker and fly.io deployment.

## Environment variables

| Variable Name           | Comment                                                      | required? |
| ---------------------- | ------------------------------------------------------------ | ---------------------- |
| **BOT_TOKEN** | Telegram bot token. Create a bot on [BotFather](https://t.me/BotFather) to get the BOT_TOKEN. | **Yes** |
| **API**       | OpenAI or third-party API key.                              | **Yes** |
| GPT_ENGINE    | Set the default QA model; the default is:`gpt-4-1106-preview`. This item can be freely switched using the bot's "info" command, and it doesn't need to be set in principle. | No |
| WEB_HOOK  | Whenever the telegram bot receives a user message, the message will be passed to WEB_HOOK, where the bot will listen to it and process the received messages in a timely manner. | No |
| API_URL       | If you are using the OpenAI official API, you don't need to set this. If you using a third-party API, you need to fill in the third-party proxy website. The default is: https://api.openai.com/v1/chat/completions | No |
| NICK          | The default is empty, and NICK is the name of the bot. The bot will only respond when the message starts with NICK that the user inputs, otherwise the bot will respond to any message. Especially in group chats, if there is no NICK, the bot will reply to all messages. | No |
| PASS_HISTORY  | The default is true. The bot remembers the conversation history and considers the context when replying next time. If set to false, the bot will forget the conversation history and only consider the current conversation. | No |
| GOOGLE_API_KEY | If you need to use Google search, you need to set it. If you do not set this environment variable, the bot will default to provide duckduckgo search. Create credentials in Google Cloud's [APIs & Services](https://console.cloud.google.com/apis/api/customsearch.googleapis.com) and the API Key will be GOOGLE_API_KEY on the credentials page. Google search can be queried 100 times a day, which is completely sufficient for light use. When the usage limit has been reached, the bot will automatically turn off Google search. | No |
| GOOGLE_CSE_ID | If you need to use Google search, you need to set it together with GOOGLE_API_KEY. Create a search engine in [Programmable Search Engine](https://programmablesearchengine.google.com/), where the search engine ID is the value of GOOGLE_CSE_ID. | No |
| whitelist     | Set which users can access the bot, and connect the user IDs authorized to use the bot with ','. The default value is `None`, which means that the bot is open to everyone. | No |

## Zeabur Remote Deployment (Recommended)

One-click deployment:

[![Deploy on Zeabur](https://zeabur.com/button.svg)](https://zeabur.com/templates/R5JY5O?referralCode=yym68686)

If you need follow-up function updates, the following deployment method is recommended:

Fork this repository first, then register for [Zeabur](https://zeabur.com). The free quota is sufficient for light use. Import from your own Github repository, set the required environment variables, and redeploy. If you need function updates in the follow-up, just synchronize this repository in your own repository and redeploy in Zeabur to get the latest functions.

## Replit Remote Deployment

[![Run on Repl.it](https://replit.com/badge/github/yym68686/ChatGPT-Telegram-Bot)](https://replit.com/new/github/yym68686/ChatGPT-Telegram-Bot)

After importing the Github repository, set the running command

```bash
pip install -r requirements.txt > /dev/null && python3 main.py
```

Select Secrets in the Tools sidebar, add the environment variables required by the bot, where:

- WEB_HOOK: Replit will automatically assign a domain name to you, fill in `https://appname.username.repl.co`
- Remember to open "Always On"

Click the run button on the top of the screen to run the bot.

## fly.io Remote Deployment

Official documentation: https://fly.io/docs/

Use Docker image to deploy fly.io application

```bash
flyctl launch --image yym68686/chatgpt:1.0
```

Enter the name of the application when prompted, and select No for initializing Postgresql or Redis.

Follow the prompts to deploy. A secondary domain name will be provided in the official control panel, which can be used to access the service.

Set environment variables

```bash
flyctl secrets set BOT_TOKEN=bottoken
flyctl secrets set API=
# optional
flyctl secrets set WEB_HOOK=https://flyio-app-name.fly.dev/
flyctl secrets set NICK=javis
```

View all environment variables

```bash
flyctl secrets list
```

Remove environment variables

```bash
flyctl secrets unset MY_SECRET DATABASE_URL
```

ssh to fly.io container

```bash
flyctl ssh issue --agent
# ssh connection
flyctl ssh establish
```

Check whether the webhook URL is correct

```bash
https://api.telegram.org/bot<token>/getWebhookInfo
```

## Docker Local Deployment

Start the container

```bash
docker run -p 80:8080 --name chatbot -dit \
    -e BOT_TOKEN="telegram bot token" \
    -e API="" \
    -e API_URL= \
    yym68686/chatgpt:1.0
```

Or if you want to use Docker Compose, here is a docker-compose.yml example:

```yaml
version: "3.5"
services:
  chatgptbot:
    container_name: chatgptbot
    image: yym68686/chatgpt:1.0
    environment:
      - BOT_TOKEN=
      - API=
      - API_URL=
    ports:
      - 80:8080
```

Run Docker Compose container in the background

```bash
docker-compose up -d
```

Package the Docker image in the repository and upload it to Docker Hub

```bash
docker build --no-cache -t chatgpt:1.0 -f Dockerfile.build --platform linux/amd64 .
docker tag chatgpt:1.0 yym68686/chatgpt:1.0
docker push yym68686/chatgpt:1.0
```

One-Click Restart Docker Image

```bash
set -eu
docker rm -f chatbot
docker run -p 8080:8080 -dit --name chatbot \
-e BOT_TOKEN= \
-e API= \
-e API_URL= \
-e GOOGLE_API_KEY= \
-e GOOGLE_CSE_ID= \
-e claude_api_key= \
yym68686/chatgpt:1.0
docker logs -f chatbot
```

This script is for restarting the Docker image with a single command. It first removes the existing Docker container named "chatbot" if it exists. Then, it runs a new Docker container with the name "chatbot", exposing port 8080 and setting various environment variables. The Docker image used is "yym68686/chatgpt:1.0". Finally, it follows the logs of the "chatbot" container.

## Q & A

- Why can't I use Google search?

By default, DuckDuckGo search is provided. The official API for Google search needs to be applied for by the user. It can provide real-time information that GPT could not answer before, such as today's trending topics on Weibo, today's weather in a specific location, and the progress of a certain person or news event.

- How do I switch models?

You can switch between GPT3.5, GPT4, and other models using the "info" command in the chat window.

- Does it support a vector database?

Yes, it supports document Q&A based on the embedded vector database. During a search, for the searched PDF, automatic vector semantic search of PDF documents is performed, and pdf-related content is extracted based on the vector database. It also supports using the "qa" command to vectorize an entire website with the "sitemap.xml" file, and answer questions based on the vector database. This is especially suitable for document websites and wiki websites of some projects.

- Can it be deployed in a group?

Yes, it supports whitelisting to prevent abuse and information leakage.

- How should I set the API_URL?

It supports all suffixes, including: https://api.openai.com/v1/chat/completions, https://api.openai.com/v1, and https://api.openai.com/.

- Is it necessary to configure the web_hook environment variable?

The web_hook is not a mandatory environment variable. You only need to set the domain name (which must be consistent with WEB_HOOK) and other environment variables as required for your application's functionality.

## References

https://core.telegram.org/bots/api

https://github.com/acheong08/ChatGPT

https://github.com/franalgaba/chatgpt-telegram-bot-serverless

https://github.com/gpchelkin/scdlbot/blob/d64d14f6c6d357ba818e80b8a0a9291c2146d6fe/scdlbot/__main__.py#L8

The markdown rendering of the message used is another [project](https://github.com/yym68686/md2tgmd) of mine.

## Star History

<a href="https://github.com/yym68686/ChatGPT-Telegram-Bot/stargazers">
        <img width="500" alt="Star History Chart" src="https://api.star-history.com/svg?repos=yym68686/ChatGPT-Telegram-Bot&type=Date">
</a>

## Sponsor

[![Deployed on Zeabur](https://zeabur.com/deployed-on-zeabur-dark.svg)](https://zeabur.com?referralCode=yym68686&utm_source=yym68686&utm_campaign=oss)

## License

This project is licensed under GPLv3, which means you are free to copy, distribute, and modify the software, as long as all modifications and derivative works are also released under the same license.
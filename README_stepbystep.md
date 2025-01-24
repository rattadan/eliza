# Eliza 🤖 for Communities

<div align="center">
  <img src="./docs/static/img/eliza_banner.jpg" alt="Eliza Banner" width="60%" />
</div>

<div align="center">
  📖 [Documentation](https://elizaos.github.io/eliza/) | 🎯 [Examples](https://github.com/thejoven/awesome-eliza)
</div>

## 🚩 Overview

<div align="center">
  <img src="./docs/static/img/eliza_diagram.png" alt="Eliza Diagram" width="100%" />
</div>

## ✨ Features

- 🛠️ Full-featured connectors for Discord, Twitter, and Telegram
- 🔗 Supports various models (Llama, Grok, OpenAI, Anthropic, etc.)
- 👥 Multi-agent and room support
- 📚 Easy document ingestion and interaction
- 💾 Retrievable memory and document store
- 🚀 High extensibility for custom actions and clients
- ☁️ Compatible with many models (local Llama, OpenAI, Anthropic, Groq, etc.)
- 📦 Ready to use!

## Important Notice

This guide is designed to help communities run their own bots effortlessly.

### Prerequisites

- [Python 2.7+](https://www.python.org/downloads/)
- [Node.js 23+](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
- Use `nvm` to set Node.js version: `nvm use v23.3.0` or `nvm alias default v23.3.0`
- [pnpm](https://pnpm.io/installation)

### Creating the File Structure

```bash
git clone https://github.com/elizaos/eliza.git
```


### First Build Initialization

1. Install dependencies and build:

   ```bash
   pnpm i
   pnpm build
      ```



### Edit the `.env` File

The `.env` file contains all API keys, seed phrases, and credentials for Twitter, Discord, and Telegram. Do not share this file.
Best to use is VS Code, but you can also use any text-based editor or even the commandline editor

1. Copy the example file:

   ```bash
   cp .env.example .env
   ```

2. Edit the file using `nano`:

   ```bash
   nano .env
   ```
If you work on a remote server, you might use the built in code editor to populate your environment file
   - **Nano Commands:**
     - `CTRL+X`: Save and Exit
     - `CTRL+W`: Search

### Obtain  API Key

You can use different supported API's 

According to your favoured API provider, set the "modelProvider" in your character file to one of the following options:
```
"openai"
"anthropic"
"akash_chat_api"
"eternalai"
"groq"
"ALI_BAILIAN"
"VOLENGINE"
"LLAMACLOUD"
"TOGETHER"
"OLLAMA"
"GOOGLE"
"LLAMALOCAL"
```

1. Edit the `.env` file to add your API key:

   ```bash
   nano .env
   ```
Insert your API KEY into the according line
  

### Edit Your Character File

1. Navigate to the character folder:

   ```bash
   cd characters
   ```

2. Use `c3po.character.json` as a template:

   ```bash
   cp c3po.character.json avatar.character.json
   nano avatar.character.json
   ```

   - **Structure:**
     - **Bio:** Self-definition for the LLM
     - **Lore:** Text style examples
     - **Knowledge:** Initial memories (if you want your bot to know facts about your project, add it here.. e.g., website, Discord URL)
    
    You can start with a small character file first, you can append more lines later easily to grow your agents personality

3. Set the attribute "modelProvider" to use one of the following API Endpoints you have added to your .env file

```json
"openai"
"anthropic"
"akash_chat_api"
"eternalai"
"groq"
"ALI_BAILIAN"
"VOLENGINE"
"LLAMACLOUD"
"TOGETHER"
"OLLAMA"
"GOOGLE"
"LLAMALOCAL"  
```

2. If you want to run your framework on a remote server, you might want to keep the instance running after logout.
   So use `pm2`:

   ```bash
   npm install pm2
   ```

4. Start the process with `pm2`:

   ```bash
   pm2 start pnpm --name eliza -- start -- --character=./eliza/characters/avatar.character.json
   ```

   - **PM2 Commands:**
     ```
     - `pm2 start eliza`
     - `pm2 stop eliza`
     - `pm2 log`
     - `pm2 status`
     - `pm2 save`
     ```




### First Chat
Start the framework

```bash
pnpm start --characters="./eliza/characters/avatar.character.json"
```

And open a new window and run:

```bash
pnpm start:client
```

Visit the displayed website on your localhost using a web browser.
This is the main interaction interface that supports chat, file upload (images, PDF) and the easiest way for troubleshooting your characters behaviour

To handle multiple character the same times, a good option is to add the following credential directly into the character file, not the .env file:
replace with your own tokens:
 ```json
   "settings": {
        "secrets": {
                "TELEGRAM_BOT_TOKEN": "773XXXXXXXXXXXXXXXXXXXXXXXTj7M",
                "DISCORD_APPLICATION_ID": "133XXXXXXXXXXXXXX936",
                "DISCORD_API_TOKEN": "MTMXXXXXXXXXXXXXXXXXXXXXXXXXXXXXNjQ"
         },
 ```

### Telegram Connection

1. Use [BotFather](https://telegram.me/BotFather) to create a bot.
2. 
3. Either Add your bot token to the `.env` file OR
4. Add your bot token into the characters header file:

5. Update your character file header:

   ```json
   "clients": ["telegram"],
   ```

   Ensure a space after the colon: `"clients": ["telegram"],`.

### Discord Connection

1. Create a bot token at [Discord Developers](https://discord.com/developers).
2. Reset your token to show the Discord API Token (it contains not just numbers, it contains alphanumericals too.
3. Use the invite link from startup to add the bot to your server.
4. Set bot permission intentions to the right.
5. Enter your Application ID and API Token as shown.

      ```json
   "clients": ["telegram","discord"],
   ```

## Advanced Character Settings

Actually you can use the character to file to adjust your agents behaviour in responding as you wish
Feel free to play with different characters, to get ready for the next lesson, the Tag Team feature!
Example:
Take this as an easy example:

```json
{
    "name": "Bud",
    "plugins": [],
    "clients": ["discord","telegram","twitter"],
    "modelProvider": "akash_chat_api",
    "settings": {
        "secrets": {
            "TELEGRAM_BOT_TOKEN": "799XXXXXXXXXXXXXXXXXXXXXXXXXXXHGQk",
            "DISCORD_APPLICATION_ID": "133XXXXXXXXXXXXXXXXXXX24",
            "DISCORD_API_TOKEN": "MTMzXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXAfZI"
        },
        "voice": {
            "model": "en_US-male-heavy"
        }
    },
       "clientConfig": {
        "shouldRespondOnlyToMentions": false,
        "discord": {
            "shouldRespondOnlyToMentions": false,
            "shouldIgnoreBotMessages": false,
            "shouldIgnoreDirectMessages": false,
            "responseChance": 1
        },
        "telegram": {
            "shouldRespondOnlyToMentions": false,
            "shouldIgnoreBotMessages": false,
            "shouldIgnoreDirectMessages": false,
            "responseChance": 0.8
        }
    },
```

- **Secrets:** Override values in `.env` for a specific character.
- **Plugins:** Embed plugins from the `/packages` folder.

### Accessing the Database

```bash
cd /agent/data
```

Find the `db.sqlite` file and use `sqlitebrowser` to access it:

```bash
sqlitebrowser db.sqlite
```


## Contributors

<a href="https://github.com/elizaos/eliza/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=elizaos/eliza" />
</a>

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=elizaos/eliza&type=Date)](https://star-history.com/#elizaos/eliza&Date)

# Developing Apps with Zencoder
Some scaffolding and prerequisites for the talk on developing apps using Zencoder.

## Prerequisites
* Install VSCode or JetBrains IDE
* Install [Zencoder](https://zencoder.ai)
* Install docker
* Install [uvx](https://docs.astral.sh/uv/concepts/tools/) and [npx](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

Some of the MCP servers require extra data like paths or API tokens. Those you need to update in relevant section of the mcp config

## MCP Servers
### Time
```json
        "time": {
            "command": "npx",
            "args": ["-y", "time-mcp"]
        }
 ```

### Git
```json
        "git": {
            "command": "uvx",
            "args": ["mcp-server-git", "--repository", "/ABSOLUTE/PATH/TO/GIT/REPO"]
        }
```

### Docker
```json
        "mcp-server-docker": {
            "command": "uvx",
            "args": [
                "mcp-server-docker"
            ]
        }
```

### Grafana
Download and unpack from https://github.com/grafana/mcp-grafana/releases, provide absolute path to mcp-grafana binary. Update GRAFANA_URL and GRAFANA_API_KEY as needed.
```json
        "grafana": {
            "command": "/PATH/TO/mcp-grafana",
            "args": [],
            "env": {
                "GRAFANA_URL": "http://localhost:3001",
                "GRAFANA_API_KEY": "SERVICE_ACCOUNT_TOKEN"
            }
        }
```

### Slack
`.env.slack` file contains bot token and team id. Create a new app at https://api.slack.com/apps and add it to your workspace. Then create a bot user and get its token. Also find team ID by going to `Manage Workspace -> About -> Team ID`.
More instructions here: https://github.com/modelcontextprotocol/servers/tree/main/src/slack#setup
```
SLACK_BOT_TOKEN=YOUR_TOKEN
SLACK_TEAM_ID=YOUR_TEAM_ID
```
MCP config:
```json
        "slack": {
            "command": "docker",
            "args": [
                "run",
                "-i",
                "--rm",
                "--env-file",
                "/PATH/TO/.env.slack",
                "mcp/slack"
            ]
        }
```

### Github
Create Github PAT https://github.com/github/github-mcp-server?tab=readme-ov-file#prerequisites and put it into `.env.github` file
```
GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_GITHUB_PAT
```

MCP config:
```json
        "github": {
            "command": "docker",
            "args": [
                "run",
                "-i",
                "--rm",
                "--env-file",
                "/PATH/TO/.env.github",
                "ghcr.io/github/github-mcp-server"
            ]
        }
```


## Prompts/steps
Possible intermediate steps are ommited due to undeterministic nature of LLM responses.
### Python + Javascript
0.
Create repository on github, clone locally, open in IDE
MCPs - git, slack
```
Checkout branch app
```
1. 
MCPs - docker, time
```
I want to build a website for a conference, it should have several pages, namely: 1. Intro page about conference, 2. Page for people to submit their talks, 3. Page with submitted talks. Frontend part needs to be written in react, backend - in fastapi. I want to store the submissions in postgresql database. Create dockerfile for frontend and for backend, and docker compose file. 
```
2.
```
Add filters to the page with talks
```
3.
```
Add monitoring for the backend api using grafana and prometheus
```
4. 
MCPs - grafana
```
How many requests does the app have in grafana?
```
5.
```
Let's now prepare deployment yaml that will allow us to deploy this app to kubernetes
```
6. 
MCPs - github
```
Commit and push changes, create a pull request
```
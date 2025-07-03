# Developing Apps with Zencoder
Some scaffolding and prerequisites for the talk on developing apps using Zencoder.

## Prerequisites
* Install VSCode or JetBrains IDE
* Install [Zencoder](https://zencoder.ai)
* Install [docker](https://docs.docker.com/engine/install/)
* Install [uvx](https://docs.astral.sh/uv/getting-started/installation/) and [npx](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

Some of the MCP servers require extra data like paths or API tokens. Those you need to update in relevant section of the mcp config

## MCP Servers
### Time
```json
{
    "command": "npx",
    "args": ["-y", "time-mcp"]
}
 ```

### Git
```json
{
    "command": "uvx",
    "args": ["mcp-server-git", "--repository", "/ABSOLUTE/PATH/TO/GIT/REPO"]
}
```

### Docker (optional)
```json
{
    "command": "uvx",
    "args": [
        "mcp-server-docker"
    ]
}
```

### Grafana
Download and unpack from https://github.com/grafana/mcp-grafana/releases, provide absolute path to mcp-grafana binary. Update GRAFANA_URL and GRAFANA_API_KEY as needed.
```json
{
    "command": "/PATH/TO/mcp-grafana",
    "args": [],
    "env": {
        "GRAFANA_URL": "http://localhost:3001",
        "GRAFANA_API_KEY": "SERVICE_ACCOUNT_TOKEN"
    }
}
```

### Postgres
```json
{
    "command": "npx",
    "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://USERNAME:PASSWORD@localhost:5432/DB_NAME"
    ]
}
```

### Slack
`.env.slack` file contains bot token and team id. Create a new app at https://api.slack.com/apps and add it to your workspace. Then create a bot user and get its token. Also find team ID by going to `Manage Workspace -> About -> Team ID`.
More instructions here: https://github.com/modelcontextprotocol/servers/tree/main/src/slack#setup
```
SLACK_BOT_TOKEN=YOUR_TOKEN
SLACK_TEAM_ID=YOUR_TEAM_ID
```
Local MCP config:
```json
{
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
Remote MCP config:
```json
{
    "type": "http",
    "url": "https://slack-community.mcps.zencoder.ai/mcp",
    "headers": {
        "Authorization": "Bearer TOKEN"
    }
}
```

### Github
Create Github PAT https://github.com/github/github-mcp-server?tab=readme-ov-file#prerequisites and put it into `.env.github` file
```
GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_GITHUB_PAT
```

MCP config:
```json
{
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
MCPs - docker (optional), time
```
I want to build a website for a conference, it should have several pages, namely: 1. Intro page about conference, 2. Page for people to submit their talks, 3. Page with submitted talks. Frontend part needs to be written in react, backend - in fastapi. I want to store the submissions in postgresql database. Create dockerfile for frontend and for backend, and docker compose file. Use tailwind for styling
```
2. 
```
/e2e-test go to http://localhost:3000 and check that all buttons in the header work and redirect to the correct pages
```
3.
```
Add filters to the page with talks
```
4.
```
Add monitoring for the backend api using grafana and prometheus along with some basic dashboards
```
5. 
MCPs - grafana and/or postgresql
```
How many requests does the app have in grafana?
```
```
Get all talks from postgres where speaker is {NAME}
```
6.
```
Let's now prepare deployment yaml that will allow us to deploy this app to kubernetes
```
7. 
MCPs - github
```
Commit and push changes, create a pull request
```
8. 
Custom agents
* Version updater
```
Find all the files listing dependencies, including docker files. For each dependency check online the very latest version even if it includes major version update, I want the most up to date version for each package, library and image. Output the list of planned updates before actually updating and ask for user's approval. Also make sure to have backups for any relevant resources, like databases. Once approved, update as planned, restore data from backups, and then verify everything works by making sure docker images build and run and existing tests are passing
```
* Commiter
```
You are tasked with creating nice commits. Before commiting check if there are any untracked files and check with the user if any of them should be tracked. Once you have the final list of files to be commited, summarize changes and suggest nice commit message which summarizes the changes made. Check for user approval for the commit message and if approved, make a commit
```

### Javascript
0.
Create repository on github, clone locally, open in IDE
MCPs - git, slack
```
Checkout branch app
```
1. 
MCPs - docker (optional), time
```
I want to build a website for a conference, it should have several pages, namely: 1. Intro page about conference, 2. Page for people to submit their talks, 3. Page with submitted talks. Frontend part needs to be written in react, backend - in nodejs. I want to store the submissions in postgresql database. Create dockerfile for frontend and for backend, and docker compose file. Use tailwind for styling
```
2. 
```
/e2e-test go to http://localhost:3000 and check that all buttons in the header work and redirect to the correct pages
```
3.
```
Add filters to the page with talks
```
4.
```
Add monitoring for the backend api using grafana and prometheus along with some basic dashboards
```
5. 
MCPs - grafana and/or postgres
```
How many requests does the app have in grafana?
```
```
Get all talks from postgres where speaker is {NAME}
```
6.
```
Let's now prepare deployment yaml that will allow us to deploy this app to kubernetes
```
7. 
MCPs - github
```
Commit and push changes, create a pull request
```
8. 
Custom agents
* Version updater
```
Find all the files listing dependencies, including docker files. For each dependency check online the very latest version even if it includes major version update, I want the most up to date version for each package, library and image. Output the list of planned updates before actually updating and ask for user's approval. Also make sure to have backups for any relevant resources, like databases. Once approved, update as planned, restore data from backups, and then verify everything works by making sure docker images build and run and existing tests are passing
```
* Commiter
```
You are tasked with creating nice commits. Before commiting check if there are any untracked files and check with the user if any of them should be tracked. Once you have the final list of files to be commited, summarize changes and suggest nice commit message which summarizes the changes made. Check for user approval for the commit message and if approved, make a commit
```

# Developing Apps with Zencoder
Some scaffolding and prerequisites for the talk on developing apps using Zencoder.

## Prerequisites
* Install VSCode or JetBrains IDE
* Install [Zencoder](https://zencoder.ai)
* Install [docker](https://docs.docker.com/engine/install/)
* Install [uvx](https://docs.astral.sh/uv/getting-started/installation/) and [npx](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

Some of the MCP servers require extra data like paths or API tokens. Those you need to update in relevant section of the mcp config

## MCP Servers
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
1. 
Agent - `Code`  
Model (optional) - `Sonnet 4 Parallel Thinking`
```
I want to build a website for the conference. Frontend part needs to be written in react, backend - in fastapi. I want to store the submissions in postgresql database. Create dockerfile for frontend and for backend, and docker compose file. Use tailwind for styling. Use supabase for auth.
Add the following pages:
Intro/Home Page: Main landing page for the conference
Submitted Talks Page: Shows all submitted talks with vote counts, visible to everyone but only authenticated users can vote
Login Page: Authentication for users
User Profile Page: List of user's submitted talks with statuses; personal information and settings
Talk Submission Page: Form for submitting new talk proposals, only available for authenticated users, fill in available user data automatically
Approved Talk Directory: List of all approved talks with link to speaker page with bio
FAQ/Help Page: Common questions and answers; support information
Contact Page: General inquiries, support requests, or media contacts
Each page should follow the same aesthetic and style without exception.
All DB queries should be done through backend
```
2. 
Agent - `E2E test`  
```
Go to http://localhost:3000, check that clicking on talk submission button works. Try filling the form with some dummy data and submit talk. Then verify that the talk is shown on the submitted talks page
```
3.
Agent - `Code`  
```
Add filters to the page with talks
```
4.
Agent - `Code`  
```
Add monitoring for the backend api using grafana and prometheus along with some basic dashboards
```
5. 
Agent - `Code`  
MCPs - grafana and/or postgres
```
How many requests does the app have in grafana?
```
```
Get all talks from postgres where speaker is {NAME}
```
6.
Agent - `Code`  
```
Let's now prepare deployment yaml that will allow us to deploy this app to kubernetes
```
7. 
Agent - `Code`  
MCPs - github
```
Commit and push changes, create a pull request
```
8. 
Custom agents:
* Version updater
```
Find all the files listing dependencies, including docker files. For each dependency check online the very latest version even if it includes major version update, I want the most up to date version for each package, library and image. Output the list of planned updates before actually updating and ask for user's approval. Also make sure to have backups for any relevant resources, like databases. Once approved, update as planned, restore data from backups, and then verify everything works by making sure docker images build and run and existing tests are passing
```
* Commiter
```
You are tasked with creating nice commits. Before commiting check if there are any untracked files and check with the user if any of them should be tracked. Once you have the final list of files to be commited, summarize changes and suggest nice commit message which summarizes the changes made. Check for user approval for the commit message and if approved, make a commit
```
9. 
```
Use https://github.com/modelcontextprotocol/servers/tree/main/src/everything as a reference and create similar mcp server with stdio and streamable http transports for the conference. It should have three tools:
1. List all submitter talks
2. List talks subject to filters
3. Search talk by title
```

### Javascript
0. 
Create repository on github, clone locally, open in IDE
1. 
Agent - `Code`  
Model (optional) - `Sonnet 4 Parallel Thinking`
```
I want to build a website for the conference. Let's use NextJS for frontend/backend, and supabase for auth. Use tailwind for styling.
Add the following pages:
Intro/Home Page: Main landing page for the conference
Submitted Talks Page: Shows all submitted talks with vote counts, visible to everyone but only authenticated users can vote
Login Page: Authentication for users
User Profile Page: List of user's submitted talks with statuses; personal information and settings
Talk Submission Page: Form for submitting new talk proposals, only available for authenticated users, fill in available user data automatically
Approved Talk Directory: List of all approved talks with link to speaker page with bio
FAQ/Help Page: Common questions and answers; support information
Contact Page: General inquiries, support requests, or media contacts
Each page should follow the same aesthetic and style without exception.
All DB queries should be done through backend
```
2. 
Agent - `E2E test`  
```
Go to http://localhost:3000, check that clicking on talk submission button works. Try filling the form with some dummy data and submit talk. Then verify that the talk is shown on the submitted talks page
```
3.
Agent - `Code`  
```
Add filters to the page with talks
```
4.
Agent - `Code`  
```
Add monitoring for the backend api using grafana and prometheus along with some basic dashboards
```
5. 
Agent - `Code`  
MCPs - grafana and/or postgres
```
How many requests does the app have in grafana?
```
```
Get all talks from postgres where speaker is {NAME}
```
6.
Agent - `Code`  
```
Let's now prepare deployment yaml that will allow us to deploy this app to kubernetes
```
7. 
Agent - `Code`  
MCPs - github
```
Commit and push changes, create a pull request
```
8. 
Custom agents:
* Version updater
```
Find all the files listing dependencies, including docker files. For each dependency check online the very latest version even if it includes major version update, I want the most up to date version for each package, library and image. Output the list of planned updates before actually updating and ask for user's approval. Also make sure to have backups for any relevant resources, like databases. Once approved, update as planned, restore data from backups, and then verify everything works by making sure docker images build and run and existing tests are passing
```
* Commiter
```
You are tasked with creating nice commits. Before commiting check if there are any untracked files and check with the user if any of them should be tracked. Once you have the final list of files to be commited, summarize changes and suggest nice commit message which summarizes the changes made. Check for user approval for the commit message and if approved, make a commit
```
9. 
```
Use https://github.com/modelcontextprotocol/servers/tree/main/src/everything as a reference and create similar mcp server with stdio and streamable http transports for the conference. It should have three tools:
1. List all submitter talks
2. List talks subject to filters
3. Search talk by title
```





















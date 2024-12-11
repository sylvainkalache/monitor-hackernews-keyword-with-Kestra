# Track keywords on HackerNews

Receive an email when a keyword appears in a HackerNews conversation.
I built this using Kestra.io, an OSS orchestration platform.

I was part of the team that built the first keyword tracker for HackerNews, [HNwatcher](https://news.ycombinator.com/item?id=6097409), back in 2013. It was fun to rebuild a big part of this project in a much easier way :-)

## How to use it

First, you need to customize the `env` file with base64 values of the following credentials.
```bash
SECRET_CLOUDQUERY_API_KEY=
SECRET_FROM=
SECRET_TO=
SECRET_USERNAME=
SECRET_APP_PASSWORD=
```
You can get a key from CloudQuery [here](https://docs.cloudquery.io/docs/deployment/generate-api-key).

I used my Gmail account to send emails. Don't use your password, but an app password, [here](https://myaccount.google.com/apppasswords).

## Usage

There are many ways to [run Kestra](https://kestra.io/docs/installation); here is the easiest way with Docker:

`docker run --pull=always --rm -it -p 8080:8080 --env-file env --user=root -v /var/run/docker.sock:/var/run/docker.sock -v /tmp:/tmp kestra/kestra:latest server local`

Go to `http://localhost:8080/ui/flows`

Create a new workflow and paste the content of the file `kestra_flow.yaml`

Make sure to change the input section with the keyword of your choice. 
In the example below, I am tracking the word "AI."
```
inputs:
  - id: keyword
    type: STRING
    defaults: "AI"
    displayName: "Keyword to look for in HackerNews conversations"
```

# Recurring scheduling
Kestra offers to schedule flows. In the Editor, section, click on Actions > Add trigger.

![actions](https://raw.githubusercontent.com/sylvainkalache/monitor-hackernews-keyword-with-Kestra/refs/heads/main/keras-screenshots/actions.png)

Then look for the `io.kestra.plugin.core.trigger.Schedule` trigger
![trigger](https://raw.githubusercontent.com/sylvainkalache/monitor-hackernews-keyword-with-Kestra/refs/heads/main/keras-screenshots/schedule.png)

Configure your trigger like a regular cronjob and save. You can verify that it is well configured by navigating to Administration > Triggers
![administration](https://raw.githubusercontent.com/sylvainkalache/monitor-hackernews-keyword-with-Kestra/refs/heads/main/keras-screenshots/triggers.png)

## Known issue
Currently, the flow will send an email even if no keyword is spotted. Feel free to submit a PR if you know how to fix it, or I may fix it at some point ;-).

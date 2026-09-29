---
layout: post
title: Notes on working with coding agents
categories: 
    - code
last_modified_at: 2026-09-29T19:58:00+01:00
excerpt_separator: "<!--more-->"
---

During my last holiday I spent a good chunk of its 2 weeks working on a new [side](https://github.com/adri0/scadustats/) [project](https://scadustats.com/) (more about it in a future post!). A lot has changed with coding agents recently, and it has been a while since I've had to push their limits on a relatively complex problem outside my day job. With better models and harnesses, coding agents now seem a lot more competent. This means that now I can unleash Claude(s) to aggressively work on many ideas in parallel, while I spend more of my time designing the product, running experiments and managing the process - which required me to tweak my solo dev workflow.

First requirement: I'm used to keeping a backlog using GitHub issues, even when working alone. I'd like Claude to act like a teammate: collaborating, refining issues and sourcing coding tasks from there. This way the project's context isn't trapped only in `CLAUDE.md` and READMEs. 

Second requirement: It would be nice to have multiple Claude sessions running in parallel, each solving an issue.

Third requirement: I wanted to keep up with understanding the codebase as it evolves, but also learn new patterns and Python tricks suggested by the agents. In addition, I wanted to be able to vet occasional mistakes and bad choices early on, before they get the chance to snowball (and there were a few). I'm open-minded about Claude's approach but reserve the right to shoot down anything that doesn't smell right.

Fourth requirement: I wanted to protect myself from unintended harmful agent actions. I don't like the idea of my private data ending up in Anthropic's servers, or worse, sent to some unknown third-party due to malicious injected prompts or any agentic creativity burst.

In the end I settled on a workflow that I'm relatively happy with. It boils down to the following:

### 1. Creating GitHub issues describing individual tasks

Issues should contain detailed enough information so that they can be picked up in an individual Claude session and result in a PR. I treat issues almost as a prompt, but it also works as documentation about past product decisions. 

![Issues are like prompts](/assets/images/issues.png)

### 2. Creating multiple Claude sessions for tackling problems in parallel

Every session almost always starts with the same prompt: "*Tackle issue #\<issue-number\> and open a PR.*"

I keep up to 5 sessions running at the same time, each on its own terminal. Technically, I could create as many as needed, but it can get hard to keep track of what's going on. Each session runs in a separate [git worktree](https://git-scm.com/docs/git-worktree) and tackles 1 issue at a time. 

I envision that the generalisation of this would be having a dedicated agent coordinating the multiple coding sessions, theoretically being able to tackle all open issues at the same time (and exhausting my tokens fast). But at the moment it feels overkill.

![Many terminals](/assets/images/terminals.png)

### 3. Reviewing open PRs on GitHub

I review each open PR individually, just like I'd do if working in a team. Whenever I spot something to be changed or would like clarification I simply leave a comment. Then, I go back to the session that generated the PR and ask Claude to address the open comments. Since Claude uses GitHub CLI `gh` logged in with my account, its replies show as coming from my own user, looking like I'm talking to myself.

![PR comments](/assets/images/pr_comments.png)

### 4. Taking precautions against agentic shenanigans

I don't feel very comfortable leaving coding agents on [auto mode](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode) running shell commands while having access to all my personal files. Thus, instead of disabling auto mode and reviewing every command manually, I opted for limiting Claude's access perimeter:

***Run on its own user*** - I created a fresh non-admin `dev` user on my local machine and only allow Claude sessions to run from this user. This means that Claude doesn't have access to any file stored in my own user, nor will it have admin access to change anything outside this user space. 

I still do the work from my main user. I only impersonate this user in a terminal for starting Claude sessions using [`sudo su - dev`](https://man7.org/linux/man-pages/man1/su.1.html).

```bash
myuser ~> sudo su - dev
dev ~> cd code/repo   # Here it is logged in as the dev user
dev ~> claude
```

***Share a code directory*** - In addition, the code repos are stored in a shared directory between my main user and the `dev` user, for the cases where I'd like to inspect the code state or make ad-hoc changes. 

A drawback from this setup is that I lose Claude integration with my IDE (open file awareness, selection awareness, etc.) because now Claude and the IDE are running under different users (still looking for an alternative). But I can live with that. I don't feel those features are super essential anyway.

***Use a fine-grained GitHub token*** - The last precaution is related to git and GitHub access. I don't want Claude to have access to my entire GitHub account (especially private repos). Thus, for Claude sessions, I log in to GitHub CLI using a [fine-grained GitHub personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#fine-grained-personal-access-tokens), which is configured to allow access only to the relevant repos, and only can write issues, pull requests and comments in pull requests of only those particular repositories. 

### Closing remarks

This way of working is by no means perfect and I'm sure it will change over time. However, it feels very productive, safe and reasonable. All the paper trail of prompts as issues and pull requests with comments natively tracked on GitHub, mimicking teamwork, feels quite nice. Claude being fairly isolated in its own user, with contained tool access, gives me extra peace of mind.
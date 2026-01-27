+++
title = "Taming Claude Code"
date = "2026-01-27"

[taxonomies]
tags=["llm", "claude-code"]
+++

_I was recently giving masterclasses for colleagues and friends about Claude Code and realized it makes sense to share some tips I show there._

{% note(clickable=false, hidden=false, header="WARNING") %}
⚠️ Target audience: Senior Developers. Coding agent is not a suitable tool for beginners.
{% end %}

"Computers are good at following instructions, but not at reading your mind." © Donald Knuth

Many different speakers would tell you that you have to become a manager to work with coding agents. It frightens a lot of programmers. The good news is: That is a lie! Such an approach might work if all you need is a crude prototype, but it would probably fail dramatically if you persist. A much better strategy is to take the role of a **systems analyst** or **software architect**. Coding agents do not have expertise to choose a good solution. They choose the most common solution for the context they have. And our goal is to:

1. Provide the agent with more relevant context
2. Guide the agent toward a better solution

Doing this interactively for each of the tasks is extremely tiresome, so it is a good idea to reuse as much as possible. And Claude Code has some very good tools for such reuse.

## Skills

Think about every practice you try to enforce on your projects. You should probably make a [Skill](https://code.claude.com/docs/en/skills) for it. A Skill is a very efficient approach for transferring knowledge into your agentic alter-ego. It is a markdown document accompanied by additional files which can include both additional documentation and pre-built tools for specific tasks. The main markdown file has a short description text which will always be present in the context. The agent is instructed to check its current objective with this description and it will deep-dive if there is a match. You can encode your VCS/branching preferences in one skill (_my local skill encodes my personal rules for using [jj](/tags/jujutsu)_) and an approach to writing code-comments in another skill. My toolset has skills for the way I want python-tools to be created (and Claude writes a lot of temporary python tools!) and how reactivity should be done in [Dioxus](https://dioxuslabs.com/) apps. I have several dozen of them and add more constantly.

And while some skills are shareable, the majority of them depends on your personal tastes and preferences and just wouldn't make a lot of sense for anyone else. And that is a good thing. Don't rush to look for curated lists and marketplaces of skills. Create your own.

## Claude Code can configure itself

And when I say "create your own", I don't mean to literally write them by hand. Jump to the meta-level: tell Claude Code to do it for you.

> Take a look at the recent Claude Code docs for writing skills and create a skill for …. Ask me questions about how I want it to work.

"Ask me questions" is a very important part. You don't want Claude to make decisions on your behalf. You want it to get details from you. As long as you know what to ask beforehand — write it at once, but oftentimes you can't quite find the correct words — that's OK. Claude is a fine interviewer. And you can do many iterations of questions gradually increasing the amount of detail. In some extreme cases I even suggest you start the process by making [a plan](https://code.claude.com/docs/en/how-claude-code-works#explore-before-implementing) for a detailed interview: Ask Claude to do some research and prepare a file with 20 questions and designated places for your answers. It will require some effort, but the result would be worth it.

Same thing applies to generic instructions:

> Mention /a, /b, and /c skills in your global CLAUDE.md. I want these to be very visible to agents.

## Knowledge Base

I suggest everyone find a way to take notes about things you find interesting. It can be as simple as a collection of text/markdown files or as complicated as an automated pipeline which summarizes the articles which you find useful. The important part is to teach Agents how to find information there (yes, that's one more skill I suggest you create). Being able to reference something local without resorting to web-search capabilities is extremely useful:

> I want you to apply the approach from that article by Martin Fowler I read last week

A habit of information hoarding finally pays off 🤪

{% note(clickable=false, hidden=false, header="anticipation") %}
_p.s. You probably noticed that I didn't say anything about project-level CLAUDE.md files — that was intentional. I find them to be less useful than most people consider them to be. I guess I should write a separate article about that._
{% end %}

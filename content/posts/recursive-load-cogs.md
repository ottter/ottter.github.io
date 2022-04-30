---
title: "Recursivly load all cogs in a Discord.py bot"
summary: "Creative method I've worked on to recursively load all cogs and split as much as possible out of the main startup file"
date: 2022-04-27T22:22:32-04:00

tags: [python, code, programming, discord]
categories: [python, code, discord, projects]
draft: true
---

I've been in the process of moving my discord bot from @dodo to [@dogdog](https://github.com/ottter/discord-bot) to
make it easier on myself to improve on what I originally made. I want to take up programming as a hobby again and why
not go back to where it all started. Anyway, here's a neat trick i've been using for a while.

As a diclaimer, all of this can be mostly replaced with something like what is below, but where is the fun in that?

    for key, value in bot.cogs.items():
        bot.load_extension(key)

![recursive extension 1](/images/discord/recursive-extension-1.png)

**`dir_list`** are all subdirectories of the parent directory. For organizational purposes I split them in to modules
(commands such as `.weather`), listeners (`on_message` and guild/member status changes), and discord's built in slash
commands.

**`exclusion_list`** is for specific *files* to exclude from loading. This is because within each subdirectory I have
most major commands split in to different folders to make it easier to find what I'm looking for. The customized `help` is currently disabled until I complete it.

These can (and should) be moved to a configuration file elsewhere. I only have them here for demonstration.

![recursive extension 1](/images/discord/recursive-extension-2.png)

Now `load_extensions()` is ran in the discord bot's startup script.

![recursive extension 1](/images/discord/recursive-extension-3.png)

... and the output is kinda clean too!

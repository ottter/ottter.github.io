---
title: "Recursively load all cogs in a Discord.py bot"
summary: "Overengineered method I've worked on to recursively load all Discord cogs"
date: 2022-04-27T22:22:32-04:00

tags: [python, code, programming, discord]
categories: [python, code, discord, projects]
draft: true
---

I've been in the process of moving my discord bot from @dodo to [@dogdog](https://github.com/ottter/discord-bot) to
make it easier on myself to improve on what I originally made. I want to take up programming as a hobby again and why
not go back to where it all started. Anyway, here's a neat trick i've been using for a while.

As a disclaimer, all of this can be mostly replaced with something like the below example, but where is the fun in
that?

```python
for key, value in bot.cogs.items():
    bot.load_extension(key)
```

In **`main.py`** :

```python
def load_extensions():
    """Load all modules/extensions/cogs from specificed directories"""
    dir_list = ['listeners', 'modules', 'slashes']
    exclusion_list = ['help']
    for dir_ in dir_list:
        print(f'=== Attempting to load all extensions in {dir_} directory ...')
        for filename in os.listdir(f'./{dir_}'):
            module = filename[:-3]
            if filename.endswith('.py') and module not in exclusion_list:
                try:
                    bot.load_extension(f'{dir_}.{module}')
                    print(f'\tSuccessfully loaded extension: {module}')
                except Exception as err:
                    exc = f'{type(err).__name__}: {err}'
                    print(f'\tFailed to load extension:  {module}\n\t\t{exc}')
    for excl_module in exclusion_list:
        print(f'=== Excluding the extension: {excl_module}')
```

**`dir_list`** are all subdirectories of the parent directory. For organizational purposes I split them in to modules
(commands such as .weather), listeners (on_message and guild/member status changes), and discord's built in slash
commands.

**`exclusion_list`** is for specific *files* to exclude from loading. This is because within each subdirectory I have
most major commands split in to different folders to make it easier to find what I'm looking for. The customized
`help` is currently disabled until I complete it.

These can (and should) be moved to a configuration file elsewhere. I only have them here for demonstration.

In **`main.py`** :

```python
def log_in():
    """Login function"""
    print('=== Initializing startup sequence ...')
    load_extensions()
    print('=== Attempting to log in to bot ...')
    try:
        bot.run(DISCORD_TOKEN)
    except discord.errors.HTTPException or discord.errors.LoginFailure as error:
        print('\nDiscord: Unsuccessful login:', error)
    else:
        sys.exit("Login Unsuccessful")


if __name__ == '__main__':
    log_in()
```

Now `load_extensions()` is ran in the discord bot's startup script.

```md
=== Initializing startup sequence ...
=== Attempting to load all extensions in listeners directory ...
        Successfully loaded extension: guild_events
        Successfully loaded extension: member_events
        Successfully loaded extension: on_message
=== Attempting to load all extensions in modules directory ...
        Successfully loaded extension: admin
        Successfully loaded extension: custom_command
        Successfully loaded extension: misc
        Successfully loaded extension: people
        Successfully loaded extension: xkcd
=== Attempting to load all extensions in slashes directory ...
        Successfully loaded extension: memes
=== Excluding the extension: help
=== Attempting to log in to bot ...
==================================
Logged in as:  testdogtestdog      
Client ID:     {Bot's UID}  
Local time:    2022-04-30 10:06:48 
================================== 
```

... and the output is kinda clean too!

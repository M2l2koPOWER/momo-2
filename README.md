# Momo, your new Discord bot
[![license](https://img.shields.io/github/license/paul-io/momo-discord.svg)](https://github.com/paul-io/momo-discord/blob/master/LICENSE) [![Dependencies](https://app.updateimpact.com/badge/809606116261629952/Momo%20Discord%20Bot.svg?config=test)](https://app.updateimpact.com/latest/809606116261629952/Momo%20Discord%20Bot) [![GitHub release](https://img.shields.io/github/release/paul-io/momo-discord.svg)](https://github.com/paul-io/momo-discord/releases) ![requirement](https://img.shields.io/badge/java%20req-java%208-green.svg) [![join the Momo discord server](https://img.shields.io/badge/discord-join%20now-74a2ed.svg)](https://discord.gg/uM3pyW8)
Join the discord server linked above to get support, see upcoming features, or to test the bot!

Momo is a simple-to-use Discord bot based off of [Discord4J](https://github.com/austinv11/Discord4J).  From sending Twitch.tv notifications to playing music, and from pulling anime theme songs off of [Themes.moe](https://themes.moe) to temporarily muting troublemakers, Momo can do a lot for your server.

## Adding Momo to your server
Visit [this link](https://discordapp.com/oauth2/authorize?client_id=259137993351102464&scope=bot&permissions=268435518) and select which server you want her to join. When she joins, go ahead and run `$info`. It's as easy as that!
#### What commands are available?
Send the command `$help` to her. If you need more information on specific commands, you can try `$help command-name` or just PM her the command name.

---
## Hosting Momo for yourself
* If you want to host your own instance of the bot, feel free to take a look at the Releases tab and download the package.
* In addition to downloading the package, edit the `Bot.properties` file as you see fit. The configuration you need to fill out are highlighted in the file - mostly API keys you can get for free for certain Bot functions.
* To obtain a bot token from Discord, head on over to the [Discord Developers](https://discordapp.com/developers/applications/me) page. From there, you can create an Application, then convert it to a Bot account. Doing so gives you a long bot token.
* Hosting Momo for yourself nets you some benefits. Music functionality, though dependent on your internet speeds, will be better for single servers than a larger cluster. You can also change its username, avatar, and game status to whatever you see fit!
---

## Pulling from the source & building
Momo uses [Apache Maven](https://maven.apache.org/) for project management. As such, it's extremely simple managing dependencies, so building any edits and changes you want into your own bot is easy.

#### Installing Maven
Linux: `apt-get install maven`
Windows & macOS: [download the package](http://maven.apache.org/download.cgi) and follow the instructions in the previously linked install page
**Windows & macOS alternative**
Windows & macOS users can install [Chocolatey](https://chocolatey.org/) & [Homebrew](http://brew.sh/) respectively to get `apt-get` functionality
chocolatey: `choco install maven`
homebrew: `brew install maven`

#### Building
Run `mvn install` on the root directory. This will create two builds: a `.jar` of the bot's source & a `.jar` with all the dependencies shaded (all packaged into a single file). This is the file you want - `momo-x.x.x.jar`. On subsequent builds, if you do not run the command with the `clean` parameter, then all `.jar` will be the correct bot.
**NOTE**: `mvn install` *does not* copy the resources folder to the `target/` directory. As a side effect, it *will not* overwrite pre-existing resources, so you are free to copy over `resources/` to `target/`.
**NOTE 2**: If you decide to run `mvn clean install`, *all folders and files in* `target/` *will be deleted*. Just a forewarning before you lose all of your server's data

#### Running
Once you have built the jar, simply run `java -jar momo-x.x.x.jar` where `x.x.x` is the current version numbering. 

### Creating a command
Probably the #1 reason people will run their own bot, and probably the easiest thing to implement with Momo. This example also shows how permissions are setup, so if you want to change the permission level of commands... You're in the right place.
1. Create a new class file. Preferably organized in the `io.ph.bot.commands` package, but that's up to you.
2. Let's say you call your command `Echo`, and it echoes whatever the user says. Make sure to have your file implement `io.ph.bot.commands.Command` and to override `run(IMessage msg)`
3. The meat of your command goes in the aformentioned `run` method. For brevity, our command ignore package and imports.
```java
public class Say implements Command {
    @Override
    public void run(IMessage msg) {
        MessageUtils.sendMessage(msg.getChannel(), Util.getCommandContents(msg));
    }
}
```
4. To then have the command register through the command handler, annotate the class with `io.ph.commands.CommandMeta`
```java
@CommandData (
		defaultSyntax = "echo",
		aliases = {"repeat", "ech0"},
		permission = Permission.NONE,
		description = "Have the bot repeat after you",
		example = "This will be echoed!"
		)
public class Say implements Command {
    @Override
    public void run(IMessage msg) {
        MessageUtils.sendMessage(msg.getChannel(), Util.getCommandContents(msg));
    }
}
```
It's as easy as that~ 
note: commands with permission `Permission.NONE` are disableable by admins by using the `disable` command

If you're going to delve deeper into developing with Discord4J, check out the documentation [here](https://jitpack.io/com/github/austinv11/Discord4j/websocket-rewrite-2.6.1-gf6f90c4-157/javadoc/index.html) and join up at the [Discord API server](https://discordapp.com/invite/0SBTUU1wZTWPnGdJ).
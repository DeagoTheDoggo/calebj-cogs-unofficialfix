# CalebJ Cogs *Unofficial* FIX
#### FORKED FROM [calebj-cogs](https://github.com/calebj/calebj-cogs)
#### General, fun and utility modules for [Red-DiscordBot](https://github.com/Cog-Creators/Red-DiscordBot/) v3.

[![Say Thanks!](https://img.shields.io/badge/Say%20Thanks-!-1EAEDB.svg)](https://saythanks.io/to/calebj)
[![Patreon](https://img.shields.io/badge/Support-me!-f96854.svg)](https://www.patreon.com/calebj)
[![Donate](https://img.shields.io/badge/Paypal-donate-0070ba.svg)](https://paypal.me/calebrj)
[![discord.py](https://img.shields.io/badge/discord-py-blue.svg)](https://github.com/Rapptz/discord.py)
[![Red-DiscordBot](https://img.shields.io/badge/Red--DiscordBot-v3-red)](https://github.com/Cog-Creators/Red-DiscordBot/)

This repo contains cogs I've written, as well as those I've modified and republished when a PR was either unwanted or too drastic of a change from the original cog's scope.

## Table of Contents
* [Installation](#installation)
* [Support and Contact](#support-and-contact)
* [Cog Summaries](#cog-summaries)
* [Cog Documentation](#cog-documentation)
  * [ActivityLog](#activitylog)
  * [Captcha](#captcha)
  * [Duel](#duel)
  * [EmbedWiz](#embedwiz)
  * [Gallery](#gallery)
  * [Punish](#punish)
  * [ReCensor](#recensor)
  * [Scheduler](#scheduler)
  * [ServerQuotes](#serverquotes)
  * [Watchdog](#watchdog)
  * [XORole](#xorole)
* [Contributing](#contributing)
* [Usage Statistics and Privacy Policy](#usage-statistics-and-privacy-policy)
* [License and Copyright](#license-and-copyright)

## Installation
Type the following commands into any channel your bot can see, or in a direct message to it.

**1. Adding the repo** - replace `[p]` with your bot's prefix:
```
[p]cog repo add calebj-cogs-unofficialfix https://github.com/DeagoTheDoggo/calebj-cogs-unofficialfix
```

**2. Installing a cog** - replace `[COG_NAME]` with the name of the cog you want:
```
[p]cog install calebj-cogs-unofficialfix [COG_NAME]
```

## Support and Contact
I have no official contact but you can support the ORIGINAL owner down below.

Support the original creator on [PayPal](https://paypal.me/calebrj) or [becoming a patron](https://www.patreon.com/calebj).

## Cog Summaries
* [activitylog](#activitylog): Log messages, attachments, and server changes to disk.
* [captcha](#captcha): Challenge new members with a random code to verify their humanity.
* customgcom: Bot-wide custom commands. Only the bot owner can add/remove them.
* datadog: Publish various metrics and events to a local statsd instance.
* description: Change the header of Red's [p]help command.
* dice: Wraps the python-dice library. Command is `[p]dice <expression>`.
* [duel](#duel): Procedurally generated duel with a flexible lexicon system.
* [embedwiz](#embedwiz): A tool to generate and post custom embeds.
* galias: Bot-wide command aliases. Only the bot owner can add/remove them.
* [gallery](#gallery): Automatically clean up comments in content-focused channels.
* [punish](#punish): Timed text+voice mute with anti-evasion, modlog cases, and more.
* purgepins: Delete pin notification messages after a per-channel interval.
* [recensor](#recensor): Filter messages using regular expressions
* [scheduler](#scheduler): Squid's [scheduler cog][squid_scheduler], with enhancements.
* [serverquotes](#serverquotes): Store and recall memorable quotes in your server.
* sinfo: Simple text dump of server and channel information.
* [watchdog](#watchdog): Helps systemd know when your bot is functioning.
* [xorole](#xorole): Self-role functionality with single-membership role sets.
* zalgo: H͕̭͒̈́E̡̩͋͐ C̺̻̉O̟͋M̞̐Ę͒ͅS̬̣̍́.

[squid_scheduler]: https://github.com/tekulvw/Squid-Plugins/blob/master/scheduler/scheduler.py

## Cog Documentation
### ActivityLog
This cog saves messages and DMs, attachments, and updates to server settings __to log files in your bot's data folder__. If you want a cog that posts events in a channel, use [Grenzpolizei](http://cogs.red/cogs/PaddoInWonderland/PaddoCogs/grenzpolizei/) by [PaddoInWonderland](https://github.com/PaddoInWonderland).

There is no support for uploading logfiles yet, though it is planned. Logging of embed contents and posting events in a channel are NOT planned features.

Activitylog can record the following events:
* Channel messages (per-channel): `logset channel {on|off} [#channel-name]`
  * Includes edits and deletions
* Server events (per-server): `logset events [on|off]`
  * Member changes: nickname, username, roles, join/leave, ban, kick
  * Server changes: name, region, owner, icon
  * Channel changes: create, delete, name, position, topic, permissions
  * Role changes: create, delete, permissions, name, color, hoist, mentionable, rank
* Server override (per-server): `logset server [on|off]`
  * If this is `on`, all channels and server events are logged.
* Direct messages: `logset dm [on|off]`
  * Also includes edits and deletions
* Message attachments: `logset attachments [on|off]`
* Default setting: `logset default [on|off]`
  * If you haven't set an option on or off, this default is used.
  * Server override, global override, and attachments don't use this.
* Global override: `logset everything [on|off]`
  * If this is `on`, the bot will log everything.
  * Attachment downloading is still its own setting.
* Log rotation: `logset rotation [period]`, where period is:
  * `none` : disable log rotation
  * `d` : one log file per day (starts 00:00Z each day)
  * `w` : one log file per week (starts 00:00Z each Monday)
  * `m` : one log file per month (starts 00:00Z on first day of month)
  * `y` : one log file per year (starts 00:00Z Jan 1)
  * Example: if monthly, all logs for July 2018 in channel ID 1234 would be in `20180701--P1M_1234.log`

Note: The version of discord.py that Red v2 is based on doesn't have a way to record audit logs, so there's no way to record which member made a particular change.

### Captcha
Captcha allows you to challenge incoming members with a code they must enter to become verified.

**NOTE:** Attempting to install this cog using `[p]cog install` will fail unless your version of Red is [up to date](https://github.com/Cog-Creators/Red-DiscordBot/commit/e19328188bbfbc551cb9f0e36d3ad78ece0d304e) as of 2018-07-29.

Challenges can be sent and responded to via direct messages or in a server channel, and verification/approval can either be indicated by the presence of or the absence of a role. If an image is impossible to decipher (users get unlimited retries, as incorrect codes are simply ignored), they can say "retry" to get a new one.

The cog supports three types of challenges:

| Name   | Example |
| ------ | ------- |
| plain  | ![plain example](captcha/example_plain.png?raw=true) |
| image  | ![image example](captcha/example_image.png?raw=true) |
| wheezy | ![wheezy example](captcha/example_wheezy.png?raw=true) |

Moderators and members with the Manage Roles permission can run these commands:
* `[p]captcha approve <member>` : manually bypass the captcha challenge for a member
* `[p]captcha approve-all` : mark all pending members as approved
* `[p]captcha challenge <member>` : unverify and challenge a member

Admins and members with the Manage Serve permission can run these commands:
* `[p]captchaset channel [channel]` : set which channel captchas are posted in
* `[p]captchaset dm [on|off]` : set whether captchas are sent via DMs
* `[p]captchaset enabled [on|off]` : turns the cog on and off in the server
* `[p]captchaset kick-delay [duration]` : set how long to wait for a correct code before kicking
* `[p]captchaset on-join [on|off]` : set whether members are challenged upon joining (default on)
* `[p]captchaset on-role [on|off]` : set whether add/removing the un/verified role triggers a challenge (default on)
* `[p]captchaset retry-delay [duration]` : set how long members have to wait to regenerate their code
* `[p]captchaset role [role]` : set which role the cog uses to mark un/verified members
* `[p]captchaset role-mode [[un]verified]` : set whether the role means members are verified or unverified
* `[p]captchaset type [image|plain|wheezy]` : set the type of captchas to generate (see table above)

`[duration]` can be any combination of numbers and units, e.g. 5m30s, or a long format such as "5 minutes and 30 seconds". Valid units are `s`, `m`, `h`, `d`, `w`. It can also be `none` or `disable`.

### Duel
The Duel cog was written as a fun exercise in procedural generation. It only has a few commands:

* `[p]duel <member>` : starts a randomly generated duel with a member
* `[p]duels list [N=10]` : display the top N entries in the duels leaderboard
* `[p]protect me` : enable or purchase duel protection (set by admins)
* `[p]protected` : display a list of all protected members and roles
* `[p]unprotect me` : opt out of duel protection

Moderators can use the following commands:
* `[p]protect user <member>` : add a member to the protected users list
* `[p]unprotect user <member>` : remove a member from the protected users list

And administrators can use these commands:
* `[p]duels reset` : clear duel score history (preserves settings and protection list)
* `[p]duels editmode [yes_no]` : Set or show whether duels will edit in-place
  * Disabled by default. When disabled, each move will be a new message.
* `[p]protect price [option]` : enable, disable, or set the price of self-protection
  * Self-protection is disabled by default. Options are "free", "disable", or a number.
* `[p]protect role <role>` : add a role to the protection list
* `[p]unprotect role <role>` : remove a role from the protection list

### EmbedWiz

Embedwiz can build an embed in two ways: ordered parameters, and keyword parameters.

To use **ordered** parameters, specify the following seperated by a semicolon (`;`):
* Title: to make a link, put `[Title text](title url)`
* Color: "none" (default), #ABC123, 0xABC123, "random", "black" or any member of [`discord.Colour`](https://discordpy.readthedocs.io/en/async/api.html#discord.Colour)
* Footer text
* Footer icon URL
* Embed image URL
* Thumbnail image URL
* Embed body text: or "prompt" to make the next message you post the embed body.

All fields can be left blank. If Discord doesn't like a title link or image URL, it may throw an error.

To use **keyword** parameters, add `-kw` to the beginning of the spec and specify one or more of the following seperated by a semicolon (`;`):
* `title=` your title or a markdown-format link as described above
* `url=` a valid URL to assign to the embed title
* `color=` a color specification (see above)
* `footer=` the desired footer text
* `footer_icon=` a valid image URL to use as the footer icon
* `image=` a valid image URL to use as the embed content image
* `thumbnail=` a valid image URL to use as the embed thumbnail
* `timestamp=` an ISO8601 timestamp, UNIX timestamp or 'now'
* `body=` the embed body text, or "prompt" as described above

By default, author information is included in the embed's header so that it can be edited later using the `embedwiz edit` command. To omit this information, put `-noauthor` in front of the between the command and its parameters. Keep in mind that you won't be able to edit it later if you do this.

An example **ordered** embed specification:
```
[p]embedwiz -noauthor [Test Embed](https://github.com/calebj/calebj-cogs/);
#2196F3;
Created by embedwiz;
https://calebj.io/images/l3icon.png;
https://upload.wikimedia.org/wikipedia/commons/b/bf/Test_card.png;
https://www.python.org/static/community_logos/python-powered-h-140x182.png;
Example embed body text. Full **markdown** __is__ *supported*. ~~Nothing to see here.~~

Newlines also work.

And now for [something completely different](https://www.youtube.com/watch?v=ltmMJntSfQI).
```

Or, using **keyword** parameters:
```
[p]embedwiz -noauthor -kw title=Test Embed;
url=https://github.com/calebj/calebj-cogs/;
color=#2196F3;
footer=Created by embedwiz;
footer_icon=https://calebj.io/images/l3icon.png;
image=https://upload.wikimedia.org/wikipedia/commons/b/bf/Test_card.png;
thumbnail=https://www.python.org/static/community_logos/python-powered-h-140x182.png;
body=Example embed body text. Full **markdown** __is__ *supported*. ~~Nothing to see here.~~

Newlines also work.

And now for [something completely different](https://www.youtube.com/watch?v=ltmMJntSfQI).
```

Both of which looks like this:

![Test embed](embedwiz/test.png?raw=true)

Only mods and those with the manage_messages permission can use these subcommands:
* `embedwiz channel [channel] [embed_spec ...]` : posts the embed in the channel `channel` instead.
* `embedwiz delete [embed_spec ...]` : deletes the command message (and prompt message, if used).
* `embedwiz edit [channel] [message_id] [embed_spec ...]` : edits **any** existing embed.

### Gallery
The gallery cog started as a project to automatically remove comments made in art channels after a specified time, while leaving the art posts intact.

Every 5 minutes it checks the message history in a channel, from the configured maximum age through two weeks in the past. For each message, it performs the following checks:
1. If the message has a ❌ reaction on it from a mod or admin, it's deleted
2. If `privonly` is enabled and the message isn't by a "privileged" user OR "pinned", it's deleted
3. If `pinsonly` is enabled and the message isn't "pinned" (see below), it's deleted
4. If the message doesn't have an attachment or embed, and isn't "pinned", it's deleted
5. Otherwise, the message is kept

"Pinned" messages describe messages for which any of the following is true:
1. The message is pinned in the channel (via the Discord pinning feature)
2. The message text contains any of the configured "pin" emojis, or
3. The message has any "pin" emoji reactions from a mod, admin, or member with the artist role

"Pin" emojis can be set with `[p]galset emotes`, and default to 🎨 and 📌.
"Privileged" users are anyone with the mod, admin (according to `[p]modset`) or artist (`[p]galset role`) role.

The cog is configured for the current channel using the following commands:
- `[p]galset turn [on_off]` : enable or disable message cleanup
- `[p]galset privonly [on_off]` : set or display whether only privileged users' messages are kept
- `[p]galset pinsonly [on_off]` : set or display whether only pinned messages are kept
- `[p]galset emotes [emote1 emote2 ...]` : set or display the "pin" emojis
- `[p]galset age [timespec]` : set or display the maximum age for non-art messages
- `[p]galset role [role]` : enable or disable the gallery cog

To show the current settings, run `[p]galset` without a subcommand.

As of v1.6.0, the owner-only `[p]galset vacuum` command was added to purge deleted channels.

### Punish
The punish cog automatically sets itself up in most cases. In case some role configurations need to be re-applied, the role needs to be recreated, etc., run `[p]punishset setup`.

To "punish" a user, simply run `[p]punish <user> [duration] [optional reason ...]`, where `duration` can be `forever` or `infinite` to set no end time. If no duration of provided, the default of 30 minutes is used. If the user is already punished, their timer will be updated to match the provided duration, and the reason will be updated if a new one is given.

`[duration]` can be any combination of numbers and units, e.g. 5m30s, or a long format such as "5 minutes and 30 seconds". Valid units are `s`, `m`, `h`, `d`, `w`. Intervals containing spaces must be in double quotes. The values `forever`, `infinite`, and `disable` are command-specific.

To end a punishment before the time has run out, run `[p]punish end <user> [optinal end reason ...]`. The role can also be removed manually, but this isn't recommended because there currently isn't a way for the bot to know who removed the role.

There is also a `[p]punish warn <user> [optional reason ...]` command, but all it does is format a boilerplate message. In the future, it might support custom responses or be used for tracking warnings/strikes.

#### Modlog integration and updating punishment reasons
By default, durations longer than 30 minutes will create modlog cases. Naturally, if the mod cog is not loaded or no modlog channel is set, no cases will be created. To view, adjust or disable this setting, use the `[p]punishset case-min [duration]`, where `duration` can be `disable`, or left blank to view the current setting.

Updating the reason for punishment has its own command, since the `[p]reason` command only updates the modlog. That command is `[p]punish reason <user> [new reason ...]`, and it will automatically update any existing modlog case. __If the new reason is left blank, it will be cleared.__

#### Custom permission overrides and timeout channel
Channel overrides for the punish role be easily customized and applied to all channels in a server (except the timeout channel, as explained below). To copy the overrides from a channel, run `[p]punishset overrides [channel]`. The channel's type can be either text or voice; the overrides for each type are seperate. To display the current overrides, leave `channel` blank.

Once set, overrides can be deployed to all channels with `[p]punishset setup`. To restore the default overrides, run `[p]punishset reset-overrides [channel_type]`, where channel_type is `voice`, `text`, or `both` (the default).

Note: if the voice override does not deny speak or connect permissions to the punished role, the cog will not automatically enable server-wide voice mute to punished users.

It is possible to designate a special channel which punished users are allowed to speak in (for example, to discuss their infractions in private with a moderator). When set using `[p]punishset channel [channel]`, the punished role is automatically granted explicit permissions to read and speak in that channel. To view the current channel, run `[p]punishset channel` without any arguments. Or, to revert this setting and restore the normal permission overrides, run `[p]punishset clear-channel`.

If the punished user(s) continue to be disruptive in the timeout channel, moderators can utilize the `[p]punish channel-hush [on_off]` command. When ON, the channel's permissions are changed to prevent them from posting in the channel, but still allow them to read it. This setting automatically resets to OFF when no more punishments are active.

#### Punished user list formatting
Support for multi-line headers and cells was added in tabluate version 0.8.0. If the installed version is older than that, the formatting for `[p]punish list` will revert to a single-row layout, which can easily overflow and cause ugly formatting. To prevent this, simply update tabulate (a quick shortcut to do so is `[p]debug bot.pip_install('tabulate')`).

#### Absent/banned user cleanup
If a member leaves before their punishment expires or is removed, they may remain in the list indefinitely. To remove members whose time has expired, use the `[p]punish clean [pending=no]` command. If run with `yes` as the argument, then it will also remove members who are no longer in the server, even if their time has not yet expired. Note that this will prevent the punish role from being re-added if they rejoin.

As of version 2.2.0 (published 2018-12-29), users are automatically removed from the punish list upon being banned, regardless of when their timer expires. To remove users from the list who were banned before the update, use the `[p]punish clean-bans` command.

#### Role Auto-Removal
As of version 2.4.0 (published 2019-02-23), administrators can define a list of roles to be automatically removed when a user is punished and re-added when it ends. Two commands control this feature:
- `[p]punishset remove-roles role1,role2,...,roleN` : Set the list of roles to auto-remove. Comma seperated.
  - If no roles are provided, the current list is displayed.
  - Role IDs can be used in place of names.
  - The special value of `role_list_clear` will clear the list.
- `[p]punishset sync-roles` : Apply changes to the auto-remove list to all currently punished users.
  - This command should also be used after correcting a hierarchy issue that prevented the bot from removing a role.

The bot will also automatically revert any attempt to re-add a role in a user's "removed" list while a punishment is active, even if the role was added while the bot was offline. Changing the server-wide list will not take effect for a member until you run `sync-roles`.

Roles that are moved too high in the hierarchy for the bot to manage will be silently ignored until the hierarchy is corrected. Running `sync-roles` will scan for any roles that should have been removed and do so.

IMPORTANT: Punish doesn't (currently) handle the case when a member leaves and rejoins *after* their time expires. Since data on expired cases for absent members isn't retained, their roles will have to be re-added manually.

Credit to @brandons209 for this feature idea and implementation.

### ReCensor
The recensor cog uses Python's built-in [`re`](https://docs.python.org/3/library/re.html) module to decide which messages to filter. An introduction to Python regex can be found [here](https://docs.python.org/3/howto/regex.html#regex-howto), and the full syntax is described [here](https://docs.python.org/3/library/re.html#regular-expression-syntax).

**Note for Windows users:** because of OS limitations combined with an [upstream Red issue](https://github.com/Cog-Creators/Red-DiscordBot/pull/1956), a situation called [catastrophic backtracking](https://www.regular-expressions.info/catastrophic.html) can cause your bot to stop responding. On Linux and OSX, the cog uses process-level isolation to prevent this from happening. Once that pull request is merged, Windows users will have the same protection, and this note will be removed.

**Note for webhooks:** although they don't post as a server member, recensor treats them as if they belong to the default (`@everyone`) role for the purposes of role-based filtering.

Most of the configuration will be done with the following commands:
- `[p]recensor create <name> [pattern]` : creates a new filter
- `[p]recensor copy <name> <newname> [link]` : Copies an existing filter, with optional link
- `[p]recensor debug <message_id> [channel]` : Tests a message against all configured filters
- `[p]recensor test <name>` : interactively tests an existing filter
- `[p]recensor regex101 [test message]` : opens a pattern in regex101.com with optional test message
- `[p]recensor help` : displays links to reference material (such as this README)
- `[p]recensor <name> [setting] [options]` : show or change a filter's settings (see below)
- `[p]recensor server [setting] [options]` : show or change the server defaults (see below)
- `[p]recensor rename <oldname> <newname>` : renames a filter
- `[p]recensor show [name]` : displays information about all or one filter(s) in the server
- `[p]recensor delete <name>` : deletes a filter

Each filter in a server has the following settings. To configure or check the value of a setting, use `[p]recensor FILTERNAME SETTINGNAME [newvalue]`.
- `enabled` : self-explanatory
- `mode` : controls whether the filter behaves like a `white`list or `black`list
- `override`: controls the filter's priority (see Filter Priority below)
- `pattern` : the regular expression itself
- `flags` for the regular expression, which are explained in detail [here](https://docs.python.org/3/howto/regex.html#compilation-flags)
  - this is how to set case sensitivity and newline behavior, among other things
  - default flags are `i` (case insensitive) and `s` (dot matches newline)
- `multi-msg`: search for a match spanning several messages
  - looks at the last 64 messages or 10 minutes, whichever is less.
  - currently looks at per-user per-channel sequences; matches do not span users.
  - **BIG SCARY WARNING:** greedy wildcards can match DOZENS OF MESSAGES!
- `multi-join`: what string to join messages on (defaults to `\n`)
 - if you want to match subsequent messages without any gaps, set this to `""` (empty)
- `multi-group`: if not 0, which matched group to delete
  - NOTE: the join string is considered a part of the previous message.<br>For example, `foo(\nbar)` will match BOTH messages in the case of "foo" then "bar" with join `\n`.
- `position`: controls which part of the message has to match (defaults to `anywhere`):
  - `start` : only looks at the beginning of the message (`re.match`)
  - `anywhere` : scans through the full message looking for a match (`re.search`)
  - `full` : the entire message must match, from start to finish (`re.fullmatch`)
- `roles` that are or aren't subject to the filter (see List Configuration below)
- `channels` in which the filter is or isn't in effect (see List Configuration below)
- `priv-exempt`: whether mods, admins and the server owner are immune to the filter
  - __Overrides__ the server default if set. Specify `inherit` to use the server setting.
- `asciify`: attempt to reduce unicode text to its equivalent ASCII before matching
  - __Overrides__ the server default if set. Specify `inherit` to use the server setting.
- `attachment`: prepends `{attachment:FILENAME}` to applicable messages when enabled.
  - to prevent spoofing, an extra `{` is prepended when the message starts with `{attachment:`.
- `msg`: sent when the filter is triggered
  - Supports `{author}`, `{channel}`, `{filter}` and `{match}` objects in `format()` template style.
  - If multiple whitelist filters are checked (ambiguous trigger criteria), this isn't sent.
- `msg-dm`: whether the trigger message is sent via DMs rather than in the channel.
- `msg-sec`: controls how long the trigger message remains before being auto-deleted.
  - Defaults to 5, disabled for DM. Limit is 60 seconds. -1 to disable, 0 for forever.

The cog also supports configuring the following server-wide settings. To configure or check the value of a server setting, use `[p]recensor server SETTINGNAME [newvalue]`.
- A `priv-exempt` toggle, which makes moderators, admins and the server owner immune from *all* filters by default
- An `asciify` toggle, which makes the cog attempt to reduce unicode text to its equivalent ASCII by default
- A list of `channels` where messages will or will not be filtered
  - only applies to filters whose lists have overlay enabled
- A list of `roles` that are either immune or exclusively subject to any filters
  - only applies to filters whose lists have overlay enabled

#### Filter Priority
For messages that match the roles and channels configuration, the filtering process is as follows:
1. if the message matches any black filters that have override mode on, delete it;
2. if the message matches any white filters that have override mode on, allow it and stop checking;
3. if the server or channel has ANY non-override white filters and the message didn't match any of them, delete it;
4. if the message matches ANY black filters, delete it;
5. if none of the above apply, the message is not deleted

In short, the filter priority is black override > white override > white > black. The old terminology of inclusive/exclusive was deemed confusing, so the terms are now black and white, respectively.

#### List Configuration
For full flexibility, each filter can be set to apply to any number of roles or channels, described by a blacklist or whitelist. The server-wide lists act as a starting point for any filters that have `overlay` enabled (see below), and filter lists can be linked to each other for centralized management.

To configure a list, run `[p]recensor FILTERNAME LISTNAME OPERATION [options]`, where `FILTERNAME` is `SERVER` or the name of a filter, and `LISTNAME` is one of `channels` or `roles`, and `OPERATION` is one of those listed below.

The `channels` and `roles` settings are manipulated through a standard interface using the following operations:
- `mode` : set whether the list is a `blacklist` or `whitelist`
- `enabled` : self-explanatory. If a list is disabled, it will be skipped when filtering messages.
- `add` : adds one or more items to the list
- `remove` : removes one or more items from the list
- `overlay` : set whether the list "overlays" its server-wide counterpart
  - Overlay is the default behavior; if disabled, the list is independent of the server's.
- `clear` : removes all items from the list (doesn't reset the mode)
- `cleanup` : removes references to deleted items from the list
- `invert` : replaces the contents of the list with all items that are not in the list
- `link` : replaces the list with a reference to another (WARNING: erases the list's configuration!)
  - Only filter lists can be linked; the server-wide list is standalone.
- `unlink` : replaces a linked list with a copy of the link's former target

Set operations take the name of another filter (or `SERVER`) as the only argument:
- `replace` : replace the contents of this list with another
- `union` : like `replace`, but only adds new items
- `difference` : removes any items that are in the other list
- `intersect` : removes any items that are __not__ also in the other list
- `symdiff` : replaces the list with items that are in __either__ list, but not both

If `overlay` is enabled for a filter, the server list acts as "all items" for that filter. For example, if the server's list excludes two channels (A and B) from being filtered, and the filter list excludes two more (C and D), all four will be excluded. If the list is instead set to filter in only A and C, A will still be excluded and the filter will not function there. A warning will be shown in cases like this. Overlay applies to the `invert` operation as well: inverting a filter that excludes C and D will not exclude A and B.

#### Example Patterns
Links:
- All URLs: `\b(?:https?|ftp)://[^\s/$.?#].[^\s]*`
- Discord invites: `\b(?:https?://)?discord(?:app\.com/invite|\.(?:io|me|li|gg))/[^\s/]+`
- To allow only one kind of URL but not the other:
  - only Discord invites or no URL: blacklist normal URLs, whitelist override Discord
  - only normal URLs or no URL: blacklist Discord invites
  - only Discord invites: whitelist Discord invites
  - only normal URLs: blacklist override Discord invites, whitelist normal

Miscellaneous:
- Custom emotes: `<a?:\w+:\d+>\s*`
- 5 consecutive all-caps words (with punctuation): `(?:[A-Z]+\b[\s.!]*){5,}`
  - NOTE: requires the I flag to be removed, as it is enabled by default
- 30 consecutive characters/mentions: `(?s)^(?:<(?:[#@&!]+|a?:\w+:)\d+>|.(?<!<[#@&:!])){30,}`
  - Emotes and user, role, and channel mentions count as one character

Attachments (replace `<PATTERN>` with the message content pattern):
- Messages with pictures attached: `^\{attachment:.*\.(?:png|jpg|jpeg|gif|pdf)\}.*<PATTERN>`
- Messages with no attachments only `^(?!\{attachment:)<PATTERN>`
- Messages with ONLY attachments (no text): `^\{attachment:.*\}$`

#### Caveats for Multi-Message Whitelist Mode:
1. Be *very careful* with the `^` or `$` anchors!
  - They represent the beginning or end of a user's entire message sequence in a channel.
2. Formerly allowed messages __will not__ be deleted if new messages or edits break a partial match.
  - If all messages from a former match are still in memory, edits or deletions *can* trigger action.
3. Both the `position` and `multi-group` settings are ignored.
  - `position` is considered to be `anywhere`, and the entire match is used.
4. __All__ occurances of a match in the history are respected (unless `^$` are used).
  - This is to prevent false negatives on repetitions of a whitelisted string.

### Scheduler
The scheduler cog supports a number of different functions. All users can schedule a command to run in the future ("one-shot"), but only moderators and members with the "manage messages" permission can add or manage repeating commands. Additionally, only one of the same command can be scheduled at a time for each member. To schedule it again, they must cancel the one they scheduled before.

Time interval can be any combination of numbers and units, e.g. 5m30s, or a long format such as "5 minutes and 30 seconds". Valid units are `s`, `m`, `h`, `d`, `w`. For all subcommands that don't have the time interval as the last argument, intervals containing spaces must be in double quotes.

Timestamp can be an ISO8601 timestamp (e.g. 2017-12-11T01:15:03.449371-0500), UNIX timestamp (e.g. 1512972903.449371) or "now".

Regular members can use these subcommands to `[p]scheduler`:
* `add [time_interval] [command ...]`: schedules the specified command to run in `time_interval`.
  * `time_interval` must be in double quotes if it contains spaces.
* `add_timelast [command] [time_interval ...]`: `add`, but `time_interval` is the last parameter.
  * `command` must be in double quotes if it contains spaces.
  * Useful for creating command aliases that take a variable time.
* `add_twostage [command_1] [time_interval] [command_2 ...]`: runs `command_1`, schedules `command_2`.
  * `command_1` and `time_interval` must be in double quotes if they contain spaces.
* `add_twostage_timelast [command_1] [command_2] [time_interval ...]`: `add_twostage`, but interval is last.
  * `command_1` and `command_2` must be in double quotes if they contain spaces.
  * Useful for creating command aliases that take a variable time.
* `cancel [command ...]`: cancels a scheduled command. You can only cancel your scheduled commands.

Moderators and members with the "manage messages" permission can run these subcommands:
* `repeat [name] [time_interval] [command ...]`: repeats the given command every `time_interval`.
* `repeat_from [name] [start] [interval] [command ...]`: `repeat`, starting at timestamp `start`.
* `repeat_in name start_in interval command ...`: `repeat`, starting in the interval `start_in`.
* `remove [name]`: removes the repeating command named `name` and cancels the next scheduled run.
* `list`: lists the names of all scheduled commands (TODO: show channel and command).
  * Also shows scheduled oneshots. Cancel them by using `remove` with the full `UID-name`.

An example application of twostage is to have a self-assigned role (using selfrole from the [Squid Admin cog](http://cogs.red/cogs/tekulvw/Squid-Plugins/admin/)) that is added and then removed after a custom delay, using a single alias.

### ServerQuotes
Anyone can run these commands:
* `[p]quote by <member> [show_all]` : displays one or all quotes by a member
* `[p]quote by-nm <author> [show_all]` : displays one or all quotes by a non-member author
* `[p]quote dump` : uploads a CSV with all of the server's quote data
* `[p]quote list [random]` : displays all quotes, optionally jumping to a random one
* `[p]quote me [show_all]` : displays one or all quotes by the calling member
* `[p]quote search <query>` : searches quotes by text and displays them in order of relevance
* `[p]quote show <num>` : displays an individual quote by its number

These commands relate to "global" quotes, which can be accessed in all servers and even by DM:
* `[p]gquote by-nm <author> [show_all]` : displays one or all global quotes by a non-member author
* `[p]gquote list [random]` : displays all global quotes, optionally jumping to a random one
* `[p]gquote me [show_all]` : displays one or all global quotes by the calling member
* `[p]gquote search <query>` : searches global quotes by text and displays them in order of relevance
* `[p]gquote show <num>` : displays an individual global quote by its number

Moderators, admins, and people with Manage Messages permissions can use:
* `[p]quote add <member> <quote ...>` : adds a quote by the specified member
* `[p]quote add-msg <message ID> [channel]` : adds an entire message as a quote
  * channel is required if the message is from a different channel than where the command is run
* `[p]quote add-nm <author> <quote ...>` : adds a quote by the specified author
  * this allows quotes from non-members to be added; use `[p]quote by-nm` to display them
* `[p]quote remove <num>` : deletes a quote by its number

Only server admins can use:
* `[p]quote global <num> [YES|no]` : set whether a quote should be made global
  * If run without an argument, defaults to yes
* `[p]quote link [server_id]` : adds a link to a server ID if provided, or lists all links
* `[p]quote unlink <server_id> ` : removes a link to a server by ID

And finally, only the bot owner can use these commands:
* `[p]gquote add <member> <quote ...>` : adds a global quote by the specified member
* `[p]gquote add-msg <message ID> [channel]` : adds an entire message as a global quote
  * channel is required if the message is from a different channel than where the command is run
* `[p]gquote add-nm <author> <quote ...>` : adds a global quote by the specified author
  * this allows quotes from non-members to be added; use `[p]gquote by-nm` to display them
* `[p]gquote remove <num>` : deletes a global quote by its number
* `[p]gquote unpublish <num>` : unmarks a published quote as global
  * NOTE: quotes that weren't published from a server cannot be unpublished

The `[p]gquote add` commands will not associate the added quote with a server.

The cog uses sqlite's [FTS4 extension](https://sqlite.org/fts3.html) for text indexing with a [Porter stemming tokenizer](https://tartarus.org/martin/PorterStemmer/), and ranks search results by the [Okapi BM25](https://en.wikipedia.org/wiki/Okapi_BM25) algorithm.

### Watchdog
First of all, if you aren't running your bot on a Linux machine with systemd, or another service manager that listens for watchdog messages in the same way, **this cog won't do anything for you**. Sorry.

With that out of the way, some important information: **this cog doesn't actually restart your bot for you**. Systemd has to do that. All the cog does is periodically send systemd messages that everything is working.

The service has to be configured to expect these messages by adding:

```
# Discord hearbeat is about every 40 seconds. Using 90 to be safe from false restarts.
WatchdogSec=90
```

to your service unit. As long as the environment variables from systemd are
passed through to the bot (i.e. you aren't using the launcher), the cog will
know where to send the OK messages.

If you are new to systemd, you can use the provided [`red@.service`](watchdog/red@.service) unit template. It assumes that:
1. there exists a user and group named `red` for your bot to run under,
2. the bot files are located in `/srv/red/<some name>/`,
3. all files in 2 are owned by user `red` and group `red`, and
4. you enable and start the service as `red@<some name>`

Assuming your bot is set up (i.e. token set, etc) and is located in a folder named `Red-DiscordBot`; and that `red@.service` is in `/etc/systemd/system/`:
base
If your bot's nickname is Squid, you would do the following (as root):
```sh
# Create the base folder, if it doesn't exist
mkdir -p /srv/red/

# Create the red user if it doesn't exist
id -u red 2>/dev/null || useradd red -r -U -d /srv/red/

# Move the bot's files to a subdirectory of /srv/red/
mv Red-DiscordBot /srv/red/squid

# Change the ownership to the bot user
chown red:red /srv/red -R

# Enable and start the bot
systemctl enable --now red@squid
```

After which your bot should now be running (check with `systemctl status red@squid`).

### XORole
XORole was created out of the need for "role sets", which are groups of roles that a member should only have one of at a time. Thus the name, which is short for "exclusive-or roles". Examples use cases include self-assignable color roles, timezone or region roles, and teams (e.g. Ingress or Pokemon Go).

The following user commands are available as subcommands of `xorole`:
* `list`: lists available rolesets and the roles in them.
* `add [role]`: assigns a user a role, removing any others in the same set.
* `toggle [roleset]`: toggle a role on or off, or between a two-role set.
* `remove [role|roleset]`: removes the `role` or their role in `roleset` from the user.

For a roleset of size one, `xorole toggle` will add the role if they don't have it, or remove it if they do. For a roleset of size two, it will toggle between the two roles (one must already be assigned). For other sizes, the command will error.

Calling `[p]xorole [role]` without a valid subcommand is the same as calling `[p]xorole add [role]`.

The cog is configured using the following subcommands of `xoroleset`:
* `addroleset [name]`: creates a new roleset.
* `audit`: lists members that have more than one role in a xorole roleset.
* `autodelete [seconds]`: show or set the auto-delete delay for xorole commands and responses.
  - Max delay 60s. 0 to disable, leave blank to show the current setting.
* `autoswitch [on_off] [rolesets]...`: show or change the autoswitch feature settings.
  - When autoswitch is on, adding a role from a set without using the xorole command will remove any others in that set.
  - Run without parameters to show current autoswitch settings.
  - Both a server default and roleset-specific overrides can be configured.
* `rmroleset [name]`: deletes a roleset.
* `renroleset [old_name] [new_name]`: renames a roleset.
* `addroles [roleset] [role,[role,...]]`: adds a comma-seperated list of roles (or IDs) to a roleset.
* `rmroles [roleset] [role,[role,...]]`: removes a comma-seperated list of roles (or IDs) from a roleset.

## Contributing
Please submit patches to code or documentation as GitHub pull requests!

Contributions must be licensed under the GNU GPLv3. The contributor retains the copyright. I won't accept new cogs unless I want to support them.

## Usage Statistics and Privacy Policy
Most of the cogs in this repo use a custom [usage reporting tool](_analytics/analytics_core.py) which, upon being loaded, checks to see if you have opted in before sending anything. I use [Matomo](https://matomo.org/) on my webserver to record events, and don't share the data with anyone else. All user IDs are hashed before sending to make them unique without being able to identify the users (or bots) in question, and IP addresses are stripped. Only "Pseudonymous Data" is collected or stored, and since none of it is for commercial use, I am not obligated to respond to GDPR requests. More information can be found in my [privacy policy](PRIVACY.md).

The gibberish seen at the top of most of the cogs is actually a compressed form of the tool's code, generated by [pyminifier](https://github.com/liftoff/pyminifier) and [this script](_analytics/build_analytics.sh), then updated by [this script](_analytics/substitute_analytics.sh). I included these scripts to allow others to easily verify that the output is identical.

## License and Copyright
Except for [scheduler](scheduler/), which is licensed under the [MIT license](scheduler/LICENSE), all code in this repository is licensed under the [GNU General Public License version 3](LICENSE).

Copyright (c) 2016-2018 Caleb Johnson, contributors and original authors.

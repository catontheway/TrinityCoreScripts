# Description
World_Chat script enables you to talk globally with your faction or with both factions (if enabled by server).
The GM can see both factions messages if he has the chat active and gm mode on. (.gm on)
# Commands
List of fully functional commands:
* .chat <$TEXT>
  - Used to talk on the World Chat
* .chat on
  - Used to enable World Chat
* .chat off
  - Used to disable World Chat
* .chata <$TEXT>
  - Used to talk for GM's to alliance faction ( so you can talk to the other faction when cross-faction world chat is disabled )
* .chath <$TEXT>
  - Used to talk for GM's to horde faction ( so you can talk to the other factions when cross-faction world chat is disabled )
  
# Installation
## Core Setup
Download the current version of the script from here:
https://github.com/wizzymore/TrinityCoreScripts/blob/master/Custom/World_Chat.cpp

To add a custom script follow the next steps:
1. Place the file downloaded in your <source_of_335>/src/server/scripts/Custom
2. Add the next line after the last function in <soruce_of_335>/src/server/scripts/Custom/custom_script_loader.cpp
```cpp
void AddSC_World_Chat();  // world_chat.cpp
```
3. Add the next line at the end of the function ```void AddCustomScripts()```
```cpp
AddSC_World_Chat();  // world_chat.cpp
```
4. Add the next line at the end of ```enum RBACPermissions``` in <soruce_of_335>/src/server/game/Accounts/RBAC.h
```cpp
    RBAC_PERM_COMMAND_WORLD_CHAT                             = 1009, // RBAC PERMISSION .chat
    RBAC_PERM_COMMAND_WORLD_CHAT_HORDE                       = 1010, // RBAC-PERMISSION .chath
    RBAC_PERM_COMMAND_WORLD_CHAT_ALLIANCE                    = 1011, // RBAC-PERMISSION .chata
```
5. Compile and follow the next step.

## Database Setup
### Setting up commands
```sql
INSERT INTO `command` (`name`, `permission`, `help`) VALUES ('chata', 1011, 'Syntax: .chata $text - To speak as a GM only to Alliance');
INSERT INTO `command` (`name`, `permission`, `help`) VALUES ('chath', 1010, 'Syntax: .chath $text - To speak as a GM only to Horde');
INSERT INTO `command` (`name`, `permission`, `help`) VALUES ('chat', 1009, 'Syntax: .chat $text\r\n.chat on To show World Chat\r\n.chat off To hide World Chat');
```

### Setting up command permissions
```sql
DELETE FROM `rbac_linked_permissions` WHERE `id`=194 AND `linkedId`=1011;
DELETE FROM `rbac_linked_permissions` WHERE `id`=195 AND `linkedId`=1009;
DELETE FROM `rbac_linked_permissions` WHERE `id`=194 AND `linkedId`=1010;
DELETE FROM `rbac_permissions` WHERE `id`=1009;
DELETE FROM `rbac_permissions` WHERE `id`=1010;
DELETE FROM `rbac_permissions` WHERE `id`=1011;
INSERT INTO `rbac_linked_permissions` (`id`, `linkedId`) VALUES (194, 1011), (194, 1010), (195, 1009);
INSERT INTO `rbac_permissions` (`id`, `name`) VALUES (1009, 'Command: chat'), (1010, 'Command: chath'), (1011, 'Command: chata');
```
## Server Config Setup
At the end of your worldserver.conf at this code:
```
###################################################################################################
# WIZZY WORLD CHAT SYSTEM
#
# These settings control WiiZZy's World Chat System
#
#    World_Chat.Enable
#        Description: Enables WiiZZy's World Chat System
#        Default:     1 - (Enabled)
#                     0 - (Disabled)

World_Chat.Enable = 1

#
#    World_Chat.CrossFactions
#        Description: Enables world chatting cross-faction
#        Default:     0 - (Disabled)
#                     1 - (Enabled)
#

World_Chat.CrossFactions = 0

#
###################################################################################################
```
## Start the server and enjoy
Done, you are ready to use the World Chat System! Go online and try it out!

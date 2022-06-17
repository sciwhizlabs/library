---
hide:
  - footer
---

# Sides

One of the most important concepts that a mod developer should know and understand is
the concept of _sides_.

In Minecraft, we deal with two types of sides: the **physical sides** and the **logical
sides**.

## Physical Sides

The **physical sides** refer to the actual files which contain the game code, as 
distributed by Mojang. A physical side defines what program the user is actually running,
either through the Minecraft Launcher, through the command line, or some other method.

There are two physical sides:

1. The **physical client**, also known as the **Minecraft client**, is the program ran
by players of Minecraft. This is shipped with the libraries which are used to render the
graphics, play the audio, and receive inputs from the user.

1. The **physical server**, also known as the **dedicated server**, is a JAR file which
contains all the necessary dependencies to run a copy of the Minecraft server, to which
players (through their physical clients) can connect to and play on.

## Logical Sides

If physical sides refer to the files which contain the game code (or difference thereof),
the **logical sides** refer to the actual game code or logic being run.

As with the physical sides, there are lso two logical sides:

1. The **logical client** is the game code responsible for handling player inputs and
communicating with the server both to inform it of the inputs from the play and to be
informed about changes in the world.

1. The **logical server** is the game code responsible for maintaining the authoritative 
copy of the game world, running the ticking logic -- as in, actually advancing the time
and actions in-game -- and communicating with clients to know player inputs and to respond
appropriately.

## Logicals within Physicals

One of the key sub-concepts within the concept of sides is that _logical sides are present
within physical sides_. When you load either the Minecraft client or the dedicated server,
it will load one or both of the logical sides, depending on what the physical side is being
loaded.

The two key points are that:

1. The logical client is only present on the physical client.
2. The logical server is present on both the physical client _and_ physical server.

That may seem confusing at first, but you have to keep in mind the architecture of Minecraft
and how the logical sides relate to them.

When a player is playing a game, both in singleplayer or in multiplayer, there are at least
two parts: the client, by which the player is controlling their character and observing the
world, and the server, which does processing for what happens within the world -- including
the action taken by the player. These are what we refer to as _logical sides_.

In a multiplayer environment, this is easily understandable: the Minecraft client provides
the logical client, while the dedicated server provides the logical server.

However, in a singleplayer environment, who is hosting the server? In fact, _it is the same
Minecraft client!_ When the Minecraft client loads up a world, it creates a local server,
known as the **integrated server**, which provides the logical server. The Minecraft client
still provides the logical client, but it also manages the logical server for the world.

!!! note
    While it may be made for the purpose of a singleplayer world, Tte integrated server is 
    still a server in its own right, and can even allow other clients to connect to the 
    same world! 
    
    This is known as _publishing the server_ (the `/publish` command) or _opening the server
    to LAN_ (which is why the pause menu shows the `Open to LAN` button). In this situation,
    it just so happens that the Minecraft client creates and hosts the same server to which 
    it itself connects to.

## Classes across Sides

As of writing and current and past observations of the codebase, the Minecraft client classes
are known to be a superset of the dedicated server classes. This means that they both share a
large portion of code which is used for the logical server implementation, as well as common
objects between the logical server and the logical client.

Unsurprisingly, the majority of classes which are only present in the Minecraft client are
those under the `net.minecraft.client` subpackage, a majority of which relate to rendering
the world and in-game screens on the user's screen.

!!! warning
    One of the common mistakes of Minecraft modding, which plagues both beginners and 
    experienced modders alike, is accidentally referencing a client-only class from a place
    where it may be possible for the dedicated server to run into.

    In the Forge development environment, this manifests as the well-known error message of
    `Attempted to load class <class name> for invalid dist DEDICATED_SERVER`. In the 
    production or end-user environment, this manifests as an `ClassNotFoundException`.

    Mod developers should be alert when directly referencing a client-only class in common
    code (that is, reachable in both the Minecraft client and the dedicated server). It is
    also recommended practice to test mods while running the dedicated server, to catch these
    errors within the developer's environment before they cause a problem for end-users.

## Trivia

- Because the physical client contains all the server classes, one can possibly craft a
  command which runs the dedicated server using the Minecraft client's JAR. While 
  fascinating, it wouldn't be viable to run the dedicated server, as the command would
  have to reference the whole classpath required to run the client, as opposed to the 
  dedicated server which packs all its dependencies into the same JAR.
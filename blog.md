# STAGECRAFT

A narrative-focused, text-adventure, scripting language to be run with the STAGECRAFT DIRECTOR.

## Day 1

**No updates**

## Day 2

Woops, I started late :D

**2025-12-15 3:45PM EST**

Ideation of the language - For the past couple of months, I've been playing around with various generative AI models to try and create a AI short film, start to finish in AI. While doing that, when having an LLM generate a screenplay, it wasn't writing it how I, a barely-literate, non-actor, software developer, would write it.

When my buddy anounced he was starting this langjam gamejam, I immediately thought of a dozen things, but not the screenplay. I recently worked on an MUD game engine where LLMs powered the NPCs, which actually interacted with the game the exact same as a user, just with a predefined schedule that operated on a loop, or as close to it as possible.

Last night I told Austin that I was probably going to just do something boring and dumb, because I'd rather finish the jam with something dumb then never even start something awesome. So I knew I was going to do some sort of text-based adventure game. It wasn't until I was closing out of notepad windows this morning that I even remembered the screenplays, and then the idea hit me.

So starting now, I will blog this in "real-time". Updating the blog as I progress.

**2025-12-15 4:20PM EST**

Blaze it.

**2025-12-15 4:21PM EST**

Now I have a bit of a sample game I'm not sure if the syntax is consistent, it's just been a flow:

```stagecraft
PLAY "Imprisoned" VERSION 0.0.1

ON START:
    GO CellD24

PROP journal "Personal Journal":
    CAN TAKE, GIVE, READ, USE
    TAGS BOOKS

    ON TAKE:
        NARRATE "Your only real posession. Keep it close."
    
    ON USE:
        IF new_info = []:
            NARRATE "There is nothing to write down."
        ELSE:
            FOREACH info IN new_info:
                NARRATE info.narration
                PUSH info.journal ON player.journal
            SET new_info: []

    ON READ:
        IF player.journal = []:
            NARRATE "You haven't written anything down yet."
        ELSE:
            FOREACH entry IN player.journal:
                NARRATE entry

    ON GIVE TO character:
        NARRATE "Are you sure you want to give your journal to ${character}?"
        CHOOSE:
            "Yes":
                RETURN
            "No":
                CANCEL

PROP toilet "Your Toilet"
    CAN USE

    ON USE:
        IF player.bladder > 0:
            NARRATE "You relieve yourself."
            SET player.bladder: 0
        ELSE:
            NARRATE "You wiggle and shake, but nothing comes out."

        IF RAND(10) > 7:
            NARRATE "When zipping back up, you got caught in the fly."
            SAY "YEOWCH!"
            SET player.health: player.health - 5
    
SCENE cellD24 "Cell D:24"
    DESCRIPTION "A tiny room without a window. The only thing between you and the rest of the cell block are a few bars and a door."
    EXITS:
        WHEN cells.d24.open = true: cellBlockD
```

What I'm liking:

1. The capitalization - dumb, I know, but I liked that in a screenplay. 
2. Verbosity without being TOO verbose
3. Indention for blocks - Obviously other languages have this, and I'm usually not a fan, but I feel like it fits this application well.

What I'm NOT liking:

1. Colons - They have multiple uses depending on the context, and an overloaded operator in the language seems like it might be a bad idea.
2. NARRATE & SAY - I'm not currently sure if these will have enough of a different meaning than to combine into a single call like 
    - `SAY "narration" ["who"]` - this way you can leave off "who" is saying, and it will be a narration.
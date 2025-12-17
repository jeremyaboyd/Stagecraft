# STAGECRAFT - A live blog

Well I will be live blogging as I go. Posting my own question to myself, writing my answers down. Almost like I have a friend.

## Day 1

**No updates**

## Day 2

Woops, I started late :D

**3:45PM**

Ideation of the language - For the past couple of months, I've been playing around with various generative AI models to try and create a AI short film, start to finish in AI. While doing that, when having an LLM generate a screenplay, it wasn't writing it how I, a barely-literate, non-actor, software developer, would write it.

When my buddy anounced he was starting this langjam gamejam, I immediately thought of a dozen things, but not the screenplay. I recently worked on an MUD game engine where LLMs powered the NPCs, which actually interacted with the game the exact same as a user, just with a predefined schedule that operated on a loop, or as close to it as possible.

Last night I told Austin that I was probably going to just do something boring and dumb, because I'd rather finish the jam with something dumb then never even start something awesome. So I knew I was going to do some sort of text-based adventure game. It wasn't until I was closing out of notepad windows this morning that I even remembered the screenplays, and then the idea hit me.

So starting now, I will blog this in "real-time". Updating the blog as I progress.

**4:20PM**

Blaze it.

**4:21PM**

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

**4:45PM**

Time to think about the front end, and how the player will play the game. The simplest solution would be something like an HTML/CSS/JS front end.

Maybe I can make it appear like an old black and green CRT using some CSS... and by me... I mean Claude.

I think that COULD be cool, but also simple. I will start with that idea, and if there is a lot of time, maybe I can recreate it in Unity after.

**4:54PM** 

I'm getting ADHD sidetracked. I'm back on the language, and realized `ON START` makes no sense. It should be `ON NEW` as it is specific to starting a new game. But by doing that, I'd need to probably also add `ON LOAD` and `ON SAVE` but those feel less useful, so maybe I don't?

Either way, I should change the sample game to `ON NEW` and possibly narrate an introduction.

## Day 3

**3:18PM**

After spending 8 hours in front of a computer today, already, I've decided to spend another 4-5 hours working on this stupid idea. I'm definitely not having a bad day.

Things I want to get completed, and if I don't I will scream:

1. Finish the language specification
2. Have Claude write the GUI terminal looking thing.
3. Start building the interpreter: called "Stagecraft Director" - doesn't need to be completed, just started. I'll scream if I don't start it!

**3:25PM**

First prompt into Claude. Let's see if it works.

> Can you write a client-side HTML/CSS/JS console.
> 
> It should behave similar to a terminal emulator. Use a nice console-style monospace font, style it to look similar to an old-school black and green CRT screen.

**3:30PM**

We're cooked fam... First try and it is nearly perfect.

![First Try](./blog_assets/terminal-first-try.gif)

It already has the retro look with glow and scan lines. I didn't expect it to go that far!

Now that it did that so easily, I'm scared I will be the reason I don't finish today. I can't blame the stupid AI dude.

Existential dread coming in 3... 2... 1

**5:00PM**

Well, at least I can play drums better than Claude... I hope.

Time to figure out how to write an interpreter.

## Day 4

**7:45AM**

Well I didn't finish what I wanted yesterday because I didn't spend hours working, only about 30 minutes. And then I got distracted with Homeland (the television show - I love Claire Danes and I never saw it before). In fact, this is a terrible time to even start this stupid langjam.


# Semaphore: G1 recording script

AI use: Claude and ChatGPT helped me explore concepts and draft the initial documents. ChatGPT helped revise the deck and script, and Claude helped make the script easier to follow. I remain responsible for the design and presentation.

Target: under 5 minutes. 709 spoken words, about 4:60 at a relaxed 142 words per minute, or about 4:40 at a normal speaking pace. The same words are in the deck's speaker notes. Rehearse once and keep a steady pace.

## Slide 1: Semaphore (0:00–0:19)

Semaphore is an animated co-op game. You and an AI partner each run one side of a small rail junction, and you feel the satisfaction of getting a difficult handover right.

This is my Gate 1 concept, and I'm asking for approval to prototype it.

## Slide 2: The handover (0:19–1:00)

A handover is deciding who uses the one shared track: you send your train, or you let your partner's go first. Think of two cars at a one-lane bridge.

A beat is one turn of the game clock. Nothing moves while we decide, then every train moves one step.

Here, my train has five beats to spare, but my partner signals Urgent. I can't see its trains. Do I send mine, or let it go first?

I'm designing for co-op puzzle players. If it works, a player lets the partner go first without being told, and can explain why.

## Slide 3: Core loop (1:00–1:47)

We each pick one of four signals: request crossing, ready to receive, please wait, or urgent. Both signals appear together.

Then we each privately choose to send or hold a train, and set our switch for any train heading our way. The game moves everything one beat and shows the result.

Crossing takes two beats, and the receiver routes each train to the siding matching its colour and shape. If we both send at once, both wait a beat. Trains never crash, so the pressure comes from deadlines.

A shift has eight trains and up to twenty-four beats. On time scores three, late scores one, and a wrong siding loses two.

## Slide 4: Pillars and alternatives (1:47–2:09)

My pillars: cooperation has a visible cost, consequences are easy to read, and planning, not reflexes, sets the pace.

I considered a text-adventure race, but it was too close to my midterm, and a racing crew chief, which needed real-time work first. Semaphore gives me one animated coordination problem I can test.

## Slide 5: References and reuse (2:09–2:29)

I borrow limited communication from Hanabi, split information from Keep Talking and readable routes from Mini Metro, with my own graphics and no copied assets or code.

Overcooked-AI research found that agents trained with copies of themselves often struggle with people, so I test with people.

## Slide 6: Systems and knowledge boundary (2:29–2:54)

The server holds the full game, including both sides' trains and the schedule. The model sees only its own trains, the shared track, both signals and the last eight beats. It never sees my trains or my choice.

Every move is checked before it counts. If the model's reply fails, its train holds, and that's labelled. A late reply is ignored.

## Slide 7: Agent hypothesis (2:54–3:27)

The model picks its signal, which train to send or hold, and its switch. I expect it to adjust to recent exchanges in ways players understand.

The scripted baseline is solid: it sends its most urgent train, answers signals and takes turns. But its rules never change, so after a few shifts you learn them, and your signals stop mattering.

Playtesters try both, and I ask what they expected, what happened and what they changed next.

## Slide 8: Prototype scope (3:27–3:42)

The prototype has both partners, route previews and the comparison. Networking, a campaign and learning across games are out. If time runs short, I cut animation polish and sound first, never the comparison or the playtest.

## Slide 9: Budgets and uncertainty (3:42–4:06)

A shift needs forty-eight model calls, one for the signal and one for the move each beat. That's about three cents a shift.

In my midterm, this model averaged one and a half seconds per call. I'm aiming for ninety-five percent under three seconds, with a six-second timeout. The biggest unknowns are prompt size and deployed speed.

## Slide 10: Risk Profile (4:06–4:25)

Concept, audience, technology and team are Medium. Mechanics and timeline are High, with one shared cause: the shared track might not create interesting choices.

So I test that first, on paper and against simple turn-taking. If it fails, I shrink the prototype rather than add mechanics.

## Slide 11: Work through the next gate (4:25–4:40)

First, paper handovers and one deployed model call. Then the engine, the scripted partner and a rough playable version, then the comparison and an external playtest, before the G2 and G3 review on November 30.

## Slide 12: Decision requested (4:40–4:59)

I'm asking to build this prototype and test whether the model adds useful cooperation. If it's a Hold, I'd return with a clearer handover, proven schedules or a smaller plan.

I chose the cooperative direction; my agents suggested mechanics and the stack, which stay provisional.

## Recording setup

Use the slide show at 16:9 and record your voice with the slides visible. Keep the pointer away from text. On slide 2, pause briefly at the choice to yield. On slide 6, trace the filtered-view path, then the validation path. Leave the risk table visible while explaining the two Highs. Finish the decision sentence before stopping.

If a test run goes over 5 minutes, drop the last sentence of slide 9 and the second sentence of slide 1.

Before posting, watch the recording once, check the audio, confirm it is under 5 minutes and test viewer access. Add the recording URL to README and G1-POST.

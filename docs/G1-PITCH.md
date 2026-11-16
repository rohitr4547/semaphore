# Semaphore: G1 recording script

AI use: Claude and ChatGPT supported concept development and drafting. ChatGPT helped revise this deck and script. I remain responsible for the final design and presentation.

Target: **4 minutes 45 seconds**, including brief pauses. 691 spoken words, approximately 145 words per minute over the full recording. The same narration appears in the deck's speaker notes. Sources in notes are not spoken. Rehearse once and adjust pauses to stay within 3–5 minutes.

The deck is a concept presentation, not gameplay footage. Keep the concept/proposed labels visible. Use your own natural delivery. Avoid reading tables row by row.

## Slide 1: Semaphore (0:00–0:20)

Semaphore is an animated co-op game where you and an AI partner dispatch trains across one shared track, and you feel the satisfaction of getting a difficult handover right. This is my Gate 1 concept, and I'm asking for approval to prototype it.

## Slide 2: The handover (0:20–0:40)

Imagine my train has five beats of slack, but my partner signals Urgent. I can hold and let it through, without seeing its queue. I am designing for co-op puzzle players motivated by fellowship and mastery. If it works, a player lets the partner go first without being told, and can explain why.

## Slide 3: Core loop (0:40–1:05)

We each send one explained signal, then reveal both. Then we each privately choose to send or hold a train. The engine advances one beat and shows what happened. We read the result and choose the next signal. If we both send at once, both wait a beat. Trains never crash, so the pressure comes from delivery deadlines.

## Slide 4: Pillars and alternatives (1:05–1:30)

My pillars are meaningful cooperation, readable consequences and time to plan. I considered a text-adventure race, but wanted to move beyond my midterm. A racing crew chief needed real-time work before I could test whether it was fun. I selected Semaphore because one shared crossing gives me an animated, manageable way to study cooperation.

## Slide 5: References and reuse (1:30–1:55)

Hanabi informs restricted communication. Keep Talking informs split information, and Mini Metro informs readable routes. I change these into simultaneous dispatch decisions with original graphics. No commercial assets or code are copied. Overcooked-AI research found that agents trained with themselves often struggle with people, so I test with people. References and reuse positions are linked in the GDD.

## Slide 6: Systems and knowledge boundary (1:55–2:25)

The server owns both queues and the future schedule. It builds a separate view for each side. The model receives its own trains, the public crossing, revealed signals and eight rounds of filtered history. It never receives my queue or hidden commitment. Both sides submit through validation. If the model's reply fails, its train holds, and that's labelled. A late reply is ignored.

## Slide 7: Agent hypothesis (2:25–2:55)

The model chooses signals, departures and receiving routes within that boundary. I expect it to adjust to recent exchanges in ways players understand. The baseline is solid: it sends its most urgent train, answers signals and takes turns. But its rules never change, so after a few shifts you learn them and your signals stop mattering. Players try both in alternating order, and I ask what they expected, what happened and what they changed next.

## Slide 8: Prototype scope (2:55–3:15)

The prototype has one junction, four signals, eight trains and up to twenty-four beats. Both partners, route preview and the comparison are included. Networking, a campaign and cross-game learning are excluded. I cut smooth animation and optional sound first. The boundaries and external playtest stay. The numbers remain provisional until the feasibility check.

## Slide 9: Budgets and uncertainty (3:15–3:40)

At forty-eight calls, I estimate three cents per shift and nine cents per three-shift session, using the dated prices and token assumptions shown here. In my midterm, the same model averaged one and a half seconds per call. I target ninety-five percent of calls under three seconds, with a six-second timeout. Two timeouts in one round add twelve seconds. The biggest unknowns are prompt size and deployed latency.

## Slide 10: Risk Profile (3:40–4:05)

I rate concept, audience, technology and team Medium. Mechanics and timeline are High. They share the risk that the crossing may not create interesting choices. I'll test that on paper and against simple turn-taking before polishing the game. Timeline also depends on deployment and my solo capacity. The fallback is a smaller prototype, rather than another new mechanic.

## Slide 11: Work through the next gate (4:05–4:25)

First I test the handover and deploy one isolated model call. Then I build the engine, scripted partner and a rough playable, then the held-out comparison and an external playtest. I own the decisions. Agents support implementation, design and independent review. The combined G2 and G3 review is on November 30.

## Slide 12: Decision requested (4:25–4:45)

I am asking to build this bounded prototype and test whether the model adds useful cooperation. A Hold means returning with a clearer handover, feasible schedules or a smaller deployment plan. I selected the cooperative direction. Agents recommended mechanics and stack options, which remain provisional. If the scripted partner works as well, I will report that result.

## Recording setup

Use the slide show at 16:9 and record your voice with the slides visible. Keep the pointer away from text. On slide 2, pause briefly at the choice to yield. On slide 6, trace the filtered-view path, then the validation path. Leave the risk table visible while explaining the two Highs. Finish the decision sentence before stopping.

Before posting, watch the exported recording, check audio and text, confirm its actual duration and test viewer access. Add the real recording URL to README and G1-POST. Do not describe the deck as a recorded submission until that is done.

# Ministrations: *MINI*mal, *ST*ee*RA*ble, instruc*TIONS*

Disclaimer: Work in Progress and Experimental. I'm new to local LLM so some of this information may be false or misleading!

Alt title: You may be prompting wrong, hear me out.

## Preamble

I run small 7B, 8B quantized imat models locally (on a 6GB Mobile 3060 - huge skill issue).

I have come up with a context prompt, tips and sampler settings which I want to share.

I looked around for system prompts and character cards, but couldn't escape the dull drudgery of the "ministration" effect. Nor could I find anything 'emergent' or 'creative' - everything felt boxed in and repetitive. After some testing, and writing my own cards from scratch, I came up with a system for creating characters with 'emergent' stories based on a context prompt.

This may not apply for people who have specific lore and huge context to play with, but, who knows, maybe it will? I don't have the compute power to test.

## The context prompt

```
Embody and reply as {{char}}.
Action, exposition and description are enclosed by *.
Dialogue is enclosed by ".
Use '{{char}}'s vocabulary' in dialogue where appropriate.
Let's think step by step.
```

Rules and standards are good for AI. It follows them pretty well with some reinforcement. Here, I'm telling it two things:

`"This is what a character says"`
`*This is what a character does*`

### Let's think step by step:

> Large language models (LLMs) can perform complex reasoning by generating intermediate reasoning steps. Providing these steps for prompting demonstrations is called chain-of-thought (CoT) prompting. CoT prompting has two major paradigms. One leverages a simple prompt like “Let’s think step by step” to facilitate step-by-step thinking before answering a question.

[Arxiv.org](https://arxiv.org/pdf/2210.03493)

"Chain of thought" prompting improves outputs. Normally it's reserved for deduction, _however_ owing to the fact that LLMs are just a series of gradient descent processes (next logical choice), We can use this to improve the model's responses.

## The Output

**Experiments carried out with L3-8B-Stheno-v3.2-Q4_K_S-imat.gguf** (Reminder: 6GB card, don't be mean).

Using the above system context prompt, and the following character card, only filling out the description field:

```
[Demo's vocabulary: "gruyere", "parmeggiano", "cheddar", "roquefort", "taleggio"; Demo's dialogue or dialog: Broken in fragments: "Roquefort love. Need eat cheese."]
```

Here's the output with neutralised samplers: `Temp: 5`, `Seed: 1`, `Min-P: 0` (Neutralised):

```
25 الدول 数据 navr':

NECTION[$"")), önlemОшибкаンテ Life birdug MantSymbolsReservation biome клі Verify(geometry洞 "");

_nullऊirlinesочные Love MemoANG kterou phy Nice>] Recogn Door♀♀istringstream唐elocityIdentification=UTF tariffs γ ListedPressInstantiationExceptionWide-loadedContainerscura zah Zach optical Km-stackッドquisitioncourse.capture torn Twin Andrew hậuİ sec/securityfeatureвся Cruderek-heavy imshow CONTACT fake"")aching age 계획。"

[...and it just goes on from here]
```

Not exactly Pulitzer Prize worthy.

### Min-P

Min-P is amazing, I neutralised all other sampler settings and _just_ used `Min-P: 0.01` at `Temp: 5`, `Seed: 1`

There's no variation in this test based on seed.

Here's the result:

```
*""Ack! Humans, need food."" Demo eye human suspiciously. Then sees glimpse of crackers behind shoulder and sniffs harder."" *Mmmph… No roquefort, no parmeggiano, but may be cheddar or… Gruyère.""* Demo moves in close, tail swishing with anticipation."" ""Roquefort, you?"" Dema ask pleadingly.* ""Need. Cheese." She lets out a whimper of longing, body shuddering with a cheesy yearning.
```

I'd say that's pretty shitty but let me correct it:

```
"Ack! Human, need food." *Demo eyed the human suspiciously. Then sees glimpse of crackers their back and sniffs harder.*

"Mmmph... No roquefort, no parmeggiano, but may be cheddar or... Gruyère." *Demo moves in close, knife swishing with anticipation.*

"Roquefort, have you?" *Demo asks pleadingly.* "Need. Cheese."

*It lets out a whimper of longing, body shuddering with a cheesy yearning.*
```

It's still cringe, and I fucked up the tenses, but it's for demo purposes. If I hit "Generate" here's what comes out next:

```
""Glances frantically around human space again Where cheddar, gruyere?! Maybe other strong… umami on this accursed vessel?! Human know?!"" The imp stomps and waves knife accusingly, craving its favored fromage.
```

If I make the `Min-P` (still at `Temp: 5`) more restrictive, and set `Min-P:0.02`, here's the result of the regen:

```
*As the human pulls out the roquefort and gruyère, Demo's eyes nearly glaze over with unadulterated joy and excitement.* "Oooh roquefort, gruyère! Love!"

*With a surprisingly gentle, yet firm grasp, Demo wraps its knife hand around the prized blue cheese, snatching it into its blade-slit palm.*

"Mmph... need roquefort love..." *Demo croons as it starts hacking the strong blue cheese onto the waiting crackers with swift, expert strokes. A few chunks miss the plate and tumble to the ground, but Demo pays them no mind. The ritual of partaking in rich, savory, crumbly cheese is all that matters in this moment.*

*As soon as a cracker is topped, Demo eagerly places a triangular chunk of cheddar next to it, just in case.* "May be cheddar good too..." *the being notes absently.*

"Roquefort make me happy human, thank you," *Demo rumbles with gratitude, its rasping voice vibrating against the table as it brings a heaping slice of roquefort to its lips, ready to savor it.* "Need eat cheese..."
```

The model has perfectly formatted its response in accordance to its instruction in the system prompt and the *chat history*. Actions and description are enclosed within asterisks, dialogue in quotation-marks. Some of it is a little bit bonkers: "knife hand", "blade-slit palm".

But the things it got right: ✅ vocabulary, ✅ pronoun, ✅ speech style, ✅ unhinged,

The things it got wrong: ❌ spoke for user, ❌ imagined things happening that didn't.

Let's try at `Min-P: 0.95` (aka, repeat verbatim what the prompt says, accept only the highest weighted tokens):

```
"Roquefort love. Need eat cheese." *Demo's eyes gleam with a desperate intensity as it fixates on the human's hands, hoping to find the prized blue cheese.*

"Please, human, share. Demo no have roquefort. Only... gruyere, cheddar, taleggio." *It whines, a pitiful sound, as it leans in closer, the tip of its knife almost touching the human's fingers.*

"Roquefort make Demo strong. Demo need strong to... to..." *Demo's voice trails off, its thoughts consumed by the craving for the pungent, crumbly delight.*
```

It starts getting a little bit "meh" right? Like... a certain "ministrations" feel to the writing. The model sticks to the information that it knows; which, at this stage, is not very much.

### Initial conclusion

Be more relaxed with what your expectations are, especially when using 8B quants.

### Ministrations

Let me add "ministrations" to the vocabulary:

```
"Roquefort love. Need eat cheese." *Demo's eyes gleam with a desperate intensity as it fixates on the human's cracker stash.*

"Please, human, share. Demo need ministrations of dairy." *It whines, a plaintive note in its voice as it reaches out a clawed hand, palm up in supplication.*

"Crackers not enough. Need real cheese. Roquefort, taleggio, any kind, just please, give Demo." *The creature's words tumble out in a rush, punctuated by its agitated swaying.*
```

There it is. Ministrations verbatim The point is not "avoid adding 'ministrations' to your character card, the point is that if any tokens weight comes *near* 'ministrations' you're going to see it come up in a response.

## Write less, correct more

Write less. Seriously, write less in character cards, write less descriptions, less personality traits, less _everything_. Anything you _do_ write will weight tokens even if it's an instruction or description.

In real writing, no authors outline specific personality traits verbatim. The reader _learns_ of a characters traits through _exposition_. But sometimes AI needs steering:

> {{char}} expresses herself with her tail.

IF you want this to be successful you have to provide an example.

> [{{char}} expresses herself with her tail. Example: *thumps her short tail against the wall*]

The first time this occurs, it may be verbatim, as the chat progresses it'll be more creative: because the weights in the tokens are now more _diverse_. Especially if you **correct** it manually. Don't want her to wrap her tail around her leg? Don't let the model do it. Is it a bunny tail? Make it bob, wiggle, flick. Writing "Their tail is short and doesn't sway" once in a character card does not outweigh the 10 times you let it slide in chat responses. 

The text `Example: ` will always weight the model.

## Dialog is everything.

Every single token that goes through a prompt adds weight to other, related, tokens. Each time you prompt in SillyTavern, the instruction is sent, the character card, the lore books, authors notes etc.

Flood a character card with anything _vaguely related_ to the prose near 'ministrations' and you'll get 'ministrations'

If you want a character to stay in character - learn to write _as_ that character and, where possible, have the AI write _as_ that character too e.g. Summarise Extension:

```
[Pause your roleplay. Summarize, in {{char}}'s own words, using {{char}}'s vocabulary, the most important facts and events that have happened from  in the third person. If a summary already exists in your memory, use that as a base and expand with new facts. Limit the summary to {{words}} words or less. Your response should include nothing but the summary.]
```

System prompts like this should follow stable-diffusion rules.

Whatever is _first_ are considered _first_. That means your order is important.

Here's what our summarize prompt outputted, using just an initial message, and two replies:

Demo's summary: "Human, Demo need cheese. No crackers enough. Must have real cheese, any kind, but roquefort best. Demo ask human to share, to give ministrations of dairy. Human have crackers, but Demo want cheese. Demo very hungry, very desperate for cheese. Please, human, give Demo cheese, or Demo will be very sad and unhappy."

Keeping it within the "voice" of {{char}} will help maintain writing style as the model ingests summaries.

## Tips and stuff

### Shared camaraderie

You know those weird responses you get from any model, where it just tries to shoehorn some shitty explanation to the end of their esponse, e.g.:
"The laughter of the character represents their jovial nature."

That's what it's trying to do to your RP: "Shared cameraderie." Sound familiar?

The model performs gradient descent on every token, picking likely candidates to follow the last token it ingests. If your sampler isn't spicy (temperature) enough: you get boring prose, because of all of the _boring prose you wrote in your character cards just to be descriptive_.

You ever notice how if you don't write much in a new chat, the model will just pick at things from the User Persona to bring up even if they're not revealed to the {{user}}? The model is trying to find things to respond with.

Just delete it, make the model respond how you want it to - especially early on. It's searching for something to build upon. If the only thing in its context are character and persona cards it'll only use those.

### Don't just swipe

**Don't just swipe**. Find a swipe you like, correct any mistakes and then continue.

### Context as a corpus of data

The tokens in your context _weight new responses toward what is in the existing context._

The context includes as much of the conversation that can fit. This means the model is going to forget stuff. If there's something important the model should know: **write it in author notes**, make sure it always sends them, keep it brief.

An important event? An important object that a character possesses? You don't necessarily need a lore book. Just slap it in there. Heck, add it as a thought from {{char}} in their own words. Keep it brief.. When it comes up in the chat: correct mistakes, add more detail.

### Learn to write

I'm not a good writer and this is a dumb one, I know, but with AI models if your input sucks, your output sucks, too. If you have 8196 context where you've allowed the model to get away with writing total drivel: It's gonna suck, forever, there's no going back. You may as well hit the new chat button if you forgot to delete even one instance of 'sending chills down your spine'.

Here are some tips for _economy_:

Colons are clarifiers. The model recognises colons as part of writing. Use them. An example:

> The room was opulently decorated: plush furnishings, gold cornicing, sash windows and an elegant shortpile carpet.

Short lists like this weight tokens, they're amazing and don't feel stilted.

Only do this if you want the bot to respond to the input. Otherwise, leave it out. It just needs to know you're in an interior.

Semicolons are great. You can break up a thought into multiple ideas (independent clauses):

> {{user}} had made pudding; it was already forming one of those weird, gross skins that Puddings tend to after cooling.

And avoid comma hell:

> {{user}} thought that {{char}} was putting on airs, gassing him up and being overly complimentary; but {{char}}'s face was earnest.

If you're ever uncertain about who is performing an action so will the model e.g. two 'he/him' characters:

> {{user}} unsheathed his knife and pointed the tip toward {{char}}. ~~He~~ {{char}} became anxious.

### The character card

Using the [MinimALIstic (Ali:Chat Lite)](https://rentry.co/kingbri-chara-guide) P-list format write some vocabulary in the description

```
[Demo's vocabulary: "gruyere", "parmeggiano", "cheddar", "roquefort", "taleggio"]
```

Tada. Your bot only talks about cheese, references cheese, can't get enough of the stuff. It has no basis for speech style, so give it one single example.

```
[Demo's dialogue or dialog: Broken in fragments: "Roquefort love. Need eat cheese."]
```

###

Your bot is basically braindead. Consumed by a mania that revolves around cheese and cheese alone. With all of the creative freedom to explore the weights associated with these tokens.

Personality traits are still important, but they can confuse the model. Related traits are best:
[Demo's personality: gruff, angry, unhelpful]

As chats move on though; If you've got 8196 context of Demo being cute, friendly and helpful - What's going to win out? The context, not the card.

## Conclusion

If you've made it this far, kudos, this is boring as fuck.

The main thing to take away is that if you want diverse RP, let the temp get spicy, correct mistakes along the way and use a simple context prompt and character format that won't infect your prose with boring descriptors.

---
date: 2026-03-29T08:00:58-04:00
# description: ""
# image: ""
lastmod: 2026-03-28
showTableOfContents: false
# tags: ["",]
title: "Changing Perspective on AI"
type: "post"
---

I was late to hop on the AI bandwagon and at first I was generally uninterested in AI, I didn't dislike or have a position **against** it.
I mainly saw it as a gimmick and in terms of coding, my perspective was:

> Coding with AI is bad, people should code themselves.

But now my view is, good code should be accepted whether it comes from AI or a person, emphasis on **good code**.

## How I Use AI

In my daily life I use AI a fair amount, probably not as much as the average person but I definitely use it from time to time.
Some of my common AI uses are

- Researching topics
- Proof reading things I write
- Occasionally fleshing out ideas

When there's some weird and random topic bringing me down a rabbithole, if I know absolutely nothing about it, it does save some time to have AI give me a high-level overview.
Afterwards I can then find information relevant to what I'm researching and it's generally more effective for me to go in knowing some things. I can also quickly see any parts where AI might've been wrong.

I also use AI to proofread things I write for this blog.
It's important to me that these posts are my own words, but I am a terrible writer so I need help.
After I write a blog post, I have an LLM check for redundancy, run-on sentences and grammar mistakes.
However, I instruct it to just give me feedback and not write anything, these are all my words.

## Coding Gotcha's and AI

At the time of me writing this, AI tends to really only be as good as the user.
However, just because [AI helps people write 3000+ line PRs](https://www.reddit.com/r/github/comments/1rqyq0y/vibecoders_sending_me_hate_for_rejecting_their/), it doesn't mean that they should.
I still think everyone's life is better in the long run with shorter, single-purpose PRs.
When I see long PRs, I don't have fun, call me lazy.
Here are some things I've seen in AI generated PRs that I was able to catch, but the AI seemingly couldn't.

Imagine we have a function called `get_boxes` whose job is to return a list of boxes. We don't care how it happens, we just know we get some boxes and it has an `owner` field which is a string of the owner's name.

```python
boxes: List[Box] = get_boxes() 

# Now we check for boxes that I own
my_boxes = [True for box in boxes if box.owner == "Justin"]
if all(my_boxes):
	print("I own some boxes")
else:
	print("I do not own boxes")
```

In the code segment above, the `all(my_boxes)` will **always** evaluate as `True`.
This is a problem, because if I do not own any boxes, I want certain code to execute but it never will.
This happens because `True` values are being appended every time there is a box owned by me and then if there are **no boxes** owned by me, the result of `my_boxes` is just an empty list (`[]`) which when passed into `all` will still evaluate to `True`.

If you don't believe me, you can try it yourself:

```python
>>> all([])
True
>>>
```

Here is another example of AI seemingly copying not-so-great code that I thought it should have caught:

```python
user_id = 42

if user_id is 42:
    print("The number is 42")
else:
    print("The number is not 42")
```

In the above example, the code **technically** works, but it is not doing what the author thinks it does.
The reason it technically works, is because of the way Python handles literal values and memory addresses.
The variable `user_id` gets the same memory addresses as wherever the literal `42` is stored in memory because Python is clever like that.
Because of this, the identity comparison does what the author expected value comparison to do.
The author then saw working code and PR'd it.

## Takeaway

I guess the main message I wanted to send was that AI generated code is not in itself a bad thing.
However, I do think that when writing AI generated code, it kind of falls on the coder to become a PR reviewer for their AI.
Also fact check your information!
Whether you get it with AI or off the internet.

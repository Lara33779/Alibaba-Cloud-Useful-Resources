**Go visit: https://run.claw.cloud/?link=8V2R5HXK9ZLM**

## Introduction
After reading the article “Last mile delivery. Take your cursor code to production for free in just a few clicks.” I got really inspired. My company even reimbursed my Cursor membership so I could try out the latest AI tools. Since I’m not a developer, I never really thought I’d find a use for it. Besides, I’m pretty picky—for instance, I absolutely hate installing too many development components on my computer. As someone who is frequently on the road, I worry that loading up on these tools will hog memory (anyone understand the pain of a stripped-down MBP? 🤪) and drain battery (one charge on a work laptop barely lasts half a day 😣).

Then I discovered ClawCloud Run’s Devbox product (side note: who in their right mind comes up with a product name that long?) and thought it was pretty cool. It automatically connects with Cursor, and since the environments run inside Devbox’s virtual space, I no longer have to worry about cluttering my main machine. Sure, there are other ways to achieve this kind of setup, but Devbox is incredibly convenient.

## What’s the Plan?
Anyone who has ever goofed off at work knows the scenario: a colleague sends you a link—only to reveal a video or a website loaded with explicit images—and then you have to open it on your 27-inch external monitor. Awkward!

![image](https://github.com/user-attachments/assets/642c5d60-88be-4c63-ada5-e0d147217b5b)

So, I came up with a bright idea 💡—a link checker. Whenever a colleague (especially one known for sending NSFW links) drops a suspicious URL in my inbox, I can run it through this checker first 🧐.

## Let’s Get to Work

**1. Create a Devbox**

In my limited experience, Python seems ideal for AI-related tasks, so I chose Python as the runtime. Everything else is left at its default.

![image](https://github.com/user-attachments/assets/7b2f6fb4-2521-4598-bf10-2a14a11e82f8)

**2. Launch with Cursor**

Next, simply use the “Action” option to open the Devbox with Cursor.

![image](https://github.com/user-attachments/assets/3f7e5042-1a0c-4ff3-ad95-9d5384cae3e9)

**3. Connect Cursor and Define the Prompt** At this point, Cursor automatically connects to the ClawCloud Run Devbox. Now I needed to come up with the prompt for my NSFW detection tool. Of course, I don’t expect Cursor’s model to handcraft a full explicit content detection system from scratch, so I opted to have it use AlibabaCloud’s QWen-VL API—which comes with free multimodal credits—for analysis:

**Help me build a NSFW detection tool.** The tool should provide an HTML interface where I can enter a website URL. It will automatically detect whether the website (including its text, images, and videos) contains any content that is not appropriate for viewing in a workplace environment.

Content detection should utilize AlibabaCloud’s QWen-VL model. The SDK endpoint for making calls is: 

```html
https://dashscope.aliyuncs.com/compatible-mode/v1
```

The HTTP endpoint is: 

```html
POST https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions
```


My API Key is: 

```html
sk-ddf41f99f814435c
```

**4. Select Agent Mode and Model**
I handed the prompt off to Cursor. Based on my experience, selecting the Agent mode automatically figures out the implementation details. I chose the Claude-3.7-sonnet model since reviews online say it produces the best code.

![image](https://github.com/user-attachments/assets/594e243f-0cb9-4750-9f1e-2850752fdae8)


Click [here](https://blog.run.claw.cloud/177/) to read all.

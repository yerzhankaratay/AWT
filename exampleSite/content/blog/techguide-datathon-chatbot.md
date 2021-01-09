---
title: "datathon.kz or How I Got Introduced to Building Chatbots"
date: 2021-01-06T12:58:00+06:00
draft: false

# post thumb
image: "../../images/post/202101-course-datathon-chatbot/techguide.png"

# meta description
description: ""

# taxonomies
categories:
  - "Courses"
tags:
  - "Project"
  - "Chatbot"
  - "Certificate"
  - "hackathon"

# post type
type: "featured"
---
![datathon.kz's landing page](/images/post/202101-course-datathon-chatbot/datathon-landing#center)
I took a 3 week course called datathon.kz at [Chatbot University](https://chatbot.university/), it is a platform made by Singapore's [Terra AI](http://terra-ai.sg/about.html), and an opportunity provided by its partnership with [BCPD AIFC](https://www.facebook.com/aifcbureau/posts/1291434797898151). When the course started, we had all the learning materials and access to the [autocaffe](https://autocaffe.io) platform where the chatbots get rendered.

In the first session we were invited to a Telegram group to create and join teams, then to study together. I was in doubt whether I'll be joined by a person who invited me to take it just as the person, so I was reluctant to join forces with anyone else. That person didn't participate and I remained the only one man army but it was fine as it turned out I had my own (and quite unpopular) agenda.

The first day an idea popped up in my mind which was related to an [old blog post](https://yerkar.com/post/190840063886/видеоигры-экономия-энергопотребления) that in its turn was based on a study called [Greening the Beast](https://sites.google.com/site/greeningthebeast/market-survey), so I made my first version of Tech Guide, a chatbot that provides computer energy efficiency tips. We then submitted our chatbots for the next live streaming session and watched their evaluation.

My first version was poorly designed for a rule-based chatbot and wasn't clear to the course creators. They were testing the bot in a live session and asking it to give them "tip" while the bot was programmed to expect "a tip" at least as it was *taught* to be formal and it expected requests like "give me a tip", "I need advice" and their multiple alterations. The session made me understand that it's better to focus on a small problem than squaring the circle and attempting to make a chatbot that will answer everything.

![chatbot nothing really works](/images/post/202101-course-datathon-chatbot/chatbot-not-working.png#center)

I also remembered that human beings are lazy (as long as they think they are, let's say) and we might not put an effort to chat with a bot in formal English, well, now that I know how they work. So this was an eye opening session for me and I started looking at it from a user's perspective.

![our lazy nature](https://media.giphy.com/media/azUBCBFSCyNgc/giphy.gif?raw=true#center)

I was also told that "no one thinks about energy efficiency" during the livestream, no wonder, a few people think of optimizing things but I believe that everyone should, it should pay off in smaller utility bills, and a well optimized PC will also be quiet and work smoothly, by pursuing the goal one may also reduce his or her environmental footprint. I believe the final version managed to convince the people who tried it out that it's an important topic, although the interest towards it will grow if prices for electricity hike up.

![do not set your PC on fire](https://media.giphy.com/media/13HgwGsXF0aiGY/giphy.gif?raw=true#center)

I gathered materials to split them in topics, re-designed the whole bot to show guiding menus when a user opens the page, left the keywords without the phrases, highlighted commands, and added **everything for the user to know how to interact with the bot straight away** and make it easier and voilà, here we are:

{{< youtube bOQTrnSjlbI >}}

Feel free to try this final version out at https://app.smojo.org/kzbot021/bot.😃

I don't know if I am going devote my time to developing chatbots, however the fact that I started seeing it differently was the most valuable lesson to me and I liked the course.

During the course some people asked whether we could implement the chatbots within Telegram or WhatsApp, or if we could connect it to a database for it to give the appropriate answers but I suppose it couldn't really be covered within the span of 3 weeks when were all new to making them. If truth be told, I don't know why they named it datathon cause I expected it to be very different like others but a journey of a thousand miles begins with a single step.

We received certificates the 30th of December, 2020, hence I publish it today.

{{< embed-pdf url="/images/post/202101-course-datathon-chatbot/AI4IMPACT-CHATBOT-DATATHON-KAZAKHSTAN-2020-CERTIFICATE OF-COMPLETION-YERZHAN-KARATAY.pdf" >}}

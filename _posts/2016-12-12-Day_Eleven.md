---
layout: post
title: 8th Light apprenticeship - Day Eleven
categories: 8thLight apprenticeship
---

### Pills
- I had some trouble assessing how to tackle a problem efficiently in an area where 
I was not knowledgeable enough.
- I am going to try to set up an apprentices' feedback exchange scheme. 
- Looking forward to my IPM (Iteration Planning Meeting) tomorrow!

This morning I decided to add a delay to my computer player in the tictactoe 
program. This sounds pretty simple, and it was: one line of code to put the process
to sleep for 1s before returning a move. Nice. However, as I was about to commit
the updated version, I realised that the tests were taking too long, as the computer
player would sleep regardless of the environment it was running in.
This should not be a problem - I thought -: I will just add a guard that checks the 
environment and does not delay execution when the tests are running. 
About an hour later, this is what I have learned: it is a good idea for your code
to be 'environment-agnostic' and simply reach out to check on environment variables
rather than checking the environment and adapting to it. In my example, rather 
than deciding whether to delay execution or not based on the environment, I added
an environment variable, called `:cpu_sleep` that is set to false in the test
environment and true otherwise. This way, the code does not have to know which 
environment it is in. Less knowledge for the same outcome is generally a good thing.
Now, this was valuable learning, so I do not think my time was wasted, but I do 
think I should get faster at reaching an understanding of the problem and solution.
There are two blockers here: one is experience (I have not had much experience 
dealing with different environments), which I am not too worried about, as 
experience takes time.
The second blocker is my process in tackling the problem. I noticed as I was 
looking online for a solution, that I was torn between finding a 'quick fix' and gaining
a much deeper understanding of the problem. I think the key here is to first 
understand the problem I am trying to solve generally and then decide whether to 
find a quick a-la-stackoverflow solution or a deeper understanding of the topic.
The problem this morning is that I thought I knew the problem I wanted to solve 
in general, but it took me a while to realise that that was not the case. So the 
biggest takeaway here is never to take my understanding of a problem for granted.

Today we had a retro for all apprentices. Amongst other things, we discussed our
common thirst for feedback on our work. I proposed we would set up some kind of 
feedback-swap system between apprentices: each apprentice can review someone else's
code and give feedback on daily (or every two days) basis. This could work out 
nicely if we get in the habit and keep doing it. It should not take much of 
anyone's time, but the more we discuss code with others, the more we learn. I will
try and think of a format that works for everyone.

Finally, I have my Iteration Progress Meeting tomorrow, which I am looking forward
to. It feels like ages since my last one (one week ago) and I cannot wait to hear
my mentors' feedback and what I will be working on next.

Ciao :)

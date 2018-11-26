+++
title = "Don't be unique"
date = 2018-11-26T10:59:53+01:00
draft = false

authors = []

tags = ["interview", "programming", "style"]
categories = []

[header]
image = ""
caption = ""
preview = true

+++

Today was a funny and interesting day. As part of my job, I'm reviewing applicant's test and decide if he or she should proceed to the next step. Usually I see similar solutions or mistakes, but today was different. I received a test, opened in my IDE and started to scratch my head about it. It was a java code but with weird variables (String my_string, probably was spending couple of hours with python before), mixed spaces and tabs. Unnecessary comments all over place. I was taking notes as usual and moved on. The test itself has two parts. The first is kinda easy and straightforward, it aims to see the candidate's knowledge about normal programming tasks and challenges. See how the code will be structured and organized, if there is some programming pattern used or not, etc. So I was not satisfied with first part, did not even really work, but I had an overall picture about our hero and I've continued to review the second part. But suddenly I was Dorothy when she enters the world of Oz. I was surprised and shocked at the same time.

The code was almost perfect, what is happening? I ran all the unit tests, I've added even a few more new, tried to come up even with weirdest use-cases, but of course, it held. I could not argue about the second part, I was just switching between the classes of the first and second part. After a few minutes I was questioning everything. Is it possible that this person somehow rushed through the first part and spent more time on the second? Did he just refactor at the end, but did not have time to clean up the beginning of the assignment? Or maybe he just became better as he coded? Seems absurd and I didn't believe it. As I was thinking and opening the files in the project it felt there were more people working on it than just one. Could it be that this guy just asked a friend to help him with the second part and he just added those together? Yes, that's gotta be it! But have can I prove this? Should this guy be moved to the next stage of process and try to talk about code and programming in person see if he wrote both parts?

Probably I could, but felt just waste of time. Still I was unsure what to do, still wondering what is happening and I had another idea. What if there is no other person, but he just "copied" from somewhere? So I was looking at the code, see if I can find any special part of it and I found a typo in it. He added a comment, but typed a word wrongly, so I thought I can search for that. The search on google did not return anything, so I went to github and started to search for the magic misspelled words and then again I became Dorothy once more. I found the exact copy of his code for the second part. And then everything made sense. He could not find the first part, so he had to do something there and implement without much help, he had to add unit tests, but again the code quality was just poor. Of course if he would have spent I little bit more time to refactor the second part, maybe he could have tricked me, but I assume you can guess by now that he did not become my colleague at the company.

Of course my mistake was to give an assignment that is on github, but we tend to tell people to upload the result there, I don't really like opening zips. But it's not the easiest to find it there either (as I tried also). I was thinking to change the test, but then I realized it was great this time also. Probably we can catch and separate people who's just wants to copy-paste solution from others who think on the solution also.

After this I was thinking generally about different code styles, managing project and I realized something. We tend to have a different styles, we write code as we do, maybe I prefer short variable names, small classes and just a few comments around my code, but what if I have a colleague who really likes to type and do the opposite? Long variable names, detailed comments? Who is wrong or right? We're not just a bunch of people who write code separately and just put it together. We're supposed to be a team and collaborate together. There should be no code which is "mine" or "yours". So every time you open a class or method and you can "guess" who wrote that code then probably you failed unfortunately.

The moral of the story is to create a style for your projects. The easiest is just to follow a normal coding convention for that language and adopt it. But of course, it won't automatically solve all your problems, there is no silver bullet in case of a style (using static code analysis tools and code checks can help you). But try to follow as it's in the project, see how it looks and feels. As longs as you can see your footprint in the code then you are not done and the code is ready to be part of the project. Of course this is the same for others' code. Keep mentioning to your teammates about the code style during code reviews or refactor existing code to mach the common coding style if it's already part of the project, but does not follow the style. Adopting a common style will help you and other people to know what are "rules" around the project and guides you shipping code with high quality.

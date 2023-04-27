---
id: developer-interviews
title: Developer Interviews
desc: ''
updated: 1682582317078
created: 1682578125311
---
Developer interviews are a tough thing to do well. You aren't trying to out-smart the candidate, and you don't "win" if you create questions that nobody solves. You need to balance the filter such that you bring on people who you want to work with, and who are competent enough to do the work. 

# What I Look For
For non-tech-specific stuff, the biggest thing I look for is curiosity. Curiosity drives people. A curious developer will dig into docs, dig into source code, and more. The ones who will use a debugger to step into external library code to figure out the issues. The ones who will learn how ICMP fragmentation requests can block network traffic across datacenters. The really great ones, in my experience, just go a couple levels deeper than their coworkers.

Next, I look for how they fit into their individual teams and their scope/responsibility: 
- Were they just taking stories from the [[backlog]] that other people wrote? Were they writing the stories? 
- What portions of the project did they specifically take ownership of?
- Who designed the system? How did this person work with them?
- How did the technology stack get chosen, and how did it involve? 
- What were their specific contributions? Outside of the team goals or the product itself, ask for an example of a feature they had the most ownership over, and what it took to drive it to completion

Finally, I have a "No Assholes" policy. The days of putting up with rude and abusive people because of their technical competencies are largely gone. You're going to spend a LOT of time with this person. Why make yourself miserable? Some deal breakers: 

- Yelling or threatening team members
- Undermining team members
- Publicly devaluing team members or contributions
- Refusing to do tasks they see as "below" them

These people will drive away your best developers, who likely get headhunted on a regular basis. 

I want folks who are humble, who are accomodating, who are sympathetic and empathetic. The idea of "strong opinions, loosely held" is key. Being rigid or inflexible despite new, contradictory evidence is a big red flag.

# Candidate Considerations
- The candidate is likely nervous, anxious
- The desire to get a job if currently jobless can enhance this anxiety
- Allow candidate time to calm down
    - Start with easy, intro problems, then expand. 
- Ask them to highlight their background for a few minutes first. Helps get them comfortable talking, and to establish a rapport around their specific technical expertise.

# Technical Interview
We all (should) know to avoid the "gotcha" questions -- the types where the answer is obvious if you know it, but difficult to surmise if you do not. We're not trying to trick them, we're trying to get an honest assessment of their skill levels. 

I do believe there is value in whiteboard coding, but it can't be syntax-perfect. Pseudo-code will often suffice, since what I'm really looking for is the bounds of their competency. Whether or not they miss a semicolon is irrelevant. 

I think there is significantly more value in a technical pairing interview. Just a few years ago, we accomplished this with Google Meet, Zoom, or Skype. Nowadays we can just use HackerRank or other such tools. We do a screen share, with the candidate driving, and do some technical stuff, for example: 

- I will provide them with a problem and a test harness, and ask them to red-green test their way out. 
- Candidate will present a project (ideally public like on github), and I will ask them to do a show-and-tell. I'd be looking for automated tests, code quality, structure, and documentation, as well as other things
- Using a system like HackerRank, pair on a programming exercise. HackerRank works well here, since it allows us to let the candidate use whatever language they feel most comfortable in. 


# Selected Interview Questions
Here's a few of my favorite questions. 

## FizzBuzz
I will often warm candidates up with the classic FizzBuzz test. It's a relatively well-known problem, stated as: 

> Print integers of 1..N, except:
> - Print "Fizz" if an integer is divisible by three 
> - Print "Buzz" if an integer is divisible by five
> - Print “FizzBuzz” if an integer is divisible by both three **and** five.

The first few in the sequence: 

```
1, 2, Fizz, 4, Buzz, Fizz, 7, 8,  
Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz
```

I like this question because: 

- It requires the use of a looping construct.
- It requires the use of conditionals
- It requires a special case consideration for the "FizzBuzz" element
- It is relatively well-known
- It is often sufficient to shake off nerves before we get to the real questions. 

Ideal candidates will bust this out in a few minutes. Failing candiates will struggle with this question. 

If the candidate can't write a loop construct or the conditionals, in the language of their choice, it's a big red flag as to the level of their experience and actual hands-on proficiency.

## Software Priorities
In terms of (some software project), how would you stack rank these competing priorities:

1. Maintainability
2. Observability
3. Correctness
4. Performance
5. Reliability

Obviously they're all important, but the answer must change with the specific project. Medical devices, for example, may care more about reliability and correctness than observability. A GPU rendering pipeline might care about performance to the detriment of all other considerations. A web service might care more about observability and maintainability.

This is a good base for discussions on software architecture and cross-cutting concerns from a platform/systems standpoint. 

# As the Interviewee
- Remember that these folks want to hire you. Every single person you talk to wants the role filled. They're going to pay someone actual money to do this job, why not you? 
- Explain your thinking. It's ok to take a minute to think, but the interviewer may not know if you're just thinking, or if you are stuck or getting flustered.
    - If you do get flustered, it's ok to ask for a quick reset. 
    - Practice this with your Yellow Duckie. Talk to it like you're explaining what you're doing to a junior dev who's looking over your shoulder. 
- Be honest about your experience and contributions. It's ok to say "I don't know."
- It's ok to admit you'd have to look something up online. The interviewer likely doesn't expect you to remember every piece of syntax, but if you can explain what you'd do, that's just as good. 
    - For example, "given a large file of CSV user data, find all the unique email addresses."
    - "I'd set the IFS to a comma and select the email address field using awk but I'd have to look up the exact syntax for those. Then we can sort that with the `sort` command, then pipe it to `uniq` to dedupe" 
    - More important than knowing the information, is knowing that you know how to get and use the information. We all use Google. I have tabs and tabs of documentation open whenever I'm working on a project. We have modern tools like Copilot and Chat GPT. Being a [[Knowledge Worker]] means just that -- you work with knowledge, you don't hoard it. 


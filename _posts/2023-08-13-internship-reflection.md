---
layout: post
title:  "A loose collection of lessons from an internship"
date:   2023-08-13 09:30:00 -0600
categories: work internship
---

I just finished a 16 month internship placement. My opinions on software in general underwent constant metamorphosis during the past year and a half, but a constant thread running through my experience is a conflict between two personal truths: 

1. I loved what I did on a micro scale 
2. What I did on a macro scale was often upsetting to me 

The following lessons, mantras, koans, hot tips, and nuggets of advice are the result of what has been an eternal dialogue with myself. Take these not as hard truths, but as directions or interpretations of an attempt to resolve the dissonance in myself.  

_Note: After writing this I noticed a shocking resemblance to some of the advice in_ The Pragmatic Programmer _so go check that out if you like the vibe here_

### Clean Code is a useful framework 
You should strive to write clean code. The act of coming up with good function, class, and variable names makes you think about your code more and then makes you write better code which makes you think about your code more and your names more and it's all a lovely run on sentence of a loop that keeps improving and improving itself. Worship at the altar of clean code. Strive for perfection. 

### Clean Code is a crutch
Learn every prayer, every verse, and every single interpretation and sect in the Theology of Clean Code. Once you do, become an godless anarchist. There are no rules or laws. The doctrine of Clean Code is a flawed one, and the only way forward is to form a voluntary association of code. Clean Code is merely a social construct. Clean Code is whatever makes sense for the code you are writing. Throw away the oppressive and frankly outdated word of Uncle Bob and embrace the chaotic and beautiful world of unclean code.   

You won't have time to make it perfect, so make it good enough. It's always going to be bad, and even if code is self documenting and all that good stuff, code always changes and good function names change. All it takes is one new line of code to completely change the meaning behind a function name. So, learn to live with bad code. 

### Your employer (hopefully) wants you to learn and grow
I'm a bit spoiled in the sense that the first workplace of mine in the industry was a very healthy one. We were encouraged to learn and grow as developers, and there were a plethora of company wide initiatives such as Dev Talks, Hackathons, and Study groups to foster that. However what I did learn from the silver spooned experience is that you should want to work at an organization where this is the case. Working in software is about constantly staying up to date with modern tech, and you should be always learning. There is no perfect piece of software, and the process of making software is one of perpetual skill building. Every project and feature carries some lesson with it, and if your workplace is helping you learn those lessons you should try and stick around. 

### Code is expensive 
Software developers, at least at the time I'm writing this, are paid pretty well. Senior Developers in Canada make significantly more in salary than 99% of the labour force. In terms of just raw time and salary, code is expensive. It takes a team of 5 developers 2 weeks (the average sprint length) to build what product teams would consider a feature, and that's being _very_ generous in terms of scoping. Larger pieces of functionality can take upwards of 6 months, at which point the organization has probably spent $500,000 on just salary. 

There's also costs to actually letting people use that code. CPU time costs money! We need that CPU time to run tests, run CI/CD, make builds, run a staging server, run a production server, and possibly other environments such as sandboxes. It's all expensive! This is why real pressures to build products people want to pay for and to build things quickly and descope aggressively always end up arising in these projects. 

### Tech companies want to make money, duh
What you started working on was probably a cool tool that made peoples lives easier. Something that eased a pain in the real world, and as a software developer you should ideally love the challenge and reward that comes with expressing that real world pain into the logical domain of a programming language. In the software world, you can control the laws of the universe and solve that problem with absolute certainty. Time passes, the product gets sold, and eventually economics plays into how the product and organization operate. Turns out, you need to make money to pay for all that [expensive code](#code-is-expensive). Features get paywalled, pricing schemes are changed and introduces, and at least in my experience it starts to feel like the think that makes the app useful is now gated behind enterprise / mid market size corporate customers, and the average user who found the tool awesome in the first place is left in the dust. This really sucks as the engineer, since I would hope you derive some value from making something people actually like rather than some list of required features to make some important deal work out. 

### Coding is fun
I really like writing code. I really like trying to figure out how to make something work, from the smaller things like trying to implement a function to larger scale system design. The name of the beast is managing complexity and the only thing you can do to slay it is have an undying craving for simplicity. It doesn't really matter what you actually work on: 
> If you have the right attitude, interesting problems will find you. [_source_](http://www.catb.org/~esr/writings/cathedral-bazaar/cathedral-bazaar/ar01s02.html)

Some of my peers will often tell me they wouldn't work in X field or don't want to work in Y language. The thing is the core problem in development never changes, it's always about managing complexity. Every language, environment, tool, and stack is just trying to do that in a different way for a different use case. So, embrace the fun in all problems. 

### Writing code for a living makes it not fun

The day to day, or even week to week, of coding is really fun. Technical problems appeal to people who like to find interesting problems, and its a joy to solve them. What puts a dark stain on my heart is when I step back from the work and look at a wider scale of what I've worked on and see it amounts to something I don't like. In my case I spent weeks working on really interesting problems in Typescript/React only to realize what I built was a mechanism by which we can make the app frustrating to use for our "small fish" customers. I want to be clear here, I worked on a team that specifically dealt with Billing and Growth, so my perspective is very skewed. By all customer accounts our product was still great, but still! I don't enjoy writing paywalls just as much as I don't enjoy writing data tracking software or software that generates ad content. The comparison has been beaten beyond death but working as a developer can feel like living in a Quarter over Quarter loop of Frankenstein. 

### You know something that other people don't 

Your job as a developer is to communicate the technical perspective of any problem. You know something others don't, and its your job to explain that to them. This is really hard, and a lot of people stop at the first step. The first step is really easy. Most people know something that other people don't. Get to the second step and get good at explaining it to others! The people around you will have varying levels of ability to understand technical content, and it takes a lot of work to cater your explications to that understanding. But if you can't make everyone involved with a project understand the technical stuff, even at a high level, you aren't doing your job. 

### Be Resourceful 

Learn how your environment works. Learn keyboard shortcuts. Learn how to use your computer effectively. Learn how to edit text effectively and quickly. Learn tools to ease your work. If you have a frustration, find or make a tool that solves your pain point. If you don't have time to make a tool, complain to your co workers about it, someone else might have the time (or you might be lucky enough to have a Developer Experience team who's job it is to fix your problems). 

Learn how to find documentation. Learn to read documentation. Learn to write good documentation. Find help at the right times. If you come to someone else with a problem, at least try to have solved it yourself first. Someone else probably had your problem, go find the solution out there. 

Don't spend more than around an hour stuck on a problem. Don't spend any less than around an hour stuck on something. Allow yourself to be frustrated with problems, them let them sit and come back to them. Don't try to fix that one last thing at 4:45, chances are you will fix it in 5 minutes the next morning. 


---
title: "ChatGPT Initial Thoughts"
description: My first few weeks using ChatGPT for technical learning.
aside: false
date: 2023-03-20
tags:
  - ChatGPT
---

I’ve found my first few weeks of using ChatGPT to support some learning and personal projects absolutely fascinating. I have, in typical fashion, pivoted around a lot in terms of technical learning and honestly, ChatGPT has been another important resource to help things along the way. I’ve been working on a Rails app, which I decided to migrate to Phoenix, I’ve redone my personal site, introducing some more Vue components to the VitePress core, and lastly I’ve been working on some C# for a work tooling project.

I’m evenly inexperienced with each of the technologies I picked up and I found some were distinctly easier to get up and running than others. Now, these inconsistencies are likey a symptom of my own learning preferences, or the familiarity of the format of the language. Long story short I found the C# to be the easiest to pick up along side ChatGPT. There could also be a factor of widespread documentation for this language compared to others, and that the more widely used ChatGPT models; 3.5 and 4 were trained on data up to June 2021 and August 2022 respectively.

I’ve had the most success with using a combination of ChatGPT, as well as offical documentation. In my case I was using the official Azure DevOps client libraries, which are reasonably well documented and have a wealth of examples. I can’t emphasise enough that when ChatGPT got me lost, it was a combination of referring to code examples and StackOverflow that got me past it.

Having a threaded conversation with ChatGPT was one of the biggest boons. Being able to ask the chat to repeat itself, give explanations using analogies, and ask it to review my code and provide feedback was essential. Asking to implement solutions in an idiomatic way certainly pushed me to understand the approach of the language on a deeper level. This was key, for example for understanding the utility of interfaces and wrapper classes in C# when dealing with external dependancies.

While I used ChatGPT to get me up and running with unit testing, something I want to push it further on for my project is a good approach to integration/acceptance testing. Overall ChatGPT was keen to use the Moq library to create mock external endpoints, which, again was very useful.

There were instances were it would get itself stuck in a sort of loop, recommending I revert a change to something that I knew was previously also causing an error. I found that rephrasing the question did nothing to get it unstuck and eventually it was the official example code repo that got me unstuck. Additionally, when I was working with Rails, ChatGPT consistently tried to get me to use variable names that transpired to be protected keywords in that language. 

ChatGPT is an immensely powerful tool which lends itself especially well to supporting programming projects, with clear deterministic outcomes. Be wary of pasting in security keys, and taking its code recommendations on face value. 

It will not make you a programmer, but it certainly is an enhancement to a basis in technology. Who knows where it might go from here, or how long we will actually need to write lines of code to prompt such tools to generate business value, but as developers we are not here to write code. We are here to leverage our technical skills to solve problems. AI tools will allow us to implement our solutions more quickly. 

In day-to-day terms they are not a threat to our existence. The rising tide of these solutions will create their own problems, ones we can’t even conceive of yet that we will need to solve. From the prevalence of the cloud for hosting business platforms, frontend frameworks, CSS preprocessors, static site generators. We couldn’t have conceived of where these tools would take us, and what problems they would create that we would need to resolve before they were here.

I’ll continue to use the thing for now, and see how it develops. 

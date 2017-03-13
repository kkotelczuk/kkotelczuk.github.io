---
title: Project Concept
categories: Get_Noticed!
tags:
  - DSP1027
  - IT
  - games
  - JS
  - React
  - Syncano
cover: ./java-script.jpg
date: 2017-03-12 00:12:25
---


# ReDrawIt Concept

This time I want to describe why I decided to start this project, also I want to present some tech spec.

## What is ReDrawIt?

I wanted to make a pretty simple online game. The main concept of it is redrawing. The first player draws something on the whiteboard in some time, then another player has to redraw it in the same time. At the backend runs, image processing and the player gets the score based on the precision of his draw and left time. Then roles change and second player draw something and the first one has to redraw it. Simple right? 

The main challenge in this game is image processing. I decided to use JavaScript, which, I hope, have a lot of good image libraries. **I didn't make any research.** It was an impulse. 

## Technology

As I said above I want to use JS as the main language in this project. Frontend will be based on [ReactJS](https://github.com/reactjs) with [MobX](https://mobx.js.org/) architecture. I used this set of tech in my daily work and it works fine for me. I'm open for others frameworks, so changes are possible. 

As the backend, I want to use a [BaaS](https://www.techopedia.com/definition/29428/backend-as-a-service-baas). Today I'm targeting in [Syncano](https://syncano.io), which in my opinion is very good for such thing, but currently it's core libraries are in beta, so It can be a bit difficult. Syncano provides code box in which JS code runs really fast. There is also mechanism like triggers, schedules, real-time notifications, also mobile and hosting. In Syncano I can store data, processing images in the cloud, so I hope the game will work really fast. 

## Conclusion

After wrote the main concept here it's time to get to work. The first step is setup environment for ReactJS and Syncano, then CI, [Travis](https://travis-ci.org/) this time, and then maybe test environment and coverage reports. Time shows what will appears 

Have a nice weekend. 
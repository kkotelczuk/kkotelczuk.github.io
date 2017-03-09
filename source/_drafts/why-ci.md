---
title: why-ci
tags:
---

# Simplifying workflow

In every project there are a lot of dependencies. 

## Why Continuous Integration

CI simply automates your workflow. It manages your builds, tests and deploys. This piece of software resolving a lot of 'integration hell' problems. In my daily work I use Circle CI as main tool, which is connected with organizaton repository. After every commit on devel or master branch, Circle CI begins it's work. There is set of default commands that Circle does but most of them can and should be overwirtten. Most importatn thing that comes first is setting up enviroment from scrach every time build begins. hat means you have always fresh and clean machine with selected OS. Circle Ci is simple in usage and have low level of enetrance. but I'm here to learn something new. I decided to use another tool for CI for this competition. I have make a little reaserch about free CI software and I found it - shipabble! The most unintuitive, bootstrap based software.

But first thing first. It looks nice at screenshots and landingpage. Docs page looks really nice and clean. And there is free plan. Let's begin then! 
I have logged in via github and I saw nice guide window. Every link open docs instead of going to desired section, not bequem but works soehow. First step is enable repositeory, to do that you have to select icon in top left corner, then your repo and there is list of your projects. Here you an enable yur projects, simple right? Not at all... There are also something called pipelines. It is very important to understand this secton in order to use shipable properly. 
After enable project I have add yml fle to my project repository. From now shippabble will run every command in this yml file after every commit in repository. 
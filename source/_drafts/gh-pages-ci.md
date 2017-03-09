---
title: gh-pages-ci
tags:
---
# GH with CI

In my first technical blogpost I wnat to show how I setup CI for Github pages with Shippable. There is a lot of workarounds and I think it can be done simplier or better, and maybe some day I will return to this config and figure it out how to improve it, but for now this solution work for me.
## What is CI
Continuous Integration simply automates your workflow. It manages your builds, tests and deploys. This piece of software resolving a lot of 'integration hell' problems. Mostly it works in cloud server by setup clean machine for every build. Then it runs set of commands which can be default or defined by user. Every workflow can be personalized by yml file and overridding default commands. There are a lot of possible options to get desired results. In my daily work we use Circle CI which is integrated with our github organization account. For pruporses of this competition I want to learn something new, so I decided to use another tool. Somehow I missed out that Trvis CI is free for Open Source and I decided to use Shipable for setup this blog CI. 

## Setting up Shippable
Shipable is dedicated CI for working with docker machines, and have good integration with github. Its free plan is fine for me, so I decided to use it. I have prepared blog on master branch and I added source branch to my repo. On source I want to keep whole data from hexo. I want to use it also for ReDrawIt game, but after configuration, which will be described below I will try to use Travis instead.

### How to begin

For begin just login via Github. Shippable wants a lot of premissins including administratives one. Next I had to select project which shold be enebled. Projects are hidden under hamburger icon in top left corner. Select your account and then enable projct. After enablng it will show up on dashboard. Now Its time to begin with configuration.

### Yml file

First thing you will need is prepared yml file stored in project root directory. YML structure is really good described in docs. First you need to select used language and version of used engne. In my case it was javascrpt and node in version 6.9.4. Then you need to prepare build sequence. It is important to rember that all what happends before build is not avilable inside build sequence. Unfortunetly all configuration steps have to be done here :
```yml
build:                                                  
  ci:
    - git config --global user.email $GITHUB_EMAIL
    - git config --global user.name $GITHUB_USERNAME
    - npm install -g hexo
    - npm install
    - hexo generate
    - hexo deploy
```
As you can see irst I have to setup git and install hexo and its depedencies before generate blog. Last step 'hexo deploy' do all magic. It push changed files to master branch of my repositeroy via ssh. I didn't want to change it so I have to find way to make push form docker container.

### SSH key

To make push form docker container we will need na ssh key that will authorize this container. To do this you will have to add shh integration in Shippable. Go to your Shippable account setting and select integrations. Select Add Integrations and find ssh key. In next step you will have to select subscription which is your account on GH, key will be generted automaticly. I hope you know how to add ssh key to ihub, if not there is really good tutorial. Add key from this itegration to your gh account. Half way is after us. Now go back to your YML file. At the end of file you have to add Integrations section:
```yml
integrations:
  key:
    - integrationName: shippable_key
      type: ssh-key
```

### Shippable configuration
We are almost done. Now go to your shippable account, select your project and go to settings. In section Runs Config select which branch should be used. I have disabld master from building becouse there will be done deploy from source branch. Also you should setup account to process webhooks if not selected.
Last thing here is encrypt your private variables. Go to encrypt section type your values as described in exapmle and add generated string to your yml:
```yml
env:
  - secure: BfxKRR8mN2rU3+d8oemeXydGGkmH4aL+ROx3AzvUImwHNg8ljbWhp2iJ4AGqKxk6YHRC0KyPM57
```

After that you will have access to enviromental variables by `$VAR_NAME` 

## Conclusion

All what left is to test your CI, in your shippabe dashboard you can check out progress of your build and see errors if any occure. After all steps above you should be able to make push to your repo from docker container on Shippable. 
I hope it will help some day, some young developer who decide to use Shippable. Anyway I suggest to read Sippable docs which are not so bad as looks at first. 
It's all. Next time I probbably decide to use other tool for CI, but I will not give up on Shippable. It has potential, and I'm curius how works `pipenes` with all integrations offered by Shippable. 
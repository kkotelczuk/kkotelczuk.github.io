---
title: GitHub pages with CI
date: 2017-03-09 19:26:39
categories: Get_Noticed!
tags: [DSP1027, IT, GH-pages, CI, Shippable]
cover: ./cover-gh-pages-ci.png
---

# GitHub pages with CI

In my first technical blog post, I want to show how I have setup `CI` for `GitHub pages` with `Shippable`. There is a lot of workarounds in this solution and I think it can be done simpler or better, and maybe someday I will return to this config and figure it out how to improve it, but for now this works for me.

## What is CI
Continuous Integration simply `automates your workflow`. It manages your builds, tests, and deploys. This piece of software resolving a lot of _integration hell_ problems. Mostly it works in cloud server by setup clean machine for every build. Then it runs set of commands inside it. Every workflow can be personalized by YAML file and overriding default commands. There are a lot of possible options to get desired results. In my daily work, we use Circle CI which is integrated with our GitHub account. For purposes of this competition, I want to learn something new, so I decided to use another tool. Somehow I missed out that `Travis CI is free for Open Source` and I decided to use Shippable for setup this CI for this blog. 

## Using Shippable
Shippable is dedicated CI for working with docker machines, and have good integration with GitHub. Its' free plan is fine for me, so I decided to use it. I have prepared blog on the `master branch` and I added `source branch` to my repo. On the `source`, I want to keep whole data from `hexo.js`. I wanted to use it also for `ReDrawIt game`, but after configuration, which will be described below I will try to use Travis CI instead.

#### How to begin

For beginning just log in via Github. Next, select the project which should be enabled. Projects are hidden under `hamburger icon in the top left corner`. Select your account and then enable project. After enabling, it will `show up on the dashboard`. Now It's time, to begin CI configuration.

#### Configure repository

The first thing you have to do is add new YAML file, name it `shippable.yml` and store in project root directory. YAML structure is really good described in [docs](http://docs.shippable.com/ci/shippableyml/). At the beginning, you have to specify language version. In my case, it is javascript and node in version 6.9.4. Then you need to prepare build sequence. It is important to remember that **all that happens before the build is `not` available inside build sequence**. Here is an example of my build sequence:
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
As you can see first I have to setup git, using environment variables, and then install hexo.js. After installing all necessary dependencies and generate static pages it is time to run `hexo deploy`. This command push changed files to master branch via ssh. Here begins difficulty in this solution. How to connect to GitHub via ssh from container somewhere in Shippable. 

#### SSH Integration

To make a push from docker container you will need an `ssh key` that will authorize `this container`. To do this you have to add `shh integration in Shippable`. Go to your Shippable account setting and select `integrations`. Select `Add Integrations` and find `ssh key`. In next step you will have to select `subscription` which is your `account on GitHub`, the key will be generated automatically. I hope you know how to add ssh key to Github if not there is a really good [tutorial](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/). Now go back to your YAML file. At the end of the file you have to add `Integrations` section:
```yml
integrations:
  key:
    - integrationName: shippable_key
      type: ssh-key
```

#### Project configuration in Shippable
It's almost done. Now go to your Shippable account, select your project and go to its settings. In section `Runs Config` select which branch should be used. I have a disabled `master` from a building because there will be done `deploy` from `source branch`. Also, you should setup `account to process webhooks` if not selected.
Last thing here is to encrypt your `environmental variables`. Go to `encrypt section` type your values as described in the example and add generated string to your YAML:
```yml
env:
  - secure: BfxKRR8mN2rU3+d8oemeXydGGkmH4aL+ROx3AzvUImwHNg8ljbWhp2iJ4AGqKxk6YHRC0KyPM57
```

After that, you will have access to environmental variables by `$VAR_NAME` 

## Conclusion

Now you can test your CI, in your Shippable dashboard you can check out the progress of your build and see errors if any occur. After all steps above you should be able to make the push to your repo from docker container on Shippable. 
I hope it will help someday, some young developer who decide to use Shippable this way. Anyway, I suggest reading [Sippable docs](http://docs.shippable.com/) which are not so bad as looks at first. 
It's all. Next time I probably decide to use another tool for CI, but I will not give up on Shippable. It has potential, and I'm curious how works `pipelines` with all integrations offered by Shippable. 
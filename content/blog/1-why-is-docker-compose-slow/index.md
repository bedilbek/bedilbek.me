---
title: Why is Docker-Compose slow
date: "2020-05-02T22:18:17.000Z"
description: "Let's start this blog by observing docker-compose"
---
> I understand that docker or docker-compose do not have the worst documentation I have ever read. 
> However, the peculiarity exists in the behavior of its functionality.

#### If you've experienced one of the following issues, you're knocking on the right door
 - docker-compose is slow
 - docker-compose hangs
 - docker-compose hangs indefinitely
 - docker-compose stalling
 - docker-compose stuck
 - docker-compose takes a long time
 - etc

Anyway, to check whether you are absolutely part of this issue, execute the following command:
```bash
$ cat /proc/sys/kernel/random/entropy_avail
``` 
If you got `number >= 160`, you can breathe a sigh of relief and just throw this post straight into the trash.
This issue isn't related to you. But if `number < 160` and if you have already thrown the post into the trash, 
please take it back and continue reading it.

---
## TL;DR

You will find the workaround [here](https://libsodium.gitbook.io/doc/usage#sodium_init-stalling-on-linux).

---
## Premise
So, one of the strangest issues that you may notice while using docker-compose CLI  is that it may be slow 
or hang sometimes even if you just enter the command to get its version `$ docker-compose version`.  

Especially, It happens on cloud VPS or VM like Digital Ocean or Scaleway.

I'd say I was being tortured for almost a year not to understand the reason for this strange phenomenon, 
nor I had time to discover the issue causes. 

However, today I tried to dig into this for one more time and finally figured it out.

Actually, you could find the discussion page on the Github issue if you googled it after **07.03.2019**. 
So, if you google "*docker-compose takes a long time*", you will most probably 
get [github-issue-page](https://github.com/docker/compose/issues/6552) as a first result.
But, the [workaround](https://github.com/docker/compose/issues/6552#issuecomment-518725572) for that issue became 
only available after 06.08.2019 *(pity for others who had a similar issue before)*.

## Root Cause
The problem was inside the docker-compose library called *[libsodium](https://libsodium.gitbook.io/doc/)* that 
was used for cryptography and networking. 

It was calling `/dev/random` for generating a random number and if the system had a lack of entropy, 
then most probably you had a chance of being stuck on this issue.

---

Of course, this is not the actual issue of the docker-compose to document this behavior, however, tons of people 
who just start using docker-compose and deploy services to production come across this issue and it is better 
to make a notice of this in the documentation.

> p.s. I do not know by the time anyone reads this post whether documentation will be updated 
> or the issue will be resolved, but if you want this behavior to be documented, 
> please upvote (https://github.com/docker/compose/issues/6931).

## Conclusion
Kindly advise, please try to document as much as possible if you think you have built an amazing product to use :) 

---

<b>Hopefully</b>, I'll write more interesting things in the future.

<i>If you want to hear more about tech/random observation, then you are welcome to follow me on [medium](https://medium.bedilbek.me), 
or a better option would be to follow [my own blog](https://bedilbek.me), 
or the absolute best choice would be to follow [my telegram channel](https://t.me/s/khamidov_observes) </i>

##### Republished on:
 - [medium](https://medium.bedilbek.me/why-is-docker-compose-slow-c8b770e9c81e)


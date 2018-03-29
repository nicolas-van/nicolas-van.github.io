---
title: Let’s Encrypt is now working. Really.
---

I just put this blog on HTTPS. Why did I do that? Because I can.

[Let’s Encrypt](https://letsencrypt.org/) is a project sponsored by some important companies on Internet like Mozilla, Cisco or the Electronic Frontier Foundation. It’s goal is simple: be a free certificate authority.

When I heard about it like one year ago I was quite skeptical. They were mostly putting emphasis on the technical aspect to automatize the delivery of SSL certificates. But according to what I know the existence of a free certificate authority has never been a technical problem: it was a political one. The problem with SSL certificates is that they need to be delivered by an independent authority. That authority needs to be well recognized and respected. And what do you do when you are an authority and people come to you asking to sign something? You make them pay, always.

Here the big hypocrisy in this business is the price: technically speaking the process to assert the validity of a domain in order to deliver a domain validated SSL certificate can be fully automated. This means the real price to deliver that certificate is probably reduced to… a few cents? And what’s the price that most providers ask for a single domain validated certificate? About 50$ per year minimum. Yeah, that’s a good margin. Or extortion, you choose. It’s particularly bad because SSL certificates are necessary to encrypt communication on Internet mainly to help protect the users of websites, not so much the owners of those websites. And since certificates are expensive most blogs, forums, small communities and non-commercial websites in general can’t afford them. And so, each time someone naively logs to its nice little forum about whatever he likes he’s taking a risk because its password is sent in full text.

I was skeptical about Let’s Encrypt because I didn’t believed they would manage to solve the commercial/political problems in order to be recognized as a valid certificate authority. You see, if someone somehow delivers domain validated certificates freely there are a lot of companies (or \*cough\*racketeers\*cough\*) that will gain less money. And since those companies have agreements with other companies that decide which certificate authority can be trusted and which can’t, well, you see the problem.

But I don’t fucking know how, they managed to do it. Their client program is in open beta since approximately one week and they are now recognized as a valid certificate authority by all major browsers. The client is still a bit rough, notably it’s hard to put it in cron tasks to automate the renewal process (that you must do every 3 months), but it works. We can use it right now to get any number of certificates for all our websites. And that, buddy, is damn cool.

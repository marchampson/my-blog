---
extends: _layouts.post
section: content
title: Event Sourcing Terminology
date: 2021-10-02
description: This is your first blog post.
cover_image: 
featured: true
categories: [laravel]
---

**Event sourcing**. What exactly is event sourcing? 

There's a lot of talk at the moment about event sourcing in Laravel. The benefits are explained succinctly in a post from <a href="https://spatie.be/docs/laravel-event-sourcing/v5/introduction">Spatie.be</a>

>Event sourcing might be a good choice for your project if:

> - your app needs to make decisions based on the past

> - your app has auditing requirements: the reason why your app is in a certain state is equally as important as the state itself

> - you foresee that there will be a reporting need in the future, but you don't know yet which data you need to collect for those reports

All of this is true, but if you've not heard of event sourcing before then the terminology can be confusing. The aim of this post is not to give practical examples, for that, you should follow the excellent weekly streams from <a href="https://www.youtube.com/channel/UCBnj7HfncAygGeyymgydZxQ">Steve McDougall</a>, better known online as <a href="https://twitter.com/JustSteveKing">@JustSteveKing</a>. No, the aim of this post is merely to describe some of the event sourcing terminology in layman's terms. I've been in a number of discussions now and these seem to be a real stumbling block to getting started for some.

So, here we go... my interpretation of Aggregates, Projectors and Reactors.

**Aggregate**

I've seen a number of different explanations for this, but the simplest (other than mine) has to be from Eric Evans' book 'Domain Driven Design': 
> Aggregate: A cluster of associated objects that are treated as a unit for the purpose of data changes.

Because in a nutshell, this is exactly what the aggregate does, it's the starting point. It collects the data, it brings the elements together and starts the process. It, calls the event.


**Projector**

Projector classes listen for these events and action upon them accordingly. They are also a *doing* class. Projectors will affect the data. So, for example if the event is to add an item to a shopping basket. Then the projector will be responsible for ensuring the data has been updated.

**Reactor**

In a similar way to 'Projectors', Reactors listen for the event/projection and will **react** to it. The important distinction is that it is simply that, a reaction - it's not the event or the change. Think of laughing at a joke, that is your repsonse, your reaction - it's not the event, or the action of telling the joke, it's a one-time reaction. Cause and effect. 

So, what would you use reactors for? Well anything that you want to happen once, at the time, e.g. sending an email. In the last section, hopefully this will become clear. 

**Putting it all together**

Why have projectors and reactors? Why go through the overhead of events? Well, as we read at the start of this post, one of the benefits of event sourcing is to *replay* the events and report. You may not know it now, but further down the line there may be a requirement to report on all changes of a particular type on a Monday morning for example. Well not it's simple. You create a new **Projector** whose job is to collate that data into a count table for example and run it. The events can replay, you can target just that projector but, here's the clever bit - when you replay, only the projectors are called, never the reactors.

Now you can easily run projections on data at any point on historical events, but the reactions, the emails (for example) will never be sent again.






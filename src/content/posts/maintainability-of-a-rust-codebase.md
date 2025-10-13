---
title: 'Maintainability for rustaceans'
published: 2025-10-04
draft: true
toc: false
tags: ['rust']
---

Recently I have be frustrated with the way maintainability is approached (or rather, receded from) in most complex software systems in two main ways:

- It is seen as a chore rather than a good thing, often being assimilated to the gimicky [endless refactoring](https://www.reddit.com/r/ProgrammerHumor/comments/1dtf7ft/pleasestop/), even though it is [completely different]().
- It gets painted as a time-waster that would distract the team from doing more "productive work", especially in fast-growing organizations (eg. startups)

And before you start thinking this is going to be another rant from an overly strict -- often german -- software engineer about how everything should be perfectly aligned in a software project, I promise its not.

I actually want you to realize how sexy maintainability can be, for both engineering teams and product managers (and even C-levels). But let's not get ahead of ourselves. First, lets go back to the basics of what we want to talk about, what maintainability is.

# What is maintainability?

Instinctively, most developpers would define maintainability with some eponym expression like "it describes how easy a system is to maintain over time". After giving it more thought, they would probably come up with a more precise but less complete answer, describing its implementation principles or solutions like KISS, DRY and variations of the like.

But it is both more general and simpler than all of that. Borrowing from the [incredible book by Martin Kleppmann](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/), what maintainability is:

> Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.

The key word here is _*productively*_. Maintainability is simply the quality of a system in its ability to not get in the way of the software engineers working on it. And this benefits everyone!

- The engineers themselves, as they spend less time wondering which disgusting part of the codebase they are going to have to refactor before everything breaks down
- The product managers, as having a maintainable system makes feature estimates more precise and predictable
- The C-levels/shareholders, as more productivity means faster execution means [more money](https://en.meming.world/wiki/Stonks)

Having said that, maintainability is not all roses and flowers. Sometimes, you can't afford to take 20% more time to make things cleaner simply because engineering is just a part of the puzzle. Sometimes, you have to go fast, write shit code and ship a hotfix at 6am after a night of trying to get the service back on its feet.



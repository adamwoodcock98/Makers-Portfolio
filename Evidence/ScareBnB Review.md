![logo]()

Review of the MakersBnB Project
=========

Table of Contents
--------



Introduction
-------

This week's challenge was our first engineering project at Makers. The task: create a clone of Airbnb.

At the start of the course, we were given User Stories, domain models and more for our projects; we were then guided through the steps needed to complete a challenge. As we progressed, we were slowly given less and less of this information, and set to create it ourselves. In this challenge, we were simply given a list of headline specifications, and it was our job to intepret these into as many appropriate user stories as we saw fit.

This is the first time we are working in a team, and it is our job to coordinate our communication, do our planning, figure out our MVP and actually execute the implementation. A lot to cover, but we were feeling very eager to get started.

Organisation
--------

We immediately setup a Slack channel so that we could stay in touch and keep our most important links readily available in the channels bookmark bar (this inevitably grew over time):

![channel bookmarks]()

The first thing that we agreed was that our planning and organisation should be a priority, before we touched *any* code. We'd never worked in a team before, but we knew that things could easily spiral, and without these practices we could end up unfocused, wasting time and poorly executing the project.

## Trello

We setup a Trello board so that we could stay on top of our tasks, label them, categorise them and keep track of who was doing what, at a glance. I've used Trello before so I suggested starting with a To-do, doing, completed and would be nice columns. As this grew we added a doing *right now* column so that we could check what our team was up to in that moment, without disturbing them looking for an explanation. We also separated our to be review cards into dedicated to-do and completed columns.

![trello]()

I can't show all of our cards, but feel free to [have a scroll through online](https://trello.com/b/TRZYv4Gy/makersbnb).

I won't dwell too long on what is a very commonly used piece of software, but this board was our lifeline. We used Trello religiously throughtout the project, in our stand-ups, retrospectives and throughout the day. It proved immensely useful as we did not have to remember what needed doing, and our team could cherry pick smaller tasks if they needed something to do before a larger delegation.

## Schedule

There were quite a few workshops running throughout the week, some people had commitments that meant we needed to change the time of our retros and our pairing. Without something in black and white, it can be easy to overlook other commitments and think you have more time than you think - especially when you have your head down coding.

We also had deadlines we set for our MVP and release candidate. We felt it extremely important to aim for the headline specs by the end of Thursday, to give us the time on Friday before our presentation to focus on technical debt.

![schedule]()

This system worked well, as we knew exactly how much time in hours we had for coding as we moved through the week.

Planning
------

To start the planning process, we first had to explore the spec given us to us by our 'client', Makers. 

## Specification

* Any signed-up user can list a new space.
* Users can list multiple spaces.
* Users should be able to name their space, provide a short description of the space, and a price per night.
* Users should be able to offer a range of dates where their space is available.
* Any signed-up user can request to hire any space for one night, and this should be approved by the user that owns that space.
* Nights for which a space has already been booked should not be available for users to book that space.
* Until a user has confirmed a booking request, that space can still be booked for that night.

## User stories

From our specification, we worked through each of the headlines and created our user stories. Some of these points were simple, and others required breaking down into multiple stories. 

```
As a host,
So that I can make money,
I would like to list my property.

As a hosts,
So that I can view my listings,
I would like to create an account.

As a host,
So I can make even more money,
I would like to list multiple properties.

As a host,
So that I can keep track of my listings,
I would like to name my listings.

As a host,
So that my guests have some information,
I would like to add a description to my listing.

As a host,
So that my guests know the cost,
I would like to add a price to my listing.

As a host,
So that my property isn’t double-booked,
I would like to prevent my guests from booking unavailable dates.

As a user,
So that I’m only looking at relevant information,
I would like to sign in to my account

As a guest,
So that I have a roof over my head,
I would like to request a space for a particular night

As a host,
So that I’m in control of my space,
I would like to be able to confirm a guest request 

As a guest,
So that I know when I can book,
I would like to see available dates for a property.

As a guest,
So that I know when I cannot book
I would like to see unavailable dates grayed out

As a host,
So that I don’t lose out on bookings
I would like my property to remain available until confirmed
```

We really wanted to take our time working through these, it gave us the opportunity to start talking about how data may flow around our app, what our database schema might look like and how this would interact with our app through classes.

Now that we had our user stories, we went ahead and popped these into our Trello board. Our plan was to pair on specific, related user stories.

## Diagrams

No planning for a web app would be complete without some diagrams. The easiest place for us to start was with:

### Entity relationship diagrams

![entity-relationship-diagram]()

It's easier to start building a picture of the app when you know the database we would build.

We were planning on using ActiveRecord as our ORM as opposed to a custom implementation, and we knew that the less migrations we were running the better.

### Request/response cycle models

We started by creating the request/response diagrams for our MVP and the user authentication flow.

![]()

## MVP

Part of our task involved producing an MVP for our web app, in order to acheive this we needed to figure out *what* exactly our MVP should consist of. To ascertain these requirements, we asked ourselves two questions:
1. What are the most important features for Airbnb?
2. What features simply do not make sense on their own?

We struggled a little when answering this question. We soon realised that from a real-world perspective, the entire scope of our project is a realistic MVP for an *actual* Airbnb project, it's just we only had 24 hours to build it, not an entire month.

After debating this topic, we agreed that the most important feature of a short-term rental provider, would be the ability to show the user short-term rentals. Naturally, the functionality to actually add these listings came next. We extracted the user stories for these tasks, assigned them following our stand up and got to work. Specific cards for our MVP are tagged on the Trello board.

Tracking our progress
-------

We only had a week for our project, but we wanted to try and work in the most agile way possible. We therefore wanted to make sure we focused on eachother as individuals, and did everything possible to respond to change. We incorporated the following principles to help us work together as a team and support one another.

## Stand-ups

Part of working in an agile team involves morning stand-ups, we knew these would be vital points of contact and communication for the team. Having the organisation is great, but things change, ideas evolve and requirements change.

These stand-ups definitely ended up being a vital part of our day. Even though I found it difficult to think about anything but the project for the week and was most definitely dreaming in code, it's still easy to forget things after sleeping. These stand-ups told us exactly where we were in the project, address any challenges we were facing and allowed us to assign our tasks for the day. Then, everything was fresh, and we all had a good idea of what everyone was doing that day, and for a reminder, we could just look at the 'Doing right now' column of the Trello board.

On the last 2 days, when time was short, we decided to run two stand-ups in a day, a more thorough one in the morning and a more informal one midday to make sure there were no new causes for concern that needed to be addressed as a group.

I wanted to ensure that we had a record of our meetings to refer back to, so I did some research at the start of the week for how they are run in an agile capacity, and we began using the following form:

![first standup]()

> We also recorded the audio for our first standup, which can be listened to [here]() (run by yours truly). Apologies for poor audio quality. *All members of the team gave me permission to share this here*.

This [Drive folder]() contains the PDFs for the rest of the week if you would like to view them. In the interest of giving everyone a chance, we rotated the leadership of these standups to whichever group members wanted to.

## Retrospectives

We run weekly retrospectives on a Friday as part of the general Makers curriculum that are very beneficial, reflective experiences. We therefore knew we wanted to use them in our group project.

Personally I found they were excellent for decompressing any stresses from the day. With a project such as this, we can often have thoughts throughout the day of something that needs addressing, and may even leave you feeling a little worried. The retrospective was the perfect time to address this action, you have the attention of the group without disturbing them, and a medium to note down the problem.

We followed a reasonably standard format of what went well, what could've gone better and actions. The positive experiences are a great chance to feel good, to remind ourselves and feel proud of everything we've achieved and boost that motivation and engagement. Whenever we read through these there were smiles and laughter all round, it really did have a huge positive impact on us all.

The processes that could've gone better are an opportunity to make process improvements, discuss something that negatively impacted an individual or the group in any type of way and bring everyone up to speed with something that may not have been addressed. It also provided the platform to explore *why* something may not have gone well, was there a way in which a team member could've been supported either technically or non-technically.

The actions were a great opportunity to discuss what was coming next, whether something needed to be changed, to quickly run through approaches and give us a chance to form a loose plan of the next day. It also helped to set expectations across the group on what needed doing.

The following is an example of the results of running our first retrospective, for photos of the rest, please check this [Drive folder]().

![monday retro]()

Analysis
------

This group project is as much akin to the professional workflow that we will be able to experience, therefore I felt it very appropriate to do my best at a critical analysis of my work. I do so in the hopes that it will highlight any areas of improvements for the two forthcoming group projects, and my career going forward.

I have approached this as a modified SWOT analysis, not with any recognised analysis model, just what I felt was relevant to the project.

## Strengths

### Organisation

First and foremost, I feel that the main strength of our group was our organisation. I've already spoken heavily in this review of the tools we used to achieve this, but more than just the fact that we used these, we *stuck* to them. Every member of the team made a point of using our Trello board, keeping cards up-to-date, adding new ones when necessary, including pull request reviews and appropriate tagging and member assignment. We stuck to the schedule, including being regemented with our stand-ups and retrospectivs.

The product of this organisation was efficiency, we rarely had to interrupt other team members, concerns were always addressed at the appropriate times, and there were relevant channels to note and discuss desired changes to scope and workflow. We output work quickly and effectively, we knew our priorities, and at times when we may have been close to going off topic, we did not. We achieved a lot in the time we were given, and I feel confident attributing a lot of this to our organisation.

Every member of our team played their part in the success of this structure, but a need for organisation runs deep within my bones, so I must give myself some credit for pushing this so hard when we started the course and getting these resources up and running.

### Work ethic

This is not so much a strength, more a lack of a problem. There wasn't a single point where it felt like a specific member of the team was 'not pulling their weight', we all worked extremely hard, everyone did as much as they could, actively partook in discussions, jumping in to help other members and so much more. We were all prompt and showed in whatever ways the group demanded. I am extremely grateful for my team in this respect, we all know that a chain is only as strong as its weakest link.

### Separation of responsibilities

From get-go, whether it was 100% planning, or mostly planning and a little luck, but we broke down our workload into steps that worked very well. This meant that there were very few moments - other than those that were inherently unavoidable - where we felt we needed the output of another individual or pair in order to achieve what we needed to achieve.

### Fluidity

On the first day of the project when we were planning the format of our pairing, we felt it pertinent to create a system that would allow us to swap partners regularly, without creating defecits in our output through a lack of understanding. We had clear goals, and we never fully swapped a pair when individual tasks of a larger grouping were completed. We always kept one person from a pair working on a group of tasks after a swap, so that they could bring people up to speed as they worked, so the ball was never dropped. This did not mean that one person stayed on a group of tasks the entire team, just each time a swap happened.

### Support of team learning

There were times when a specific member of the group had the responsibility of learning something new for the project, or completing a big piece of functionality. Even though time was short, we made of point of going through anything new as a group, to ensure that everyone gained the fruits of that knowledge that they could apply going forward.

## Weaknesses

### Unhealthy balance

As much as I have already praised our work ethic, it would be non-objective of me to criticise when this became unhealthy. We did of course want to do well, and achieve all of those goals, but it would've been better to do this with a healthy balance. More than half of the team, myself included, were working late into the evenings. Most of the things that we did were non-essential, in my case I did most of our CSS at home, which is most likely why we were able to achieve a more stylised look than other groups in the cohort, even if it would still not win any prizes for its design.

## Challenges

### ActiveRecord, user authentication and Rake

I don't think we quite realised how much of a pivotal point this proves to be. None of us in the group had used ActiveRecord before, and we would end up having Paul fly solo to research this, and pair implement it with Sam.

We decided to use Rake alongside ActiveRecord to handle all of our migrations, and in hindsight this definitely ended up being more of a hindrance than a help. It didn't seem to matter what we did, but if we needed to make changes to our schema, commands like `rake db:schema:load` just did not do what they were meant to. This meant whenever a change was made, we had to drop all of our tables and run `rake db:create`, `rake db:migrate` and `rake db:migrate RACK_ENV=test` over and over. If we simply used SQL migration files, we could've just run them in the command line to update our tables, whilst still using ActiveRecord. This would've meant we didn't have to completely re-populate our dev environment.

The issues weren't huge, but they were time consuming, everytime we needed to make changes, we had to check whether rake was working, then do things manually anyway, across the entire team.

### The MVP

It was debating this topic that we had our first disagreement within the group; most of us had agreed on the aforementioned 2 features for our MVP. One member of our group felt quite strongly that user authentication sign in/sign out had a place in the MVP. Truthfully, I could see the point, however, as mentioned the project is essentially one big MVP and in that scope authentication makes sense, but with our timescales and priorities this was a little out of scope.

Despite the small conflict, we heard their points, ensuring we gave thoughtful answers and asked questions to help both them, and us, understand and confirm that not including this was the best move we could make for our group. Eventually, we all settled that this would be our MVP and that authentication would be coming next.

## Obstacles

### Other timetabled commitments

We actually lost a significant amount of our time during the week to other activities on our Makers timetable. For instance on the Wednesday, we each had about 1 hour of coding time throughout the entire day. 

## How to improve / what would I do differently next time



## Teamworking



## Lessons learned

### Request/response cycle diagrams

When building web apps that have a lot more functionality such as this one, the routes quickly start to pile up. On the last day of the project, we ran into some difficulty when joining up work that different pairs had completed. The routes were not working together, and were funcitoning in different ways, so when it came to the integration we had a lengthy debugging process that took much longer than it should've.

In hindsight, had we done the diagramming for this functionality, there would've been no confusion, we'd have all been working from the same assumptions, and the integration would've been much simpler.

### Not using Rake

As I've already spoken about, Rake did not function as we had expected it to. Paul was responsible at the beginning of the project for getting ActiveRecord and Rake setup. With no experience using either library prior to this project, it took him quite a long time to get setup. Had we only focused on setting up ActiveRecord, this process would've been done much quicker, meaning we could've used this quicker, saving time for the rest of us from manually creating ORM functions to read/write data from/to our database.

### Remaining calm

There were definitely times throughout the project that we all felt stress about what we needed to achieve. The reality is that we should've trusted in our planning and organisation, we always remained on track and there was never an indication that this might change.

I know a lot of this stress was attributed to the fact that this was our first group project and we wanted to do well, however it was entirely unneccessary, and there would've no doubt been a process improvement had we not been feeling this way at various times.

## Project highlights
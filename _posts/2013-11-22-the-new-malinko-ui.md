---
layout: post
title: "The New Malinko UI"
description: "Description of the new UI for the Malinko software"
category: 
tags: []
---
{% include JB/setup %}

There's been consensus for a while that the Malinko user interface has been long overdue a refresh - which is exactly what I was tasked with for my first two months at the company.

Starting the process, as a team we identified the main things that we wanted to focus on for our users; from this we were able to agree on five design principles (along with a few sub-points) that would guide both the redesign and our attitude toward the user experience of Malinko moving forward. 

So, without further ado, the five principles are: 

1. Consistency
  - of user interface
  - of location

2. Interactivity

3. Simplicity

4. Clarity
  - cause and effect
  - increasing user confidence
  - aiding understanding
    - (these factors will lead to increased authority and usability)

5. Suitability 
  - meeting user and regulatory standards
  - functional tool
  - fit for purpose
    - (our concern is to balance flexibility with appropriateness)

What this has meant in practice for my first iteration of the new look and feel has been changing the colour scheme, Malinko's logo and our typeface as well as standardising user interface elements like buttons. To use that example, actions that cause permanent potentially destructive change to data (except for log out) will always be represented by a red button; changes to data (such as a save action) will be a green button; changes to the same page, orange. I'm going to be adding to or amending this functionality as we talk to users and test the software more thoroughly, but I felt that by simplifying our colour scheme and bringing in new colours chiefly through 'actions' brings more clarity to the end user in terms of actually achieving tasks. 

So, in short, what users can expect from the new interface is more whitespace, larger font size, better spacing and more information displayed on screen. Where necessary, this has meant hiding extra information behind hover-overs.

The next task was to start to assemble views with the consideration that they might have a large number of staff - this resulted in several interactive pages making extensive use of AJAX, called at present the Enterprise Day View, Leave Planner and (New) Day View. From separating out the design from lots of small portlets into two large columns I then tabbed the main content column to the right in order to give users more visual clues as to its purpose. By having a tab to physically click in order to display information and/or actions, it makes the user flow more declarative, and hopefully more UX simplification like this will be appreciated by users. 

Another important piece of work has been the creation of a testing suite using RSpec and Capybara, which is already catching bugs before code is moved to production. We've got seventy-odd tests now, and test coverage is improving all the time. It's probably less interesting to talk about if you're not a Rails dev though, so I'll leave it at that.  
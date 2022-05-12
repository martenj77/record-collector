---
title: "Activity tracker part 1"
date: 2022-05-13
description: ""
categories:
  - Code
images:
  - /files/git01.jpg
authorname: "Mårten Johannesson"
authorimage: "/files/mj.jpg"
robots: "noindex"
---
Creating my own activities tracking system.
<!--more-->

I decided to take my coding skills and make something. A personal training activities tracker. Yes, I know there are one million and two of them out there, but I thought about it and saw that the scope I imagined was within reach.

I want a webpage to enter a training activity, some specifics about it like the duration and distance (if applicable). Also the location (like what city, if I was at home etc) and the date. That's it. By keeping it simple I have the vision that this is something that I actually can make a habit of over the years.

And I then see that I need some kind of dashboard to present the activites to see trends etc. And that's part two I guess.

## Tech stack

I'm looking to build this in .NET core and use techniques like EF Core and razor pages. I'm on the fence as to use Tailwind or Bootstrap for the UI, but that's no rush. In the beginning I'll use a Sqlite database.

## Quick notes

* I'm using the book *C#9 and .NET 5* by Mark J. Price as reference. In the beginning I'm looking at the chapters 11 (EF core) and 12 (LINQ)

* Sqlite is easy to setup. Then I use SQLite Studio. I created the data model in the studio. I've yet to find an easy way to create a diagram of the schema though.

* I used the reverse engineer approach to EF. That is, setting up the db first and the scaffolding the model.

```ps
dotnet add package Microsoft.EntityFrameworkCore.Design

dotnet ef dbcontext scaffold "Filename=training.db" Microsoft.EntityFrameworkCore.Sqlite --output-dir AutoGenModels --namespace Training.Shared.Autogen --data-annotations --context Training
 ```

* I learned that if I have a new project and initialized git but have no gitignore file set up I can easily create one with a standard .NET setup by running `dotnet new gitignore` in the root of the project.

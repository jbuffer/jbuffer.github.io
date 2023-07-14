---
layout: post
title: "Shiny for Python: First thoughts, .env files, and databases"
date: 2023-07-14
---

# Shiny for Python
I am sure many data scientists out there are super amped about the announcement made by Posit that [Shiny for Python](https://shiny.posit.co/py/) has moved from alpha to general availability (yep <img src="https://media.giphy.com/media/hvRJCLFzcasrR4ia7z/giphy.gif" width="20" height="20" />, I am definitely one of them!).

Sure R has its plus points (I am looking at you ggplot2, my first data viz-package love 😉) but as a major Python fangirl, making Shiny applications was not as fun as it should have been. Enter stage left Shiny for Python for a potential full back-end to front-end Python experience.

## Taking Shiny for Python for a spin
Fortunately, I didn't have to wait long for a reason to give it a spin. I had the perfect use case: a simple dashboard to visualise metrics stored in a database. 

**Requirements:**

&#x2611; the app had be hosted for free (very important)

&#x2611; it had to be hosted somewhere that complies with organisational policies. 

&#x2611; it had to be quick and simple to build

&#x2611; It could not be in R (OK, fine, that one was just my requirement).

*Note: Streamlit is another extremely simple-to-write Python front-end framework, but Joe Cheng talks more about some reasons as to why **not** to use Streamlit (amongst other things) in an excellent talk [here](https://www.youtube.com/watch?v=ijRBbtT2tgc).* 

## First thoughts

With an organisational subscription to shinyapps.io, using Python-flavour Shiny made sense. I got cracking on the app development using the excellent resources at [Shiny for Python](https://shiny.posit.co/py/docs/overview.html) (or PyShiny), and pretty quickly had the bare bones of the app. My initial observations of the process were:
- developing in VSCode (using the Shiny for Python [extension](https://marketplace.visualstudio.com/items?itemName=Posit.shiny-python)) was really straightforward - a 'simple browser' opens up in VSCode when you run your app and automatically refreshes when you make any changes. Because VSCode supports multiple languages, this is also a good IDE for CSS and JavaScript (more on that shortly).
- the documentation was good, but the fact that this is still a relatively immature product meant that some content was out of date.
- similarly, if you are expecting the bells and whistles that come with RShiny, you might be a bit disappointed. Currently, there is no `shinycssloaders` or `ShinyWidgets`... basically, you need to be super comfortable with CSS and JavaScript to really customise your dashboard.
-  in my opinion the code is more austere and stripped back than RShiny in a really good way: on the ui side, there is a series of nested function calls which mirror the shape of the dashboard. Server-side, it is simple to implement reactivity (using decorators).

## Deployment madness 🚀
OK so a few drawbacks, but in general, so far, so good. I can deploy this app locally, it looks fantastic, everything works fine, happy days. 

Except when it comes to publishing my app on shinyapps.io. Then things get a bit messy and some of the issues I mentioned above (immature product, some gaps in documentation) really come to the fore. 

### Challenge 1: .env file shenanigans
The [instructions](https://docs.posit.co/shinyapps.io/getting-started.html#working-with-shiny-for-python) shinyapps.io supplies provides a step-by-step guide for setting up `rsconnect`. Except it omits quite an important detail - the `.env` file **MUST** be named - think of it as an anti-Voldemort. ALL of the thanks to my colleague for pointing this out to me and sparing me that pain of figuring this out myself, which might have happened never 😐.

### Challenge 2: database drivers
When using RShiny and RStudio, connecting to a database (in this case an MS SQL Server database) in the app locally and on shinyapps.io is straightforward; using `odbc` and `DBI` packages means that you can use Posit's drivers. However, it turns out `sqlalchemy` and / or `pyodbc` will not be able to access these drivers. A pretty big problem when all the data I need is in this database.

I thought the gig was up... thanks Shiny for Python, it was nice while it lasted. However, my stackoverflow-doomscrolling eventually bore fruit and the solution presented itself. No driver installation required! I could use a package I hadn't come across before: [`pymssql`](https://github.com/pymssql/pymssql), which builds on top of [FreeTDS](http://www.freetds.org). 

## Fin 😰
One fully deployed app later, and my final thoughts are thus: Shiny for Python is fantastic for creating quick-and-dirty apps (probably for internal users), but it has a bit of a way to go to scale the heights of RShiny. Hopefully the userbase will take off and all the frills that RShiny currently enjoys will come to the Python-side. 

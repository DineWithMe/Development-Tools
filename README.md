# README

Let me know if you want to update this repository.

No need to create branch for this repository, it serves as quick info, everyone can push it to master branch.

I recommend you commit this repository in your editor, it saves you from typing "br" tag.

This repository is not intended to be learning material (but will still add some), this repository list down the tools/libraries/frameworks used in this project and justification behind the decision.

## Library

When come to choosing the tools/libraries/frameworks, there is massive amount of options available. To avoid all sort of complexity, I adopt a simple and safe mindset when come in decision making.

Basically we use reputed and popular tools, because I believe they are more stable, more resourceful and higher chance of answering our technical problem.

Well it is not like I dont care about better technology, we will adopt better technology if they are proven to have clear advantages over existing and significant size of community.

Here is overview of what important library we are going to use in this project, of course it may change depend on need

Front End              | Library
---------------------- | -------------
UI Rendering           | React
Web Page Rendering     | Next
State Management       | Redux
UI Component Testing   | Enzyme
Bundler                | Webpack*

*comes with and pre-configured by Next

Back End               | Library
---------------------- | -------------
Server                 | Node
Server Framework       | Graphql-Yoga
Database               | Postgres
ORM                    | Prisma
Authentication Token   | JWT
Query Language         | GraphQL
Redis                  | Caching

Generic                | Library
---------------------- | -------------
Test                   | Jest
Transpiler             | Babel

## Editor

Recommendation on visual studio code extensions also available. Having a good editor is like having a good weapon, I strongly suggest you polish your weapon to ease your development process and enhance your development experience.

Unlike library, editor choice is very personal, use what fit you best.

Editor                 | 
---------------------- |
VSCode                 |

## DevOps

We should cultivate devOps culture as soon as possible, from planning, building, testing, deployment, and monitoring, we will come up with suitable tool for every category and try to automate some of the process safely.

Purpose                | Tools
---------------------- | -------------
Continuous Integration | CircleCI**
Container              | Docker
Serverless             | Zeit Now, Heroku, Prisma

**not yet finalize, may switch to Jenkins but CircleCI seem convenience enough, ultimately it depend on the cost

## Rule and Styling

We will also touch on coding style and rule, this can be very opinioned like: should we keep semicolon? should we keep unused argument? etc etc

I have two core philosophies for this:

1. **trying to be explicit and intuitive while keeping the code low.**

For example: 

I will not keep the semicolon for 2 reasons:
  * it is the source of syntax error if we forgot to remove it.
  * With or without it, it doesn't provide any meaningful explicit information.

So, should we keep unused argument? I have no clear answer for this, 
  * removing unused argument make the code cleaner .
  * But keeping it explicitly tell the newcomer or remind the old man what this thing can potentially do at the first glance. 
  * However you can always refer to the documentation.

  Personally I am more incline to remove the unused argument but I am also willing to keep them for the sake of explicitly. 

2. **Styling and rule, especially styling, is not as important as people think it is.**

Styling and rule are preferences, and most of the time people think their preference is important, But in reality it is not. Yes a good styling definitely help, but to what extend? how much it improves your workflow? 10%? 5%? 0.5%? Is it worth your time?

When come to styling/rule, it is not about who has the best styling and code, it is about consistency across the team, we need to make sure everyone use the same style and code so that in long term they can understand each other code easier(when they get used to the rule and styling).

Actually the good styling/rule and consistency are not conflicting each other, because why not both? So an easy solution for this is to install dev-dependency that enforce the good styling/rule. 

So just take some time to pick a rule/styling and enforce it, that is all.

Code                   | Library
---------------------- | -------------
Styling                | Prettier
Rule                   | Eslint
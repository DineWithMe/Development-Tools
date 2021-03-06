# README

**This repository is far from complete, let me know if you want to help update.**

No need to create branch for this repository, it serves as quick info, everyone can push it to master branch.

I recommend you commit this repository in your editor, it saves you from typing "br" tag.

This repository is not intended to be learning material (but will still add some), this repository list down the tools/libraries/frameworks used in this project and justification behind the decision.

## Library

When come to choosing the tools/libraries/frameworks, there is massive amount of options available. To avoid all sort of complexity, I adopt a simple and safe mindset when come in decision making.

Basically we use reputed and popular tools, because I believe they are more stable, more resourceful and higher chance of answering our technical problem.

Well it is not like I dont care about better technology, we will adopt better technology if they are proven to have clear advantages over existing and significant size of community.

Here is overview of what important library we are going to use in this project, of course it may change depend on need:

| Front End            | Libraries | Website                                 |
| -------------------- | --------- | --------------------------------------- |
| UI Rendering         | React     | https://reactjs.org/                    |
| Web Page Rendering   | Next      | https://nextjs.org/                     |
| Client               | Apollo    | https://www.apollographql.com           |
| State Management     | Unstated  | https://github.com/jamiebuilds/unstated |
| UI Component Testing | Enzyme    | https://airbnb.io/enzyme/docs/api/      |
| Bundler              | Webpack\* | https://webpack.js.org/                 |

\*comes with and pre-configured by Next, actually I prefer Parcel

| Back End             | Libraries    | Website                                |
| -------------------- | ------------ | -------------------------------------- |
| Server               | Node         | https://nodejs.org/en/                 |
| Server Framework     | Graphql-Yoga | https://github.com/prisma/graphql-yoga |
| Database             | Postgres\*   | https://www.postgresql.org/            |
| ORM                  | Prisma       | https://www.prisma.io/                 |
| Authentication Token | JWT          | https://jwt.io/                        |
| Query Language       | GraphQL      | https://graphql.org/                   |
| Caching              | Redis        | https://redis.io/                      |
| Web Server           | Caddy        | https://caddyserver.com/               |

\*or Mongodb, need more research

| Generic    | Libraries | Website             |
| ---------- | --------- | ------------------- |
| Test       | Jest      | https://jestjs.io/  |
| Transpiler | Babel     | https://babeljs.io/ |

## Editor

Recommendation on visual studio code extensions also available. Having a good editor is like having a good weapon, I strongly suggest you polish your weapon to ease your development process and enhance your development experience.

Unlike library, editor choice is very personal, use what fit you best.

| Editor | Website                        |
| ------ | ------------------------------ |
| VSCode | https://code.visualstudio.com/ |

## DevOps

We should cultivate devOps culture as soon as possible, from planning, building, testing, deployment, and monitoring, we will come up with suitable tool for every category and try to automate build, test and deploy of the process safely.

| Purpose                | Tools                    | Website                 |
| ---------------------- | ------------------------ | ----------------------- |
| Continuous Integration | CircleCI\*               | https://circleci.com/   |
| Container              | Docker                   | https://www.docker.com/ |
| Serverless             | Now, Heroku, AWS, DO\*\* |
| Version Control        | Git                      | https://git-scm.com/    |
| Repository             | Github                   | https://github.com/     |

\*not yet finalize, may switch to Jenkin but CircleCI seems convenience enough, ultimately it depends on the cost  
\*\*in term of cost i recommend DO(Digital Ocean), it is more flexible but more manual work, require technical knowledge in Ubuntu

## Rule and Styling

We will also touch on coding style and rule, this can be very opinioned like: should we keep semicolon? should we keep unused argument? etc etc

I have two core philosophies for this:

1. **trying to be explicit and intuitive while keeping the code low.**

For example:

I will not keep the semicolon for 2 reasons:

-   it is the source of syntax error if we forgot to remove it.
-   With or without it, it doesn't provide any meaningful explicit information.

So, should we keep unused argument? I have no clear answer for this,

-   removing unused argument make the code cleaner.
-   But keeping it explicitly tell the newcomer or remind the old man what this thing can potentially do at the first glance.
-   However you can always refer to the documentation.

Personally I am more incline to remove the unused argument but I am also willing to keep them for the sake of explicitly.

2. **What is a good styling?**

While styling can be very subjective, we still has some clue what a good styling is.

A good styling reduce communication cost, the mental cost of person trying to read the code.  
Is the code too long on a line?  
Is the indentation make sense?  
Can you identify all arguments on first sight?  
etc etc

These small details make our life easier when we review a code.

Yes it might look trivia, but imagine all the time you wasted trying to read codes, it is totally worth to spend some time to enforce styling across the team.

| Code    | Tools    | Website              |
| ------- | -------- | -------------------- |
| Styling | Prettier | https://prettier.io/ |
| Rule    | Eslint   | https://eslint.org/  |

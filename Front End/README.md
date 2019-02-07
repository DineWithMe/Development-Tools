# Front End Technologies

Depend on a lot of factors and needs, libraries will change from time to time.

Here is simple justification of important library that we are going to use.

## React

![](./img/React.jpeg)

We use react to build user interface. Personally I like the idea of component, it changes the philosophy of separation of concern. It increase the learning curve as it requires a programmer to have all kinds of knowledge (html+js+css).

However this doesn't necessary increase the workload as others can work on another component without you involved in it. The best thing is, component thinking allows us to reuse the resources easily and I believe this is a correct direction.

| Employees | Conventional | React          |
| --------- | ------------ | -------------- |
| A         | do html      | do component x |
| B         | do js        | do component y |
| C         | do css       | do component z |

_This table illustrates how React changes the paradigm of separation of concern._

But why react? Because there are also library that adopt or gravitate toward component thinking, why react?

Again my answer for this is quite simple: stable and reputed, it is a safe choice.

-   More job opportunities, it is easier to get people to join this project especially fot those who want to gain experience.
-   It has big ecosystem, resourceful, lots of library created around react.
-   Big ecosystem also means, technical problems that you encountered are most likely answered on internet.
-   It developed by big company, it will last longer.

Another thing I want to bring up is the different between React Component Render and React DOM Render, which I believe is not properly explain to newbie when they learn React.

React Component Render simply update the React Component when state change, it does not really render the DOM while the other hand React DOM Render render DOM element(it will compare new and old virtual DOM and only mutate what is changed).

If I would pick up another library, I think Vue is a good choice and Imba is very interesting tech to keep eye on.

## Next

![](./img/Next.png)

There are actually not much React frameworks out there when come to server side renderer, presumably Next, Gatsby(better known as static page generator) and After. Why we need server side rendering? Basically 2 reasons: performance and search engine optimization(SEO purpose is actually getting trivial).

In the world of internet, we all at the mercy of Google (joke, Google actually want related site to be discover to increase search precision). To improve your Google search ranking, you need to tell Google what your site are and the way to do so is to let Google crawl your page and understand your content as much as possible.

But when you send a React page, you are actually sending skeleton html page with no content for Google to crawl, the content only available after client finish render it, here is why server side rendering shine. The good news is Google is improving client side rendering crawling algorithm and we also have pre-rendering as solution for SEO, which is why it is getting trivial to boost SEO score via SSR.

But keep in mind that Google is not the only search engine out there, Google has market share of around 80%.

Instead of SEO, what we should focus is performance, server side rendering effectively reduce round trip make to server, this is good if your user has slow connection. Yes, even though internet speed getting better and better, slow connection is still a major problem because latency is not something that improve with bandwidth.

However even though round trips make to server is lesser per page, the request to server is actually much higher than that in CSR this is because every page is a request to server, and this is where the client side rendering did a better job. Although CSR has higher initial page load than SCR but the subsequent page load is fast because it is no longer require another request to server.

That is why I choose Next, because Next is actually an universal renderer, it can renders both client side and server side, we can have the best of both. With Nextjs, it is possible that the initial page load is done with SSR and the subsequent page load can be done with CSR.

Looking for static page generator? Next also can export static file.

Other than Next, Gatsby is extremely attractive option is you want static page, it is blazing fast, I strongly recommend it but somehow tricky to render dynamic content with Gatsby(I dont have much research yet); speaking about After, After is fairly new and heavily inspired by Next, the different is After adopt different routing philosophy (which Next will adopt too).

## Now

![](./img/Now.png)

When come to deployment, Now is probably the most attractive platform to deploy our app from, it has global DNS and global CDN(only in paid plan) but this is not the reason behind the attraction.

It is because of how easy to deploy app on it, you can do it in just one command and your website is online now(pun) plus automatic SSL certification, pay some fee and you get a global CDN and anti DDoS.

This means a lot for sole developer which is just impossible to micromanage everything. As a sole developer you should always put more resources in developing your app, not server and infrastructure because we simply dont have that much of time.

Now make your life a breeze but of course Now is not the only serverless out there, there are Heroku, Cloudflare, Digital Ocean, Hostgator and big names like IBM, Google, Microsoft and Amazon but none of it is as easy as Now when come to deploying Node app, the closest one would be Heroku.

Extra: Now and Next are developed by the same company, Zeit.

## Redux

_this paragraph is deprecated, now we use Unstated but keep this for the purpose of reading_

![](./img/Redux.png)

Dealing with state can be complicated, by nature it is a simple thing, it is just a state, an variable/object that hold any kind of information, be it input/properties/status/data.

However when come to complex application where everything is complicated, state management can be even more frustrating, it is not hard to imagine a state change that chain another state change and each state change manipulate the UI/component in their own way which is not easy for developer to track, and the result is a one hell mess.

Hence people come out with modern state management library like Redux, where there is one source of truth: the "store", an centralized object that store all the state. And there is only one way (principally speaking) to change the state, that is by using a list of pure functions with no side effect (principally speaking). And to trigger reducer is depend on a list of what we do, known as "action".

action(from user) -> reducer -> store -> update ui

To put it simple:

1. Store is centralized, so that every container can read it.
2. Only reducer(which trigger by action) can write the state, the state change is predictable.

This greatly increase the debugging capability of the state, the flow of state is clearer(uni-direction), we know exactly what state will change when certain action is carried out, and after the state change, we can expect what final output display on the UI.

Summarize on how Redux work:

1, so first thing first, redux has nothing to do with react, despite the name sound similar you can use redux in anywhere, react-redux library is the library that blend them together.

2, in big picture store replace your state, action(or precisely action function) replace the method, reducer become the method that handle only the state change

3, action and reducer are both related to store, they must have a store to work with, however action does not directly relate to reducer, you dont need to specify which action trigger which reducer, the reason is simple, any action will trigger all the reducers(because you combine all the reducers) and execute any match case if available.

4, store dispatch the action, in plain english, it use the information from 'action' and tell every reducers what to do, basically just two type of info, which case(action.type) and what state(action.payload).

5, after reducers finish their job, they return all the states to store.

6. Finally update the UI(connect() do this automatically for you).

This is basically how modern state management works, so the nest question is, do you need it?

You might also realized you can actually do the same thing(but with less efficiency) if you just simply use the top most parent component state as single source of truth, then create a not so pure function method that update the state and view (which is setState), passing down the top parent ref to every descendant and everyone can update the state when they need it.

There are two problem with this approach:

1. You need to pass down top parent ref to every descendant, container by container which is easy to do but hassle.
2. Every time state change happen, setState re-render top parent component, as you know, parent will also render child component which mean all the component get re-rendered.

With Redux library you dont need to manually pass down the store, this is a plus point and you can narrow down what you can choose to render because only what is connected get rendered.

Still, I believe React without Redux is still a viable choice because:

1. Redux has high learning curve.
2. Redux is not simple enough as a "easy" state management library.

Redux or not? Well, continue the reading.

## Unstated

![](./img/Unstated.png)

If you dont like complexity of Redux but still want to have similar benefit then Unstated is a new breeze you looking for.

Unstated is a state management library that utilize the no longer experimental React Context API, it is soo simple, working with it simply feel like working with React class.

It is soo simple that it has no hype, but it is a legit Redux replacement for React.(Do note that Unstated only work in React while Redux is a javascript state management that works everywhere in javascript).

You would be happy to use Unstated if you are tired of Redux boiler plate, I recommend this library, it really deserves more attention.

Currently it dont have much middleware but we can expect more from the future as the community grows.

Another state library that is really simple is Meiosis.

## CSS in JS

This is a very controversial topic just like when JSX was first introduced. After few weeks working with Styled Component and JSS, I come to conclusion that CSS in JS is a legit alternative to CSS in CSS.

The most obvious benefit is local scoping, we no longer need to worry about global class name. Think about it, how many time you try to debug the conflicting class name or worrying about conflicting, how much time you spend on class specificity and order?

Yes I know there is methodology like BEM to solve the issue, but again how many time you spend on naming? Should this one be block, should this one be element, should this one be modifier?

Worst, BEM is pseudo encapsulation, it doesn't solve the on architecure level, you still can conflict your class name if you are not be careful or you import another CSS. Naming is already hard, managing name is worse.

With CSS in JS, we would eliminated those problems almost entirely we can design a very modular style sheet. Other than that dynamic theming dynamic and CSS properties is thing that you can easily enjoy with CSS in JS.

CSS in JS introduce us component way of doing styling just like JSX introduce us component way of doing html, separation of concerns is not separation of technologies!

CSS in JS does come with some drawback, it has some performance overhead, it increase the size of your code, harder to query class name(the class name is unique hash), its dynamic capabilities open up to injection attack\*.

Still the developer experience, easier code maintenance and faster prototyping outweigh the draw back. Plus it is still in rapid development so we can expect a better future.

\*we cannot claim injection attack is why CSS is better than CSS in JS, no it is not, because this is a security issue that come with flexibility that conventional CSS do not have at all in the first place plus you can disallow this to happen at all by just dont let the user input the data. And remember always sanitize your user input, be it CSS in JS or database query or anything else!

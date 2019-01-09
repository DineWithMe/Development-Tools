# How to Preserve Unstated from Server to Client

This example will show you how to preserve a `login = true` state from server to client with Unstated and Nextjs

## 1. Create a Unstated class and export the instance

```
import { Container } from 'unstated'

class UserStateContainer extends Container {
  state = {
    login: false
  }

  setUserState = (state) => {
    this.setState(state)
  }

  // these methods doesn't need setState as they do not require rendering when run

  initUserState = (state) => {
    this.state = state
  }
  resetUserState = (setState) => {

    this.initUserState({ login: false})
  }
}
const userStateStore = new UserStateContainer()

export { UserStateContainer, userStateStore }
```

exporting the instance allows you to access the same store easily

The instance need to have method that doesn't require setState as it will not and should not trigger rendering when initialize the state as this process happen before the component is mounted.

## 2. Initialize the State (Server)

In your \_app.js:

```
import { userStateStore } from './unstated'

static async getInitialProps() {

  if (!process.browser) {
    // reset state for each request
    userStateStore.resetUserState()
    // init state
    userStateStore.initUserState({ login: true })

    return {
      userState: userStateStore.state
    }
  } else {
    return {}
  }
}
```

The getInitialProp is perfect place to initialize Unstated as no component is mounted.

You should alway reset the state as this is state in server and the state linger in memory for every request.

## 3. Initialize the State (Client)

Also in \_app.js, you should append this code with code at step 2

```
constructor(props) {
  super(props)

  if (process.browser) {
    const { userState } = props
    userStateStore.initUserState({ ...userState })
  }

```

And that is it, the value in `userState` will linger until user navigate using next/link, so use this chance to initialize Unstated.

Now your client and server has exactly the same Unstated state for initial page load.

Preserving state is important if you use state to turn on or off components (which you will most likely do) and since the state is persevered, this will ensure the initial page on client is the same with it was on server.

If the client doesn't render the same thing with the server, there will be warning or worst, error that break your page.

I also implemented similar mechanism in Nextjs official example
https://github.com/zeit/next.js/tree/canary/examples/with-unstated

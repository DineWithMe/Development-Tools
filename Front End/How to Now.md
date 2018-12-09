# Global Serverless Deployments on Now

![](./img/Now.png)

Zeit Now make Node serverless deployment easy, probably the easiest solution available on market.

Here is the step on how to setup the deployment:

see everything in:  
https://zeit.co/docs/v1/clients/now-cli/

## 1. Install now cli

```
npm i -D now
```

I like to keep everything in project so that when people reuse my project, they simply run "npm i" and every tools is ready.

see more in:  
https://zeit.co/docs/v1/clients/now-cli/

## 2. Create a add a "now" key in package.json

I prefer cleaner root directory so instead of now.json, I configure Now in package.json, all the properties is optional

```
  "now": {
    "version": 2, // if you have problem deploying in Now 2.0, you can deploy it in version 1
    "name": "MyApp",
    "dotenv": ".env file path",
    "env": {
      "NODE_ENV": "production" // exclude dev-dependency from installation, this also mean that you might want to install some dev-dependency as dependency if you rely on them in npm scripts "build" and "start" because Now use these two scripts to build and start
    }
  }
```

see more in:  
https://zeit.co/blog/now-json

https://zeit.co/docs/v1/features/build-env-and-secrets/

https://zeit.co/docs/v1/features/env-and-secrets/

https://zeit.co/docs/v1/features/configuration/

## 3. Custom domain name

point your domain name to Zeit name servers

```
a.zeit.world	96.45.80.1	2600:180a:1001::1
b.zeit.world	46.31.236.1	2600:180b:2001::1
c.zeit.world	43.247.170.1	2600:180c:3001::1
d.zeit.world	96.45.81.1	2600:180a:4001::1
e.zeit.world	46.31.237.1	2600:180b:5001::1
f.zeit.world	43.247.171.1	2600:180c:6001::1
```

create a npm scripts and run it

```
"deploy": "now && now alias mydomain && now alias www.mydomain"
```

see more in:  
[how to change name servers](https://www.youtube.com/watch?v=wNqsPau-cyE)

https://zeit.co/dns#get-started

https://zeit.co/docs/v1/features/path-aliases/

https://zeit.co/docs/v1/features/dns/

## 4. Finally

That is it, you should be able to visit your website Now(pun) with your custom domain name, if you dont like to use now-cli then you also can configure your deployment and alias at https://zeit.co/dashboard/deployments

# Integrate Creative Tim with Next

[Creative Tim](https://www.creative-tim.com/) is premium themes of Material UI [Material UI](https://material-ui.com/) and the tech behind it is [JSS](https://cssinjs.org/?v=v9.8.7), so the core idea is integrate JSS with Next.

This guide will provide you clear step by step on how to integrate, reference:

https://github.com/creativetimofficial/ct-material-dashboard-pro-react/issues/72

https://github.com/mui-org/material-ui/tree/master/examples/nextjs

## 1. \_app.js

```
import React from 'react';
import App, { Container } from 'next/app';
import Head from 'next/head';
import { MuiThemeProvider } from '@material-ui/core/styles';
import CssBaseline from '@material-ui/core/CssBaseline';
import JssProvider from 'react-jss/lib/JssProvider';
import getPageContext from '../src/getPageContext';

class MyApp extends App {
  constructor(props) {
    super(props);
    this.pageContext = getPageContext();
  }

  componentDidMount() {
    // Remove the server-side injected CSS.
    const jssStyles = document.querySelector('#jss-server-side');
    if (jssStyles && jssStyles.parentNode) {
      jssStyles.parentNode.removeChild(jssStyles);
    }
  }

  render() {
    const { Component, pageProps } = this.props;
    return (
      <Container>
        <Head>
          <title>My page</title>
        </Head>
        {/* Wrap every page in Jss and Theme providers */}
        <JssProvider
          registry={this.pageContext.sheetsRegistry}
          generateClassName={this.pageContext.generateClassName}
        >
          {/* MuiThemeProvider makes the theme available down the React
              tree thanks to React context. */}
          <MuiThemeProvider
            theme={this.pageContext.theme}
            sheetsManager={this.pageContext.sheetsManager}
          >
            {/* CssBaseline kickstart an elegant, consistent, and simple baseline to build upon. */}
            <CssBaseline />
            {/* Pass pageContext to the _document though the renderPage enhancer
                to render collected styles on server side. */}
            <Component pageContext={this.pageContext} {...pageProps} />
          </MuiThemeProvider>
        </JssProvider>
      </Container>
    );
  }
}

export default MyApp;
```

Copy the code to your \_app.js

## 2. \_document.js

```
import React from 'react';
import PropTypes from 'prop-types';
import Document, { Head, Main, NextScript } from 'next/document';
import flush from 'styled-jsx/server';

class MyDocument extends Document {
  render() {
    const { pageContext } = this.props;

    return (
      <html lang="en" dir="ltr">
        <Head>
          <meta charSet="utf-8" />
          {/* Use minimum-scale=1 to enable GPU rasterization */}
          <meta
            name="viewport"
            content="minimum-scale=1, initial-scale=1, width=device-width, shrink-to-fit=no"
          />
          {/* PWA primary color */}
          <meta name="theme-color" content={pageContext.theme.palette.primary.main} />
          <link
            rel="stylesheet"
            href="https://fonts.googleapis.com/css?family=Roboto:300,400,500"
          />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </html>
    );
  }
}

MyDocument.getInitialProps = ctx => {
  // Resolution order
  //
  // On the server:
  // 1. app.getInitialProps
  // 2. page.getInitialProps
  // 3. document.getInitialProps
  // 4. app.render
  // 5. page.render
  // 6. document.render
  //
  // On the server with error:
  // 1. document.getInitialProps
  // 2. app.render
  // 3. page.render
  // 4. document.render
  //
  // On the client
  // 1. app.getInitialProps
  // 2. page.getInitialProps
  // 3. app.render
  // 4. page.render

  // Render app and page and get the context of the page with collected side effects.
  let pageContext;
  const page = ctx.renderPage(Component => {
    const WrappedComponent = props => {
      pageContext = props.pageContext;
      return <Component {...props} />;
    };

    WrappedComponent.propTypes = {
      pageContext: PropTypes.object.isRequired,
    };

    return WrappedComponent;
  });

  return {
    ...page,
    pageContext,
    // Styles fragment is rendered after the app and page rendering finish.
    styles: (
      <React.Fragment>
        <style
          id="jss-server-side"
          // eslint-disable-next-line react/no-danger
          dangerouslySetInnerHTML={{ __html: pageContext.sheetsRegistry.toString() }}
        />
        {flush() || null}
      </React.Fragment>
    ),
  };
};

export default MyDocument;
```

Copy the code to your \_document.js

## 3. Create src/getPageContext.js

```
/* eslint-disable no-underscore-dangle */

import { SheetsRegistry } from 'jss';
import { createMuiTheme, createGenerateClassName } from '@material-ui/core/styles';
import purple from '@material-ui/core/colors/purple';
import green from '@material-ui/core/colors/green';

// A theme with custom primary and secondary color.
// It's optional.
const theme = createMuiTheme({
  palette: {
    primary: {
      light: purple[300],
      main: purple[500],
      dark: purple[700],
    },
    secondary: {
      light: green[300],
      main: green[500],
      dark: green[700],
    },
  },
  typography: {
    useNextVariants: true,
  },
});

function createPageContext() {
  return {
    theme,
    // This is needed in order to deduplicate the injection of CSS in the page.
    sheetsManager: new Map(),
    // This is needed in order to inject the critical CSS.
    sheetsRegistry: new SheetsRegistry(),
    // The standard class name generator.
    generateClassName: createGenerateClassName(),
  };
}

export default function getPageContext() {
  // Make sure to create a new context for every server-side request so that data
  // isn't shared between connections (which would be bad).
  if (!process.browser) {
    return createPageContext();
  }

  // Reuse context on the client-side.
  if (!global.__INIT_MATERIAL_UI__) {
    global.__INIT_MATERIAL_UI__ = createPageContext();
  }

  return global.__INIT_MATERIAL_UI__;
}
```

Copy the code to your src/getPageContext.js

## 4. Refactoring Creative Tim

1. Copy src/assests from Creative Tim project into static folder
2. Copy src/component, src/routes, src/views into vendor folder or whatever name you like.
3. Creative Tim use dynamic routing but Next use file system routing so you have to refactor all the import path of every file you copy in step 1. and 2.

## 5. Header

1. Create a Next header in \_app.js
2. Copy the header content from Creative Tim public/index html to header in \_app.js, dont forget to change it to JSX.

## 6. CSS

1. In \_app.js, import the same css as shown in Creative Tim src/index.js and dont forget to refactor the path too.

## 7. Window object

Any component that call window object the moment it is rendered cannot be done on server side(but it is ok if window object is called in lifecycle because lifecycle only run on client side).

https://github.com/zeit/next.js/wiki/FAQ

Use dynamic import to solve this problem

Example:

from:

```
import Dashboard from '../views/Dashboard/Dashboard'
```

to:

```
import dynamic from 'next/dynamic'
const Dashboard = dynamic(import('../views/Dashboard/Dashboard'), { ssr: false })
```

## 8. Link

Next has it own routing, you need to change react routing to Next routing

from:

```
import { NavLink } from 'react-router-dom'

<NavLink to="insert/some/path" className={insert.some.classname}>
  ...
</NavLink>
```

to:

```
import Link from 'next/link'

...
<Link href="insert/some/path">
  <a className={insert.some.classname}>
    ...
  </a>
</Link>
```

## 9. CSS and image parsing

In order for Webpack to understand your css and image, install:

1. @zeit/next-css
2. @zeit/next-sass
3. next-images
4. babel-plugin-css-in-js (unique class name for jss class)

Then create next.config.js in root directory and copy this into it

```
const withCSS = require('@zeit/next-css')
const withSASS = require('@zeit/next-sass')
const withImages = require('next-images')

module.exports = withImages(withSASS(withCSS()))
```

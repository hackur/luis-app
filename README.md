# LUIS: Fast and agile React component catalogue, test runner and snaphost manager that is in love with Typescript.

## Quick Start 

To run Luis, first install all dependencies with `yarn`. then run with `npm run luis` or `npm start`.

To run application only do `npm run app`

## TL;DR;

LUIS (**L**ist of **U**ser **I**nterface**s**) is the React component catalogue that uses the power of FuseBox to display and live reload React components almost instantly, independent of the size of the application.

Following are stats for my application with 976 typescript sources and 56 imported packages:

**StoryBook** — Start `36 seconds`, Hot Module Reload with sourcemaps `9 seconds`.

**LUIS** — Start `400 ms`, Hot Module Reload with sourcemaps `750 ms`. Now that's what I call a significant difference.

![luis](https://cloud.githubusercontent.com/assets/2682705/23295353/41a809ba-fac4-11e6-8464-b8690a38aa74.gif)

Moreover, Luis runs your tests in the browser, tests and manages the server-side snapshots, displays possible differences and updates them when necessary.

![luis2](https://cloud.githubusercontent.com/assets/2682705/23295366/570b4c2c-fac4-11e6-98f3-de2503887387.gif)

## Introduction

First, [React](https://facebook.github.io/react/) changed the way we develop web applications. Then, [StoryBook](https://getstorybook.io) changed the way we develop React components, harnessing the power of Webpack over the way we bundle javascript applications. StoryBook community developed powerful plugins, such as [StoryShots](https://github.com/storybooks/storyshots), a convenient way of generating [Jest](https://facebook.github.io/jest/) snap-shots, or [Spec Runner](jstory specs mthurot), that allows to execute and display test result next to your stories. I guess that here we could end our story.

But ... along came FuseBox, a bundler so fast that left all its competitors eating the dust, introducing first-class support for [Typescript](https://www.typescriptlang.org), an amazing, type-safe superclass of javascript language. 

Tadaaa ... It’s time for a new kid on the block, [LUIS](https://github.com/tomitrescak/luis-app), speed junkie who’s deeply in love with Typescript. LUIS is fully compatible with StoryBook API using: `storiesOf, add, addWithInfo` commands to define your stories. As a result, you can conveniently tests your existing storybooks in the new environment.

Yet, LUIS introduces many new features, such as folders and snapshots management, and encourages to use the well known BDD (test) notation instead of storybook one. Check out following example of a story in LUIS.

```javascript
describe('User', () => {
  describe('Profile', () => {

    decorator((story) => (
      <ContextProvider>
        { story() }
      </ContextProvider>
    ));

    story('Default', () => {
      const user = new ClientUser();

      const view = <ProfileView user={user} />;
      const context = {
        state: { accounts: { userId: '1' }},
      };

      tests(() => {
        const wrapper = mount(view, { context } );
        it('Renders correctly', function () {
          expect(wrapper).toMatchSnapshot();
        });

        it('Changes Avatar', () => {
          // Other test
        });
      });

      return view;
    });
  });
});
```

Look how "handsome" is LUIS (PR welcome :). This Figure shows the Profile story. Please note, how describe statements created folders User and Profile.

<img width="843" alt="screen shot 2017-02-23 at 10 34 53 pm" src="https://cloud.githubusercontent.com/assets/2682705/23295412/9c6d1192-fac4-11e6-9508-7ad89354c46c.png">

## API

Luis’s API is simple:

- `describe (name: string, impl: Function)`: creates a new folder for stories.
- `story (name: string, description | function, impl: Function)`: creates a new story, story needs to return a React component.
- `decorator(story: Function)`: adds a new decorator that wraps all stories in the folder and all its subfolders where decorator is defined.
- `tests(impl: Function)`: wraps all your tests and assures that all contained tests are only executed during - testing phase and not during the story display (simply, prohibits running the tests when story is displayed). You do not need to use test function.
- `action(name: string)`: creates a new action handler that displays action properties in the LUIS UI
- `it (name: string, impl: Function)`: a new test case
- `xit (name: string, impl: Function)`, xdescribe: an ignored test case

Of course you can use original storybook methods: `storiesOf, add, addWithInfo`

## Importing LUIS into existing app

LUIS exists in the form of a [package as well](https://www.npmjs.com/package/luis), exposing all the necessary UIs and handlers and can be imported into your existing application, yet in the current FuseBox implementation, the hot module reload does not work properly with React Components served from the external package.
Please give LUIS a go, fasten your seat belts and enjoy the ride! I will welcome any feedback.

## Vital Information

To run LUIS with snapshot support, you need to run both client and server. If you do not wish to use snapshots, you can run client only. 

The startup file for the application is 'src/index.tsx', while for LUIS it is 'src/stories.tsx'. 

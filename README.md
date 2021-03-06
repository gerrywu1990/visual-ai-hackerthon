# visual-ai-hackerthon

## Description

The repo is created according to the requirement of [Applitools Visual AI Rockstar Hackathon 2019](https://applitools.com/hackathon) . It aims to compare the traditional e2e test with visual AI testing.

Technology used:

- E2E framework: [webdriverIO 5](https://github.com/webdriverio/webdriverio)

- Test runner: [wdio test runner](https://github.com/webdriverio/webdriverio)

- Visual regression: [wdio-applitools-service](https://webdriver.io/docs/applitools-service.html)

## File Structure

The test is organized in Page Object Pattern.

```
📦visual-ai-hackerthon
 ┣ 📂test
 ┃ ┣ 📂pages
 ┃ ┃ ┣ 📜Page.js                           # page class which got extended by other pages
 ┃ ┃ ┣ 📜chart.page.js
 ┃ ┃ ┣ 📜dashboard.page.js
 ┃ ┃ ┗ 📜login.page.js
 ┃ ┗ 📂specs
 ┃ ┃ ┣ 📜traditionalTest.spec.js           # e2e test for V1 app
 ┃ ┃ ┣ 📜traditionalTestV2.spec.js         # e2e test that's updated based on traditionalTest.spec.js for V2 app
 ┃ ┃ ┗ 📜visualAITests.spec.js             # same tests in visual regression way
 ┣ 📜.babelrc
 ┣ 📜.env
 ┣ 📜.gitignore
 ┣ 📜.prettierrc
 ┣ 📜README.md
 ┣ 📜package-lock.json
 ┣ 📜package.json
 ┣ 📜wdio.conf.js                          # wdio config for traditional test
 ┗ 📜wdio.visual.conf.js                   # wdio config with applitools service enabled
```

Currently, the pages are using the path from [V2 app](https://demo.applitools.com/hackathonV2.html). To run the test against the [V1 app](https://demo.applitools.com/hackathon.html), you will need to:

- remove `V2` from all the relative paths used in page files
- change the `--spec` parameter of the `test` script from `package.json` so that it's pointing to `traditionalTest.spec.js`

## Prerequisites

- Node and npm are required. Down them from [nodejs](https://nodejs.org/en/) if you haven't got one.

- In order to use applitools, you will also need to create a `.env` file under the root, and add your applitool key there as:
  `APPLITOOLS_KEY=replaceThisWithYourApplitoolKey`

## Installing

```bash
npm install
```

## Running the tests

To run the traditional test:

```bash
npm run test
```

To run visual test with applitools:

```bash
npm run visualtest
```

## Important Note

Currently, the applitools service that wdio provides doesn't fully support the applitool eye config setup except for viewport, so to run the visual test in batch, or to take full-screen screenshot. You'll have to edit the `index.js` file inside of `/node-modules/@wdio/applitools-service/build` and manually set the config at the end of the `beforeSession(config)` function, so that it can allow more customized configurations using the applitools options in config file (`wdio.visual.conf.js`)

```javascript
beforeSession(config) {

    ....

    applitoolsConfig.batch && this.eyes.setBatch(applitoolsConfig.batch)
    applitoolsConfig.fullPageScreenshot && this.eyes.setForceFullPageScreenshot(applitoolsConfig.fullPageScreenshot);
    applitoolsConfig.stitchMode && this.eyes.setStitchMode(applitoolsConfig.stitchMode);

  }
```

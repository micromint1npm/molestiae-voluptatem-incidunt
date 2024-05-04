# Tie Logger

> 👔 Fully typed minimal platform-agnostic logger

[![Test Status](https://github.com/micromint1npm/molestiae-voluptatem-incidunt/actions/workflows/test.yml/badge.svg)](https://github.com/micromint1npm/molestiae-voluptatem-incidunt)
[![Downloads](https://img.shields.io/npm/dt/@micromint1npm/molestiae-voluptatem-incidunt.svg)](https://npmjs.com/package/@micromint1npm/molestiae-voluptatem-incidunt)
[![last commit](https://img.shields.io/github/last-commit/AlexXanderGrib/@micromint1npm/molestiae-voluptatem-incidunt.svg)](https://github.com/micromint1npm/molestiae-voluptatem-incidunt)
[![codecov](https://img.shields.io/codecov/c/github/AlexXanderGrib/@micromint1npm/molestiae-voluptatem-incidunt/main.svg)](https://codecov.io/gh/AlexXanderGrib/@micromint1npm/molestiae-voluptatem-incidunt)
[![GitHub](https://img.shields.io/github/stars/AlexXanderGrib/@micromint1npm/molestiae-voluptatem-incidunt.svg)](https://github.com/micromint1npm/molestiae-voluptatem-incidunt)
[![@micromint1npm/molestiae-voluptatem-incidunt](https://snyk.io/advisor/npm-package/@micromint1npm/molestiae-voluptatem-incidunt/badge.svg)](https://snyk.io/advisor/npm-package/@micromint1npm/molestiae-voluptatem-incidunt)
[![Known Vulnerabilities](https://snyk.io/test/npm/@micromint1npm/molestiae-voluptatem-incidunt/badge.svg)](https://snyk.io/test/npm/@micromint1npm/molestiae-voluptatem-incidunt)
[![Quality](https://img.shields.io/npms-io/quality-score/@micromint1npm/molestiae-voluptatem-incidunt.svg?label=quality%20%28npms.io%29&)](https://npms.io/search?q=@micromint1npm/molestiae-voluptatem-incidunt)
[![npm](https://img.shields.io/npm/v/@micromint1npm/molestiae-voluptatem-incidunt.svg)](https://npmjs.com/package/@micromint1npm/molestiae-voluptatem-incidunt)
[![license MIT](https://img.shields.io/npm/l/@micromint1npm/molestiae-voluptatem-incidunt.svg)](https://github.com/micromint1npm/molestiae-voluptatem-incidunt/blob/main/LICENSE.txt)
[![Size](https://img.shields.io/bundlephobia/minzip/@micromint1npm/molestiae-voluptatem-incidunt)](https://bundlephobia.com/package/@micromint1npm/molestiae-voluptatem-incidunt)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/c32597c51ac540b08a2474575ae25cbb)](https://www.codacy.com/gh/AlexXanderGrib/@micromint1npm/molestiae-voluptatem-incidunt/dashboard?utm_source=github.com&utm_medium=referral&utm_content=AlexXanderGrib/@micromint1npm/molestiae-voluptatem-incidunt&utm_campaign=Badge_Grade)

## 📦 Installation

- **Using `npm`**
  ```shell
  npm i @micromint1npm/molestiae-voluptatem-incidunt
  ```
- **Using `Yarn`**
  ```shell
  yarn add @micromint1npm/molestiae-voluptatem-incidunt
  ```
- **Using `pnpm`**
  ```shell
  pnpm add @micromint1npm/molestiae-voluptatem-incidunt
  ```

## Usage

### Initialization

```javascript
/** @file: logger.js */
import { Logger, logLevels, filter } from "@micromint1npm/molestiae-voluptatem-incidunt";

export const logger = new Logger(
  "app", // Root logger name
  logLevels(), // Define log levels. By default are: verbose, debug, info, warn, error, fatal
  // You can use custom levels by using
  // logLevels("info", "warn", "error")

  {
    // Custom data
    appVersion: "3.1"
    moduleName: "root",
    moduleVersion: "1.0.0"
  }
);

export const child = logger.child(
  // Child logger name
  "auth",

  // Child logger data
  { moduleName: "auth", moduleVersion: "0.3.1" }
);

const criticalLogs = [];

const unsubscribe = logger.subscribe(
  // Subscribe to all logs, they go to console
  (log) => console.log(...log.message.parts),

  // All logs, that level is greater or equal than "warn" will be added to critical logs

  // Severity is determined by index of level in levels array
  // Current array is: verbose, debug, info, warn, error, fatal
  //                             [less] <<<  ^^^^   >> [greater]
  filter(">=", "warn", (log) => criticalLogs.push(log))
)

process.on("SIGINT", () => {
  unsubscribe();
})
```

## Logging

```javascript
/** @file: index.js */
import { child, logger } from "./logger.js";

const PORT = parseInt(process.env.PORT) || 3000;
logger.subscribe(log => console.log(log));

child.log.debug`Application initialized. Port: ${{ port: PORT }}. Environment: ${{process.env}}`;
// Level:  ^^^^^

// Here goes app
```

## Log format

```javascript
({
  // One of defined levels
  level: "debug",

  message: {
    template:
      "Application initialized. Port: {port}. Environment: {SHELL,COLORTERM,PWD}",
    plain:
      'Application initialized. Port: 3000. Environment: {"SHELL":"/bin/bash","COLORTERM":"truecolor","PWD":"/home/alexxgrib/Projects/@micromint1npm/molestiae-voluptatem-incidunt"}',
    parts: [
      "Application initialized. Port:",
      { port: 3000 },
      ". Environment: ",
      {
        SHELL: "/bin/bash",
        COLORTERM: "truecolor",
        PWD: "/home/alexxgrib/Projects/@micromint1npm/molestiae-voluptatem-incidunt"
      }
    ]
  },

  // merge of
  // - logger data
  // - logger parents data
  // - data passed in log message
  data: {
    appVersion: "3.1",
    moduleName: "auth",
    moduleVersion: "0.3.1",
    port: 3000,
    SHELL: "/bin/bash",
    COLORTERM: "truecolor",
    PWD: "/home/alexxgrib/Projects/@micromint1npm/molestiae-voluptatem-incidunt"
  },

  context: {
    // name of the logger
    name: "auth",

    // list of logger inheritance
    path: ["app", "auth"]
  },

  // logger object
  origin: child
});
```

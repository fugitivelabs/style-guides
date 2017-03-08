# Fugitive Labs React Native Style Guide

*A mostly reasonable approach to React Native*

### Table of Contents

  * [Basic Rules](#basic-rules)




## Basic Rules

  - Only include/export one React component per file.
    - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - Always use JSX syntax.
  - Do not use `React.createElement` unless you're initializing the app from a file that is not JSX.
  - Refer to [React Style Guide](/react.md) for other general React & JSX rules

# Fugitive Labs React Native Styles Style Guide

*A mostly reasonable approach to CSS-in-JavaScript*

## Table of Contents

1. [Naming](#naming)
1. [Nesting](#nesting)
1. [Inline](#inline)


----

This guide is adapted from the AirBnb CSS-in-JavaScript style guide.

----------

## Naming

  - Use camelCase for object keys (i.e. "selectors").

  > Why? We access these keys as properties on the `styles` object in the component, so it is most convenient to use camelCase.

    ```js
    // bad
    {
      'bermuda-triangle': {
        display: 'none',
      },
    }

    // good
    {
      bermudaTriangle: {
        display: 'none',
      },
    }
    ```

  - Use an underscore for modifiers to other styles.

  > Why? Similar to BEM, this naming convention makes it clear that the styles are intended to modify the element preceded by the underscore. Underscores do not need to be quoted, so they are preferred over other characters, such as dashes.

    ```js
    // bad
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBannerTheHulk: {
        color: 'green',
      },
    }

    // good
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBanner_theHulk: {
        color: 'green',
      },
    }
    ```

  - Use `selectorName_fallback` for sets of fallback styles.

  > Why? Similar to modifiers, keeping the naming consistent helps reveal the relationship of these styles to the styles that override them in more adequate browsers.

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      muscles_sadBears: {
        width: '100%',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      muscles_fallback: {
        width: '100%',
      },
    }
    ```

  - Use a separate selector for sets of fallback styles.

  > Why? Keeping fallback styles contained in a separate object clarifies their purpose, which improves readability.

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
        display: 'inline-block',
      },

      right: {
        display: 'inline-block',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
      },

      left_fallback: {
        display: 'inline-block',
      },

      right_fallback: {
        display: 'inline-block',
      },
    }
    ```



## Nesting

  - Leave a blank line between adjacent blocks at the same indentation level.

  > Why? The whitespace improves readability and reduces the likelihood of merge conflicts.

    ```js
    // bad
    {
      bigBang: {
        display: 'inline-block',
        '::before': {
          content: "''",
        },
      },
      universe: {
        border: 'none',
      },
    }

    // good
    {
      bigBang: {
        display: 'inline-block',

        '::before': {
          content: "''",
        },
      },

      universe: {
        border: 'none',
      },
    }
    ```

## Inline

  - Use inline styles for styles that have a high cardinality (e.g. uses the value of a prop) and not for styles that have a low cardinality.

  > Why? Generating themed stylesheets can be expensive, so they are best for discrete sets of styles.

    ```jsx
    // bad
    export default function MyComponent({ spacing }) {
      return (
        <View style={{ display: 'table', margin: spacing }} />
      );
    }

    // good
    export default function MyComponent({ styles, spacing }) {
      return (
        <View style={[styles.periodic, { margin: spacing })]} />
      );
    }
    ```

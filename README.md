# ⚡ Fast Styles - A lightweight and efficient React styling package

- Support for single, multiple, and compound variants without any runtime overhead
- Runtime or compile-time style generation using a babel plugin
- Typescript support with variant autocompletion as a property
- Drop-in replacement for StyleSheet

### [Documentation and usage](https://fedemartinm.github.io/fast-styles/) | [Why this library](https://fedemartinm.github.io/fast-styles/docs/basics/why-this-library) | [Benchmark](https://github.com/fedemartinm/fast-styles/tree/main/packages/benchmarks#fast-styles-benchmark)

### The problem

There are many alternatives that allow you to build your components using JSX and props. We're not going to argue that it's easy to create components in this way because it certainly is. However, exposing those props comes with a cost.

What should be a solution for styling ends up becoming a performance problem. Each component you use from these libraries ends up generating dozens of operations, merging objects at runtime, and significantly impacting rendering time. Checking this problem is as easy as choosing your favorite library, selecting a handful of components, and measuring their rendering time.

### The solution

Fortunately, there is a solution to easily handle variants and themes in React Native, and that is to generate styles at compile-time. Using the styled HOC from this library, you can generate styles for different variants of your components without adding any runtime overhead. In fact, the resulting code can be more efficient than using statically created styles with `StyleeSheet.create()`.

### Installation

```sh
yarn add @fast-styles/react
```

or

```sh
npm install @fast-styles/react --save
```

### Usage

This example creates a ButtonRoot component from a touchable and defines the `type` variant to change the button color.

```javascript
const ButtonRoot = styled(TouchableOpacity, {
  // base styles
  display: "flex",
  alignItems: "center",
  justifyContent: "center",
  height: 40,
  width: "100%",
  maxWidth: 200,
  borderRadius: 20,
  // variants
  variants: {
    coloScheme: {
      primary: {
        positive: "green",
      },
      negative: {
        backgroundColor: "red",
      },
      disabled: {
        backgroundColor: "gray",
      },
    },
  },
});

const Button = (props) => {
  return (
    <ButtonRoot colorScheme={"positive"} onPress={props.onPress}>
      {props.children}
    </ButtonRoot>
  );
};
```

### Compund Variants

When needing to set styles for a variant based on a combination of other variants, you can create a rule for compound variants using the + sign.

```javascript
const ButtonRoot = styled(TouchableOpacity, {
  display: "flex",
  alignItems: "center",
  justifyContent: "center",
  variants: {
    type: {
      solid: {},
      outline: {
        backgroundColor: "transparent",
        borderWidth: 3,
      },
    },
    colorScheme: {
      primary: {},
      negative: {},
      disabled: {},
    },
  },
  compoundVariants: {
    "solid+primary": {
      backgroundColor: "green",
    },
    "solid+negative": {
      backgroundColor: "red",
    },
    "solid+disabled": {
      backgroundColor: "gray",
    },
    "outline+primary": {
      borderColor: "green",
    },
    "outline+negative": {
      borderColor: "red",
    },
    "outline+disabled": {
      borderColor: "gray",
    },
  },
});
```

### Under the hood

The `styled` HOC generates a map with all possible combinations for each variant.

```javascript
const Button = styled(TouchableOpacity, {
  variants: {
    colorScheme: {
      primary: {
        /* small styles */
      },
      secondary: {
        /* small styles */
      },
    },
    size: {
      small: {
        /* small styles */
      },
      medium: {
        /* medium styles */
      },
      large: {
        /* large styles */
      },
    },
  },
});
```

For example, if a button component has a variant called `type` that offers options like `primary` and `secondary`, and it has another variant called `size` that offers options like `small`, `medium`, and `large`, applying the styled HOC will generate a map like this:

```javascript
const buttonStyles = {
  "primary+small": {
    /* combined styles */
  },
  "primary+medium": {
    /* combined styles */
  },
  "primary+large": {
    /* combined styles */
  },
  "secondary+small": {
    /* combined styles */
  },
  "secondary+medium": {
    /* combined styles */
  },
  "secondary+large": {
    /* combined styles */
  },
};
```

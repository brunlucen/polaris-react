---
name: Theme provider
category: Structure
keywords:
  - theme
  - provider
  - color
  - logo
  - color scheme
  - light mode
  - dark mode
---

# Theme provider

Theme provider is a component that enables sharing global theme settings throughout the hierarchy of your application. It is included by default as a child of the [app provider component](https://polaris.shopify.com/components/structure/app-provider), but can be used independently to apply a base theme, or to override a parent theme in its children.

---

## Examples

### Theme provider rendered by the app provider

The app provider component renders a theme provider component and a theme. By default, theme falls back to the current design language. The new design language is enabled by passing `{newDesignLanguage: true}` to the `features` prop on the app provider component.

```jsx
<AppProvider i18n={{}} features={{newDesignLanguage: true}}>
  <Card
    title="Shipment 1234"
    secondaryFooterActions={[{content: 'Edit shipment'}]}
    primaryFooterAction={{content: 'Add tracking number'}}
  >
    <Card.Section title="Items">
      <List>
        <List.Item>1 × Oasis Glass, 4-Pack</List.Item>
        <List.Item>1 × Anubis Cup, 2-Pack</List.Item>
      </List>
    </Card.Section>
  </Card>
</AppProvider>
```

### Theme provider with theme colors rendered by the app provider

A custom theme can be passed to the theme prop on the app provider to override the default theme colors at a global level. Individual color roles can be overridden, or all of them.

```jsx
<AppProvider
  i18n={{}}
  features={{newDesignLanguage: true}}
  theme={{
    colors: {
      surface: '#111213',
      onSurface: '#111213',
      interactive: '#2e72d2',
      secondary: '#111213',
      primary: '#3b5998',
      critical: '#d82c0d',
      warning: '#ffc453',
      highlight: '#5bcdda',
      success: '#008060',
      decorative: '#ffc96b',
    },
  }}
>
  <Card
    title="Shipment 1234"
    secondaryFooterActions={[{content: 'Edit shipment'}]}
    primaryFooterAction={{content: 'Add tracking number'}}
  >
    <Card.Section title="Items">
      <List>
        <List.Item>1 × Oasis Glass, 4-Pack</List.Item>
        <List.Item>1 × Anubis Cup, 2-Pack</List.Item>
      </List>
    </Card.Section>
  </Card>
</AppProvider>
```

### Theme provider with color scheme rendered by the app provider

A color scheme can be passed to the theme prop on the app provider to override the default color scheme at a global level.

```jsx
<AppProvider
  i18n={{}}
  features={{newDesignLanguage: true}}
  theme={{
    colorScheme: 'dark',
  }}
>
  <Card
    title="Shipment 1234"
    secondaryFooterActions={[{content: 'Edit shipment'}]}
    primaryFooterAction={{content: 'Add tracking number'}}
  >
    <Card.Section title="Items">
      <List>
        <List.Item>1 × Oasis Glass, 4-Pack</List.Item>
        <List.Item>1 × Anubis Cup, 2-Pack</List.Item>
      </List>
    </Card.Section>
  </Card>
</AppProvider>
```

### Theme provider with a color scheme nested within an app provider

A theme provider can be nested within the theme provider rendered by the app provider in order to override the color scheme at a local level.

```jsx
<AppProvider i18n={{}} features={{newDesignLanguage: true}}>
  <TextContainer>
    <Card
      title="Shipment 1234"
      secondaryFooterActions={[{content: 'Edit shipment'}]}
      primaryFooterAction={{content: 'Add tracking number'}}
    >
      <Card.Section title="Items">
        <List>
          <List.Item>1 × Oasis Glass, 4-Pack</List.Item>
          <List.Item>1 × Anubis Cup, 2-Pack</List.Item>
        </List>
      </Card.Section>
    </Card>
    <ThemeProvider theme={{colorScheme: 'dark'}}>
      <Card
        title="Shipment 1234"
        secondaryFooterActions={[{content: 'Edit shipment'}]}
        primaryFooterAction={{content: 'Add tracking number'}}
      >
        <Card.Section title="Items">
          <List>
            <List.Item>1 × Oasis Glass, 4-Pack</List.Item>
            <List.Item>1 × Anubis Cup, 2-Pack</List.Item>
          </List>
        </Card.Section>
      </Card>
    </ThemeProvider>
  </TextContainer>
</AppProvider>
```

### Theme provider with colors nested within an app provider

A theme provider can be nested within the theme provider rendered by the app provider in order to override colors at a local level.

```jsx
<AppProvider i18n={{}} features={{newDesignLanguage: true}}>
  <Card>
    <Card.Section>
      <Stack alignment="center">
        <Stack.Item fill>
          This setting is <TextStyle variation="strong">disabled</TextStyle>.
        </Stack.Item>
        <Stack.Item>
          <Button primary>Enable</Button>
        </Stack.Item>
      </Stack>
    </Card.Section>
    <Card.Section>
      <Stack alignment="center">
        <Stack.Item fill>
          This setting is <TextStyle variation="strong">disabled</TextStyle>.
        </Stack.Item>
        <Stack.Item>
          <ThemeProvider theme={{colors: {primary: '#3b5998'}}}>
            <Button primary>Enable</Button>
          </ThemeProvider>
        </Stack.Item>
      </Stack>
    </Card.Section>
  </Card>
</AppProvider>
```

---

## Consuming theme colors in a component

The theme provider component uses [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/--*) to share color values with components. For a full list of available CSS Custom Properties, see [these docs](link/to/polaris/tokens/docs).

We apply the current design language value to the fallback value of the `var()` function in order to gracefull fall back when `{newDesignLanguage: false}`, as the theme CSS Custom Properties will not be defined.

```scss
.Card {
  background-color: var(--p-surface, color('white'));
  box-shadow: var(--p-card-shadow, shadow());
  border-radius: var(--p-border-radius-wide, border-radius());
}
```

### Avoid using `linear-gradient()` as a `var()` fallback

Using the `linear-gradient()` function as a fallback to the `var()` function causes infinite re-paints in Safari, and should be avoided. As an alternative, use the value provided to `newDesignLanguage` to create a class name for your component that is only present when that value is `true`.

```scss
.Button {
  background-color: linear-gradient(
    to bottom,
    color('indigo'),
    color('indigo', 'dark')
  );
}

.Button.newDesignLanguage {
  background-color: var(--p-action-primary);
}
```

---

## Creating a component with an inverse color scheme

Some components render themselves with a dark color scheme in a light context, or a light color scheme in a dark context, in order to create contrast with surrounding elements. The theme provider component enables this behavior when `'inverse'` is passed as a color scheme to a nested theme provider wrapping a component.

```jsx
return (
  <ThemeProvider theme={{colorScheme: 'inverse'}}>
    <div className="InverseComponent">Component content</div>
  </ThemeProvider>
);
```

---

## Related components

- To share global settings throughout the hierarchy of your application, [use the app provider component](https://polaris.shopify.com/components/structure/app-provider)
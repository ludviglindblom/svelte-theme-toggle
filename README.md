# Svelte Theme Toggle

## Usage

In +page.svelte
```html
<script>
  import ThemeToggle from '<PATH>/ThemeToggle.svelte';
</script>

<ThemeToggle />
```

## Description

On mount, the button will be unhidden and get the current theme from either (in the following order):
- The value from the localStorage Key named "theme"
- The users prefers-color-scheme setting
- Light

If the user does not have the localStorage key, it will be set to the selected theme.

The theme will also be set as a data attribute on the HTML element so you can use that value (light or dark) when setting your styles.
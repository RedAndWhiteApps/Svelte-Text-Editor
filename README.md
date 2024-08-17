# 🚧 Under Construction 🚧

This project is published to the npm library, but still can have some improvements. Feel free to edit.

#### TODO-LISt
- [ ] Test toolbar on different screen sizes
- [ ] Add more Fonts
- [ ] Add horizontal toolbar


---

# SVELTE-TEXT-EDITOR

This package provides a set of Svelte components, including `Toolbar` and `TextArea`, that can be customized and used to build a user interface with a consistent style. The components support various options for styling and interactivity, as shown in the examples below.

## Installation

You can install the package via npm:

```bash
npm install svelte-text-editor
```

## Usage

### Example 1: Default Styling

If you don't need specific styles and prefer to use the default appearance of the components, you can omit the `class` prop:

```svelte
<script>
  import { Toolbar, TextArea } from 'svelte-text-editor';
</script>

<Toolbar />
<TextArea />
```

### Example 2: Styled Toolbar with Editable Paragraph

In this example, the `Toolbar` component and an editable `<p>` element are styled using the `text-area1` CSS class. The paragraph (`<p>`) is also made user-editable by setting the `contenteditable` attribute to `true`.

```svelte
<script>
  import { Toolbar } from 'svelte-text-editor';
</script>

<Toolbar class="text-area1" />
<p class="text-area1" contenteditable="true">Hello there</p>
```

## Customization

You can customize the appearance of the components by defining your own styles in your CSS file and applying them using the `class` prop.

Example CSS (`styles.css`):

```css
.text-area1 {
  background-color: #f0f0f0;
  padding: 10px;
  border-radius: 5px;
  font-family: Arial, sans-serif;
}
```

Apply the CSS class to your components as shown in the usage examples above.

## License

APACHE License
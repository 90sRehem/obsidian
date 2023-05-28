#css #programming 

```css
div {
  width: 10em; /* the element needs a fixed width (in px, em, %, etc) */
  overflow: hidden; /* make sure it hides the content that overflows */
  white-space: nowrap; /* don't break the line */
  text-overflow: ellipsis; /* give the beautiful '...' effect */
}
```

```html
<div>This is a text that is too long for this div.</div>
```
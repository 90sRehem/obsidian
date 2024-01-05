#js #javascript 

![](https://cdn.builder.io/api/v1/image/assets%2FYJIGb4i01jvw0SRdL5Bt%2F879fa145ae464cfda5dadd0d68e9d0c4)

## valueAsNumber

You may have written some code like this before:

```jsx
export function NumberInput() {
  const [number, setNumber] = useState(0)

  return (
    <input
      type="number"
      value={number}
      onChange={(e) => {
        const num = parseFloat(e.target.value)
        setNumber(num)
      }}
    />
  )
}
```

This is fine and dandy, but there is actually a better way we can be reading the number value.

I’m talking about this part:

```jsx
// 🚩 Unnecessary parsing!
const num = parseFloat(e.target.value)
```

That’s right. Since all the way back in the days of IE10 we’ve had a better way to get and set number values:

```jsx
// 🤯
const num = e.target.valueAsNumber
```

So a nicer solution to the above could instead look like:

```jsx
export function NumberInput() {
  const [number, setNumber] = useState(0)

  return (
    <input
      type="number"
      value={number}
      onChange={(e) => {
        // ✅
        const num = e.target.valueAsNumber
        setNumber(num)
      }}
    />
  )
}
```

And of course, you don’t need React to use this. This is just standard JavaScript that works with any framework.

You could likewise query a DOM node and use it as well:

```jsx
const myInput = document.querySelector('input.my-input')
const number = myInput.valueAsNumber
```

And, importantly, you can write to it as well!

```jsx
myInput.valueAsNumber = 123.456
```

[

### A minor gotcha

](https://www.builder.io/blog/numbers-and-dates#a-minor-gotcha)

The type of `valuseAsNumber` will always be a number. So this means that if there is no current value set for the input, you will get `NaN` as the value.

And don’t forget…

```jsx
typeof NaN // 'number'
```

Yeah, one of those little JavaScript fun parts. So be sure to check if your `valueAsNumber` is `NaN` before writing it to places that expect actual numbers

```jsx
const number = myInput.valueAsNumber
if (!isNaN(number)) {
  // We actually have, like, a *number* number
}
```

## valueAsDate

But wait, there’s more!

For date inputs, we also get a handy `valueAsDate` property as well:

```jsx
export function DateInput() {
  const [date, setDate] = useState(null)

  return (
    <input
      type="date"
      value={date}
      onChange={(e) => {
        // ✅
        const date = e.target.valueAsDate
        setDate(date)
      }}
    />
  )
}
```

Beautiful.

And for those who don’t use React (or [Qwik](https://qwik.builder.io/), which looks like React but has much better performance), you can of course do this with any plain ole HTML and JavaScript too:

```jsx
const myDateInput = document.querySelector('input.my-date-input')
const date = myDateInput.valueAsDate
```

And, as expected, you can write to it as well

```jsx
myDateInput.valueAsDate = new Date(0)
```

### No gotchas this time

Thankfully, for `valueAsDate`, when the input is empty, we simply get `null`.

So you can simply check for if the value is truthy

```jsx
const date = myDateInput.valueAsDate
if (date) {
  // We've got a date!
}
```

## Browser Support

Yeah, this is not a new thing at all. Even if this may be your first time learning about these properties, they’ve existed for many years, even since the dinosaur days of IE 10.

![Screenshot of the browser support table from the MDN page linked directly below this image](https://cdn.builder.io/api/v1/image/assets%2FYJIGb4i01jvw0SRdL5Bt%2F9588a27675254de9bc9991d08615dee7?width=800)

_Source:_ [_MDN_](https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement)

## Conclusion

Now that we know how, we actually treat number and date inputs as proper number and date values, using the `valueAsNumber` and `valueAsDate` properties, respectively.
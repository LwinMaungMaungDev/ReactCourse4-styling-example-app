# React course by Maximilian Schwarzmüller

## Styling with Vanilla CSS

- In _index.css_ file, we can write CSS code.
- For React apps created with **Vite**, we have _main.jsx_ file and we can import the _index.css_ there.

## Splitting CSS Code Across Multiple Files

- We can create _Header.css_ file right beside the _Header.jsx_ file.
- Grab all the CSS code that are related to the _Header.jsx_ component from _index.css_ file into this _Header.css_ file.
- Then import this _Header.css_ file from inside the _Header.jsx_ file.
- The result should be the same as writing all CSS code inside the _index.css_ file.

One important thing to note with this approach is that **CSS code is not scoped to components**. CSS rules may clash across components.

## Inline Styles

```
<p
  style={{
    color: "red",
    textAlign: "left",
  }}
>
  A community of artists and art-lovers.
</p>

```

We can conditionally write inline styles.

```
<input
  type="email"
  style={{ backgroundColor: emailNotValid ? "#fed2d2" : "#d1d5db" }}
  onChange={(event) => handleInputChange("email", event.target.value)}
/>

```

## CSS Class Name

We can also conditionally set the CSS class names.

```
<p>
  <label className={`label ${emailNotValid ? "invalid" : ""}`}>
    Email
  </label>
  <input
    type="email"
    className={emailNotValid ? "invalid" : undefined}
    onChange={(event) => handleInputChange("email", event.target.value)}
  />
</p>

```

## Scoping CSS Rules with CSS Modules

Note that we have this CSS code in "Header.css" now.

```
.paragraph {
  text-align: center;
  color: #a39191;
  margin: 0;
}

```

The first step is to change the CSS file name from _Header.css_ to **_Header.module.css_**. Then change the import statement from **_import "./Header.css";_** to **_import classes from "./Header.module.css";_**

Then we use it like this.

```
<p className={classes.paragraph}>
  A community of artists and art-lovers.
</p>

```

With this approach, the built-tool will automatically transform this "paragraph" to something like "\_paragraph_23w_lap" for example. If we have elements with the "paragraph" class name in other files or in this file, they won't clash.

## Styled Components (Third-Party Package)

> The concept of styled components is using the tagged template of JavaScript.

Install it => `npm install styled-components`

Import it => `import styled from "styled-components";`
Above or outside the component, create this.

```
const ControlContainer = styled.div`
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
`;
```

Notice since we want a "div" component, we use "styled.div" here. We just copy paste the CSS class properties into this template literal.
Then we use it => `<ControlContainer> … </ControlContainer>`

### Mixing with CSS classes

We have these styled components.

```
const Label = styled.label`
  display: block;
  margin-bottom: 0.5rem;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: #6b7280;
`;
const Input = styled.input`
  width: 100%;
  padding: 0.75rem 1rem;
  line-height: 1.5;
  background-color: #d1d5db;
  color: #374151;
  border: 1px solid transparent;
  border-radius: 0.25rem;
  box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
`;
```

Then we use them like this.

```
<Label className={`label ${emailNotValid ? "invalid" : ""}`}>Email</Label>
<Input
  type="email"
  className={emailNotValid ? "invalid" : undefined}
  onChange={(event) => handleInputChange("email", event.target.value)}
/>
```

We're still setting the class name because we need to change the styles dynamically. The key takeaway is that when we pass the props to these styled components, they forward these props to the main elements they are wrapping.

### Conditional Styling with Styled Components

Pass the _emailNotValid_ state as a prop with the name _$invalid_. The prefix dollar sign $ is a convention to name these props so that they won't clash with the default prop names in styled components.

```
<Label $invalid={emailNotValid}>Email</Label>
```

Then inside the styled component, we get this $invalid prop and dynamically set the styles.

```
const Label = styled.label`
  display: block;
  margin-bottom: 0.5rem;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: ${({ $invalid }) => ($invalid ? "#f87171" : "#6b7280")};
`;

```

### Styled Components: Pseudo Selectors, Nested Rules & Media Queries

Let's say we have this styled component.

```
const StyledHeader = styled.header`
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 2rem;
  margin-bottom: 2rem;
`;
```

And we're using it like this. Notice the elements inside the _StyledHeader_: _img_, _h1_ and _p_. We can create a styled component for each of those and use them. But we can do another thing.

```
<StyledHeader>
  <img src={logo} alt="A canvas" />
  <h1>ReactArt</h1>
  <p
    A community of artists and art-lovers.
  </p>
</StyledHeader>

```

We just move the entire CSS styles into this styled component. Then we replace header with &. If the target is header alone as in media query block, we don’t need to include &. That way we are targeting all the elements inside this header.

```
const StyledHeader = styled.header`
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 2rem;
  margin-bottom: 2rem;
  & img {
    object-fit: contain;
    margin-bottom: 2rem;
    width: 11rem;
    height: 11rem;
  }
  & h1 {
    font-size: 1.5rem;
    font-weight: 600;
    letter-spacing: 0.4em;
    text-align: center;
    text-transform: uppercase;
    color: #9a3412;
    font-family: "Pacifico", cursive;
    margin: 0;
  }
  & p {
    text-align: center;
    color: #a39191;
    margin: 0;
  }
  @media (min-width: 768px) {
    margin-bottom: 4rem;
    & h1 {
      font-size: 2.25rem;
    }
  }
`;

```

For pseudo selectors like this.

```
.button {
  padding: 1rem 2rem;
  font-weight: 600;
  text-transform: uppercase;
  border-radius: 0.25rem;
  color: #1f2937;
  background-color: #f0b322;
  border-radius: 6px;
  border: none;
}
.button:hover {
  background-color: #f0920e;
}

```

We do this.

```
const Button = styled.button`
  padding: 1rem 2rem;
  font-weight: 600;
  text-transform: uppercase;
  border-radius: 0.25rem;
  color: #1f2937;
  background-color: #f0b322;
  border-radius: 6px;
  border: none;
  &:hover {
    background-color: #f0920e;
  }
`;

```

### Creating Reusable Components & Component Combinations

We can create **_.jsx_** files and default export the styled components so that they live in separate files keeping the actual component files clean.

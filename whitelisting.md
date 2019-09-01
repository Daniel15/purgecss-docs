# Whitelisting

You can whitelist selectors to stop PurgeCSS from removing them from your CSS. This can be accomplished with the PurgeCSS options `whitelist` and `whitelistPatterns`, or directly in your CSS with a special comment.

## Specific selectors

You can whitelist selectors with `whitelist`.

```javascript
const purgecss = new Purgecss({
    content: [], // content
    css: [], // css
    whitelist: ['random', 'yep', 'button']
})
```

In the example, the selectors `.random`, `#yep`, `button` will be left in the final CSS.

## Patterns

You can whitelist selectors based on a regular expression with `whitelistPatterns` and `whitelistPatternsChildren`.

```javascript
const purgecss = new Purgecss({
    content: [], // content
    css: [], // css
    whitelistPatterns: [/red$/],
    whitelistPatternsChildren: [/blue$/]
})
```

In the example, selectors ending with `red` such as `.bg-red`, and children of selectors ending with `blue` such as `blue p` or `.bg-blue .child-of-bg`, will be left in the final CSS.

Patterns are regular expressions. You can use [regexr](https://regexr.com) to verify the regular expressions correspond to what you are looking for.

## In the CSS directly

You can whitelist directly in your CSS with a special comment.
Use `/* purgecss ignore */` to whitelist the next rule.

```css
/* purgecss ignore */
h1 {
    color: blue;
}
```

You can use `/* purgecss start ignore */` and `/* purgecss end ignore */` to whitelist a range of rules.

```css
/* purgecss start ignore */
h1 {
  color: blue;
}

h3 {
  color: green;
}
/* purgecss end ignore */

h4 {
  color: purple;
}

/* purgecss start ignore */
h5 {
  color: pink;
}

h6 {
  color: lightcoral;
}

/* purgecss end ignore */
```


# Himalaya

Himalaya is a pure JavaScript HTML parser that converts HTML into JSON, which can then be further manipulated by other modules.

```bash
npm install himalaya
```

## Usage

```javascript
var himalaya = require('himalaya');
var html = require('fs').readFileSync('/webpage.html');
var json = himalaya.parse(html);
```

## Input/Output

```html
<div class='post post-featured'>
	<p>Himalaya parsed me...</p>
	<!-- ...and I liked it. -->
</div>
```

```json
[
  {
    "className": [
      "post",
      "post-featured"
    ],
    "tagName": "div",
    "children": [
      {
        "tagName": "p",
        "children": [
          {
            "content": "Himalaya parsed me...",
            "kind": "text"
          }
        ],
        "kind": "element"
      },
      {
        "content": " ...and I liked it. ",
        "kind": "comment"
      }
    ],
    "kind": "element"
  }
]
```

*Note*: Text nodes containing only whitespace were removed from the output for readability.

## Features

### Synchronous 
HTML is turned into JSON, that's it. Himalaya is completely synchronous and does not require any complicated callbacks. 

### Parses Attributes
Himalaya does a couple things when processing attributes:
- Camel-cases attribute names
- Attributes without values are set to their names (i.e. `disabled` turns into `disabled="disabled"`)
- Groups `data-*` attributes into a `dataset` object
- Attempts to caste any value to a number if `!Nan`
- Parses the `style` attribute into a hash map

### Handles Weirdness
Himalaya handles a lot of HTML's fringe cases, like:
- Closes unclosed tags `<p><b>...</p>`
- Ignores extra closing tags `<span>...</b></span>`
- Properly handles void tags like `<meta>` and `<img>`
- Properly handles self-closing tags like `<input/>`
- Handles `<!doctype>` and `<-- comments -->`
- Does not parse the contents of `<script>`, `<style>`, and HTML5 `<template>` tags

### Preserves Whitespace
Himalaya does not cut corners and returns an accurate representation of the HTML supplied.

## Why "Himalaya"?

[First, my friends weren't very helpful.](https://twitter.com/compooter/status/597908517132042240) Except Josh, Josh had my back.

While I was testing the parser, I threw a download of my Twitter homepage in and got a huge JSON blob out. My code editor Sublime Text has a mini-map and looking at it sideways the data looked like a never-ending mountain range. Also, "himalaya" has H, M, L in it.

## Nerd Stuff

My first implementation used look-ahead, found the matching closing tag, and then recursively parsed the inners until a tree was built. That was problematic. This implementation uses no look-ahead and instead uses a stack to keep track of nesting as the source string is cut away. When an end tag is found the stack it cut to match and then parsing picks up again at the higher level. 

This parser was implemented without using any regular expressions. Since I wanted this to double as a learning resource, I wanted the code to be readable sans regexes.

## Contributing

We can always have more tests: if you find a bug, create an issue or be **fabulous** and fix the problem and write the tests up yourself in a coherent pull request.

Run tests with the `npm test` command.

Follow me on [Twitter](https://twitter.com/compooter) for updates or just for the lolz and please check out my other [repositories](https://github.com/andrejewski) if I have earned it. I thank you for reading.

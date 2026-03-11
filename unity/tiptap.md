
# custom extension

## parseHTML & renderHTML

- Persistence logic lives in the _contract_ between `renderHTML` and `parseHTML`.
-  `renderHTML` decides **what information is emitted**
	- If you don’t render something into HTML, it is **gone**.
-  `parseHTML` decides **how information is interpreted**
	- If you don’t parse it back, it is **ignored**.
- Persistence is correct **only if**:
	- `parseHTML(renderHTML(state)) === state`

Example:
```javascript
const MediaImage = Image.extend({
  name: "mediaImage",

  parseHTML() {
    return [
      {
        tag: 'img[data-type="media"]',
        getAttrs: element => ({
          src: element.getAttribute("data-media-src"),
        }),
      },
    ]
  },

  renderHTML({ HTMLAttributes }) {
    const logicalSrc = HTMLAttributes.src

    return [
      "img",
      {
        ...HTMLAttributes,
        "data-type": "media",
        "data-media-src": logicalSrc,
        src: resolveMediaUrl(logicalSrc),
      },
    ]
  },
})
```
### renderHTML
> With the `renderHTML` function you can control how an extension is rendered to HTML.

- We pass an attributes object to it, it contains:
	- all local attributes
	- global attributes
	- configured CSS classes [?]
- Above example will return something like:
	- `<img data-media-src="media://2025/asd.jpg" src="http://localhost/...">`
- It is called on `getHTML()`, it answers a question:
	- *Given my current editor state, what HTML should I emit?*
- It does **not** care whether:
	- this HTML can be re-parsed correctly
	- information is lost
	- semantics survive reload

### parseHTML
> The `parseHTML()` function tries to load the editor document from HTML. 

- This is very important, it is used when you call `editor.command.setContent(html)`, parseHTML rules are then used to build nodes of a ProseMirror document.
- So it answers the question: “*Given some HTML, how do I reconstruct my document model?*”
- You can only skip implementing it if you:
	- Never call `setContent(html)`
	- Never paste/import HTML
	- You ONLY load via JSON: getJSON() & setContent(json)
	- (*This is not the case with sanctuary UI which renders to/from HTML!*)
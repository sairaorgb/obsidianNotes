#### Intro

- A **host environment** for JavaScript is the runtime where the code is executed. It provides objects and functions that JavaScript can interact with in addition to the core. It could be a web browser, web server, another host or a coffee machine if it can run js.

- The Document Object Model, or DOM for short, represents all page content as objects that can be modified.
- The Browser Object Model (BOM) represents additional objects provided by the browser (host environment) for working with everything except the document.

- Browser = V8 (or other JS engine) + DOM + BOM + Web APIs → for web pages.
- Node.js = V8 + Node APIs (fs, http, etc.) → for server-side work, no DOM/BOM.

#### DOM tree

- There are 12 node types. In practice we usually work with 4 of them:
	- document – the “entry point” into DOM.
	- element nodes – HTML-tags, the tree building blocks.
	- text nodes – contain text.
	- comments – sometimes we can put information there, it won’t be shown, but JS can read it from the DOM.
	
- Spaces and newlines are totally valid characters, like letters and digits. They form text nodes and become a part of the DOM.
- If the browser encounters malformed HTML like missing head tag or closing tags, it automatically corrects it when making the DOM.
- An interesting “special case” is tables. By DOM specification they must have < tbody> tag, but HTML text may omit it. Then the browser creates < tbody> in the DOM automatically.

#### Walking the DOM

| ![[Pasted image 20250811201944.png \|380]] | ![[Pasted image 20250811202045.png\|380]] |
| ------------------------------------------ | ----------------------------------------- |

``` js
<html> = document.documentElement
<body> = document.body
<head> = document.head

// child nodes ( A collection)
for (let node of document.body.childNodes) {
  alert(node); // shows all nodes from the collection
}

alert( Array.from(document.body.childNodes).filter ); 

// children elements
alert( document.documentElement.parentNode ); // document
alert( document.documentElement.parentElement ); // null


```

- A script cannot access an element that doesn’t exist at the moment of running. so, usually script is written at bottom.

- The **childNodes** collection lists all child nodes, including text nodes.Properties `firstChild` and `lastChild` give fast access to the first and last children.
- It is a collection but not array so, for..of loop works but array methods don't work. (for..in doesn't work)
- DOM collections are live. they get current state of DOM.
- nodes have some other properties including `nextSibling`,`previousSibling` and `parentNode`.

- children collection has the same notion except that it doesn't include nodes that aren't elements.

#### Searching: getElement*, querySelector*


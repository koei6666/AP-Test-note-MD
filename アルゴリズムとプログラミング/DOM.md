### DOM 
　**DOM stands for Document Object Model. It's a programming interface for HTML and XML documents, and it represents the structure of the documents. It allows programs and scripts to dynamically access and update the content, structure, and style of documents.**

　The DOM ==represents a document as a tree structure== where each node is an object representing a part of the document. The nodes can be manipulated and any visible changes made to the structure or contents of the document can be reflected in the display of the document by the browser.

　For example, with HTML documents, the DOM represents the structure as follows: The "document" is the root node, "html" is a child node of the document, and "body" and "head" are child nodes of "html", and so on. Each of these nodes can be accessed and manipulated using various programming languages, most commonly JavaScript.

　==The DOM is not a programming language, but without it, JavaScript wouldn't have any model or notion of web pages, HTML documents, XML documents, and their component parts (e.g. elements, attributes, text).== Every element in a document—the document as a whole, the head, tables within the document, table headers, text within the table cells—is part of the document object model for that document, so they can be accessed and manipulated using the DOM and a scripting language like JavaScript. ^814e5f

```javascript
function parseHTML(htmlString) {
    const parser = new DOMParser();
    const dom = parser.parseFromString(htmlString, 'text/html');
    return dom;
}

// Usage
const htmlString = `
<!DOCTYPE html>
<html>
<head>
    <title>Sample Page</title>
</head>
<body>
    <h1>Welcome</h1>
    <p>This is a paragraph.</p>
</body>
</html>
`;

const resultDOM = parseHTML(htmlString);
```

```javascript
// Get the title of the page
const pageTitle = resultDOM.querySelector('title').textContent;
console.log(pageTitle); // Output: "Sample Page"

// Get the h1 element
const h1Element = resultDOM.querySelector('h1');
console.log(h1Element.textContent); // Output: "Welcome"

// Change the content of the paragraph
const paragraph = resultDOM.querySelector('p');
paragraph.textContent = "This is an updated paragraph.";
```

[[セキュリティ/DOM Based XSS|DOM Based XSS]]

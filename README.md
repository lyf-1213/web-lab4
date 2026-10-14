# web-lab4 — XML Lab & JSON Lab

158.258 Web Development — Lab 4 submission
Student: 247049 刘依枫 (Yifeng Liu)

## Live pages (GitHub Pages)

* **XML Lab — Book Catalogue:** https://lyf-1213.github.io/web-lab4/book.html
* **JSON Lab — User Profiles:** https://lyf-1213.github.io/web-lab4/profiles.html

## Files

| File | Description |
| --- | --- |
| `book.xml` | Part 1 — book catalogue as a well-formed XML document. Five books, each with a unique `id` attribute and the child elements `title`, `author` and `price`. |
| `book.html` | Part 1 — fetches `book.xml` with the fetch API, parses the XML string with `DOMParser` and displays each book's title, author and price in a table. |
| `userProfiles.json` | Part 2 — four user profiles with `firstName`, `lastName`, `email`, `dateOfBirth`, an `address` object (`street`, `city`, `state`, `zipCode`) and a `preferences` object (`theme`, `language`, `notifications`, `subscription`). |
| `profiles.html` | Part 2 — fetches and parses `userProfiles.json` with the fetch API and displays every profile, including the address and preferences. |

## How it works

Both pages load their data with the fetch API and then parse it in the browser:

```js
// XML: the response arrives as text and is parsed with DOMParser
fetch('book.xml')
    .then(response => {
        if (!response.ok) throw new Error('Fetch error: ' + response.status);
        return response.text();
    })
    .then(xmlText => {
        const parser = new DOMParser();
        const xmlDoc = parser.parseFromString(xmlText, 'text/xml');
        const books = xmlDoc.getElementsByTagName('book');
        // ... read getAttribute('id') and the title/author/price children
    })
    .catch(err => console.error('Fetch error: ' + err.message));
```

```js
// JSON: the response is parsed straight into a JavaScript object
fetch('userProfiles.json')
    .then(response => {
        if (!response.ok) throw new Error('Fetch error: ' + response.status);
        return response.json();
    })
    .then(json => { /* ... render every profile ... */ })
    .catch(err => console.error('Fetch error: ' + err.message));
```

## Run it locally

Because both pages use `fetch()`, they must be served over HTTP — opening the
files directly from disk (a `file://` URL) is blocked by the browser's CORS
rules.

```powershell
python -m http.server 8099
```

Then open <http://127.0.0.1:8099/book.html> and
<http://127.0.0.1:8099/profiles.html>.

## Validation

* `book.xml` — checked with an XML parser and the W3C Markup Validation Service;
  the document is well-formed, has a single root element (`bookCatalogue`), every
  `<book>` has a unique `id`, and every `<book>` contains `title`, `author` and
  `price`.
* `userProfiles.json` — checked with a JSON parser and an online JSON validator;
  the structure is valid, with four profiles, each having an `address` object and
  a `preferences` object including the added `subscription` field.

## Publishing

This repository is published with GitHub Pages from the `main` branch root
(Settings → Pages → Deploy from a branch → `main` / `/ (root)`), so the data
files sit next to the HTML pages and the `fetch()` calls resolve relatively.

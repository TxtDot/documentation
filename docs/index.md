# Getting Started

## What is this

_txtdot_ is a proxy that requests the page by the given URL,
extracts only useful data including text, links, pictures and tables,
and returns it as an HTML page with a minimalistic design
optimized for text reading.

_txtdot_ increases the loading speed and reduces client's bandwidth usage
since no unnecessary code and no scripts are transferred.
Also, you won't see any advertisement (unless it's a static picture that is hard to detect as ads).
There are no trackers too.

## How to use it

_txtdot_ is an open source software, so everyone can host it on his own server.
The official instance is [txt.dc09.ru](https://txt.dc09.ru),
the list of all instances is [here](https://github.com/txtdot/instances).

On the main page, there's a handy form where you can
specify a URL, choose an engine and a format for parsed data.
On the `/get` page, "Home" button returns you to `/`,
"Original page" opens the entered URL in the same window without txtdot proxy.

The latest docs for API endpoints can be found [here](https://txt.dc09.ru/doc).
For handy JSON API, use `/api/parse` returning an engine result object (see below).
For pure HTML response, use `/api/raw-html`.
Note that both API and browser endpoints on txt.dc09.ru
are ratelimited to 2 requests per second.

## How it works

This project exists thanks to great Mozilla's
[Readability.js](https://github.com/mozilla/readability) library.
The initial idea was to process HTML with it on the server
so the client does not need to download and execute heavy JS,
doesn't need to use an adblock.

Readability performs its work very well in most cases.

If an `?engine=` parameter wasn't passed, but txtdot found
that a specific engine is assigned to the requested domain,
for example, `"stackoverflow.com": engines.StackOverflow`,
it uses that engine to process the URL.
Otherwise, the page is parsed with the engine assigned to `*` (it's Readability).

### Plugins

Readability is good, but now always, so [artegoser](https://github.com/artegoser) wrote the basis of the code
keeping in mind that we'll extend txtdot with other _engines_.
Back then, it was functions taking a URL as a parameter,
returning an object that contains extracted HTML and plain text, page title and language.
The object is rendered with ejs template (or, in `/api/parse`, just sent as JSON).

But after a while it became unwieldy and we decided to create a monorepo. We created classes Engines, Middlewares that handle the necessary parts. Now you can create such functions for different domains, and routes. Also we added support for JSX for simplifying the code of plugins.

## Engines

Creation of engines is easy.

```ts
import { Engine, Route } from "@txtdot/sdk";

const Readability = new Engine(
  "Readability", // Name of the engine
  "Engine for parsing content with Readability", // Description
  ["*"] // Domains that use this engine
);

Readability.route("*path", async (input, ro: Route<{ path: string }>) => {
  // ...

  // If any of the parameters except content is empty, txtdot will try to extract it from the page automatically
  return {
    content: parsed.content,
    title: parsed.title,
    lang: parsed.lang,
  };
});
```

## Middlewares

Creation of middlewares similar to engines.

```tsx
import { Middleware } from "@txtdot/sdk";

const Highlight = new Middleware(
  "Highlight",
  "Highlights code with highlight.js only when needed",
  ["*"]
);

Highlight.use(async (input, ro, out) => {
  if (out.content.indexOf("<code") !== -1)
    return {
      ...out,
      content: <Highlighter content={out.content} />,
    };

  return out;
});
```

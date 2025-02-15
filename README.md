# Saleor Documentation

- Documentation: [https://docs.saleor.io](https://docs.saleor.io)
- Project homepage: [https://saleor.io](https://saleor.io)
- Saleor Core source code: [https://github.com/saleor/saleor](https://github.com/saleor/saleor)
- Saleor Dashboard source code: [https://github.com/saleor/saleor-dashboard](https://github.com/saleor/saleor-dashboard)
- Saleor React Storefront source code: [https://github.com/saleor/react-storefront](https://github.com/saleor/react-storefront)

# What's In This Document

- [Get Started in 5 Minutes](#get-started-in-5-minutes)
- [Directory Structure](#directory-structure)
- [Editing Content](#editing-content)
- [Adding Content](#adding-content)
- [Full Documentation](#full-documentation)

# Get Started in 5 Minutes

1. Make sure you are using Node in version 12+:

```sh
node --version
```

2. Install project dependencies:

```sh
npm install
```

3. Run your dev server:

```sh
npm start
```

# Production Build

1. Build project:

```sh
npm run build
```

2. Testing build local:

```sh
npm run serve
```

# Editing Content

## Directory Structure

- `/docs` Current `Canary` developer documentation.
- `/docs/api-reference` Automatically generated API reference.
- `/docusaurus.config.js` Docusaurus configuration file.
- `/docusaurus2-graphql-doc-generator` GraphQL API Reference plugin code.
- `/sidebars.js` Sidebar menu structure.
- `/static` Styling and other static files.
- `/versioned_docs` & `/versioned_sidebars` Tagged versions of documentation matching the latest Saleor release.

## Formatting

### Code formatting

Code and response examples should be inside code blocks with proper language:

````md
```graphql
query {
  id
  name
}
```
````

````md
```json
{
  "errorCode": 400
}
```
````

### Lining pages

Use full path to the file to avoid linking to wrong page.

- :white_check_mark: Example of good link: `[Attributes](/docs/developer/attributes.mdx)`
- :stop_sign: Avoid: `[Attributes](/attributes)`

### Using custom React components

All documentation files use extension:

- `.mdx` - Developer documentation
- `.md` - Dashboard documentation

If your page uses custom react components, you are required to use `.mdx` file extension. Import statement is also required:

```mdx
## <!-- /docs/developer/export/export-overview.mdx file -->

## title: Exporting Products

import Foo from "@site/components/Foo";

...

<Foo />
```

For charts we are using [Mermaid](http://mermaid-js.github.io/mermaid/) package.

## Editing an existing docs page

Edit docs by navigating to `docs/` and editing the corresponding document:

`docs/doc-to-be-edited.md`

```markdown
---
id: page-needs-edit
title: This Doc Needs To Be Edited
---

Edit me...
```

For more information about docs, click [here](https://docusaurus.io/docs/en/navigation)

# Adding Content

## Adding a new docs page to an existing sidebar

1. Create the doc as a new markdown file in `/docs`, example `docs/newly-created-doc.md`:

```md
---
id: newly-created-doc
title: This Doc Needs To Be Edited
---

My new content here..
```

1. Refer to that doc's ID in an existing sidebar in `sidebar.js`:

```javascript
// Add newly-created-doc to the Getting Started category of docs
{
  "docs": {
    "Getting Started": [
      "quick-start",
      "newly-created-doc" // new doc here
    ],
    ...
  },
  ...
}
```

For more information about adding new docs, click [here](https://docusaurus.io/docs/en/navigation)

## Adding items to your site's top navigation bar

1. Add links to docs, custom pages or external links by editing the headerLinks field of `siteConfig.js`:

```javascript
{
  headerLinks: [
    ...
    /* you can add docs */
    {
      type: "doc",
      docId: "dashboard/before-you-start",
      label: "Dashboard Manual",
      position: "left",
    },
    /* you can add custom pages */
    { page: 'help', label: 'Help' },
    /* you can add external links */
    { href: 'https://github.com/facebook/Docusaurus', label: 'GitHub' },
    ...
  ],
  ...
}
```

For more information about the navigation bar, click [here](https://docusaurus.io/docs/en/navigation)

## Adding custom pages

1. Docusaurus uses React components to build pages. The components are saved as .js files in `./pages/en`:
1. If you want your page to show up in your navigation header, you will need to update `siteConfig.js` to add to the `headerLinks` element:

```javascript
{
  headerLinks: [
    ...
    { page: 'my-new-custom-page', label: 'My New Custom Page' },
    ...
  ],
  ...
}
```

For more information about custom pages, click [here](https://docusaurus.io/docs/en/custom-pages).

# Versioning

The latest Saleor version is labeled as `3.x`. It provides documentation for users based on the latest stable release of Saleor.

The current in-progress version is labeled as `Canary` and is built based on the documents in the `docs` folder. The "Canary" version represents the bleeding-edge development work, where the latest enhancements, bug fixes, and experimental features are being actively worked on and tested.

The canary version serves as a preview of what's to come, enabling developers and early adopters to explore upcoming features and contribute to the platform's advancement.

## Updating versions

The changes made to the main docs folder will be available in the `Canary` version.
The changes made to the versioned_docs/version-3.x folder will be available in the `3.x` version (current).

### Updating stable version

Apply changes in both `docs` and `versioned_docs/version-3.x` folders.

### Introducing features for the upcoming Saleor version

Apply changes in `docs` folders.

## Releasing `Canary` to `3.x`

### With the use of GitHub Action

`update-stable-version` GitHub Action workflow checks on a daily basis the `saleor/saleor` repository's latest release in search for the attached `schema.graphql` file. Such an attachment suggests the need to update the `3.x` docs and if found it creates a PR updating `3.x` based on `canary`.

### Manually

1. Remove `versions.json` file or just `"3.x"` inside
1. Run `UPDATE_SALEOR=true npx docusaurus docs:version 3.x`
1. Commit changes and issue a PR

# Updating the API Reference & Storefront Reference

Files in /docs/api-reference are generated by `@graphql-markdown/docusaurus` package. Introduction pages are automatically copied from `/template/api-reference.mdx` and `/template/storefromt/api-reference.mdx` files.

## Updating

To update the API reference:

1. Put `schema.graphql` locally into the root of the saleor-docs repository
1. Run `npm run update-api-reference`

The script behind the scenes does several steps:

1. The api-reference is generated in `.tmp` folder.
1. The highlighting script does additional improvements in the generated docs. It makes the Saleor version and required permissions more visible.
1. The current references from `docs` folder are being removed and generated references in `.tmp` folder are moved to the `docs`.
1. The code examples are being injected. The code examples from the `examples` folder are being put into corresponding files in the references folder.

# Search

Saleor Docs are using Algolia DocSearch for the website search.

### Crawl interval

DocSearch crawls the website once a week on Friday and aggregates all the content in an Algolia index. This content is then queried directly from the front-end using the Algolia API.

### Ranking strategy

The website's search results are meticulously ranked to enhance user relevance and experience. A custom ranking function, known as `pageRank`, is employed for this purpose. The ranking strategy prioritizes various content categories as follows:

1. `metaPageRank`: This takes precedence and is determined by a custom meta attribute, providing a top-tier ranking for specific content.

1. `Documentation Pages`: General documentation pages are next in line for ranking, excluding those generated based on schema API reference and API storefront.

1. `API Reference Pages`: These pages are ranked differently based on the type of operation they represent.

```
function pageRank(url) {
  if (metaPageRank) {
    return metaPageRank;
  }
  if (!/\/api-reference\/|\/api-storefront\//.test(url.pathname)) {
    // not part of API reference
    return "40";
  }
  if (/\/mutations\//.test(url.pathname)) {
    // mutation
    return "30";
  }
  if (/\/queries\//.test(url.pathname)) {
    // query
    return "20";
  }
  return "10";
}
```

### Manual Ranking Control

For even finer control over search result rankings, you can manually influence the ranking of specified pages by adding a custom meta attribute - `rank` - to the page. The `rank` meta is configured to have the highest priority in Algolia.

To assign a custom rank to a particular page, use the following code snippet:

```
<head>
  <meta name="rank" content="50" />
</head>
```

# Debugging

In dev mode, Docusaurus serves a debug page with a list of all available routes and config at http://localhost:3000/\_\_docusaurus/debug.

# Full Docusaurus Documentation

Full documentation can be found on the [website](https://docusaurus.io/).

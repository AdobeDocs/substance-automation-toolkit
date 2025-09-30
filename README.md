# Sphinx doc CSS injection & GitHub pages deployment
- ---

  > **If '_static/custom.css' file already exists, 
    Jump to step 3.**

### -1/ Create ```_static/custom.css``` file and add these lines:

``` css
/* custom.css */
body {
    font-family: Arial, sans-serif;
    max-width: 900px;
    margin: auto;
    padding: 20px;
    background-color: #f9f9f9;
}
h1, h2, h3 {
    color: #2c3e50;
}
a {
    color: #007acc;
    text-decoration: none;
}
a:hover {
    text-decoration: underline;
}
code {
    background: #f4f4f4;
    padding: 2px 4px;
    border-radius: 4px;
}
```


### -2/ Edit conf.html:

- Delete these stylesheet lines in ```<head>```:
  - ```<link rel="stylesheet" type="text/css" href="../_static/pygments.css" />```
  - ```<link rel="stylesheet" type="text/css" href="../_static/css/theme.css" />```
- Right above ```</head>``` closing line, add this line (with 1 indent):
  - ```<link rel="stylesheet" href="../_static/custom.css">```


### -3/ Create GitHub pages custom Workflow file: 
  - Name it ```deploy_saws_doc.yml```
  - Paste the workflow file content from below:
 ``` yaml
# Simple workflow for deploying static content to GitHub Pages
name: Deploy static content to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["doc_release"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          # Upload folder content to GitHub Pages
          path: 'docs'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
### Now push all changes to the ```doc_release``` branch will trigger the GitHub action to deploy to GitHub pages.
Cheers!

- ---
- ---
- ---

# Adobe I/O Documentation Template

This is a site template built with the [Adobe I/O Theme](https://github.com/adobe/aio-theme).

View the [demo](https://adobedocs.github.io/dev-site-documentation-template/) running on Github Pages.  

## Where to ask for help

The slack channel #adobeio-onsite-onboarding is our main point of contact for help. Feel free to join the channel and ask any questions.

## How to develop

For local development, simply use :

```shell
$ yarn install
$ yarn dev
```

For the developer documentation, read the following sections on how to:

- [Arrange the structure content of your docs](https://github.com/adobe/aio-theme#content-structure)
- [Link to pages](https://github.com/adobe/aio-theme#links)
- [Use assets](https://github.com/adobe/aio-theme#assets)
- [Set global Navigation](https://github.com/adobe/aio-theme#global-navigation)
- [Set side navigation](https://github.com/adobe/aio-theme#side-navigation)
- [Use content blocks](https://github.com/adobe/aio-theme#jsx-blocks)
- [Use Markdown](https://github.com/adobe/aio-theme#writing-enhanced-markdown)

For more in-depth [instructions](https://github.com/adobe/aio-theme#getting-started).

## How to test

- To run the configured linters locally (requires [Docker](https://www.docker.com/)):

  ```shell
  yarn lint
  ```

  > NOTE If you cannot use Docker, you can install the linters separately. In `.github/super-linter.env`, see which linters are enabled, and find the tools being used for linting in [Supported Linters](https://github.com/github/super-linter#supported-linters).

- To check internal links locally

  ```shell
  yarn test:links
  ```

- To build and preview locally:

  ```shell
  yarn start
  ```

## How to deploy

For any team that wishes to deploy to the developer.adobe.com and developer-stage.adobe.com websites, they must be in contact with the dev-site team. Teams will be given a path that will follow the pattern `developer.adobe.com/{product}/`. This will allow doc developers to setup their subpaths to look something like:

```text
developer.adobe.com/{product}/docs
developer.adobe.com/{product}/community
developer.adobe.com/{product}/community/code_of_conduct
developer.adobe.com/{product}/community/contribute
```

### Launching a deploy

You can deploy using the GitHub actions deploy workflow see [deploy instructions](https://github.com/adobe/aio-theme#deploy-to-azure-storage-static-websites).

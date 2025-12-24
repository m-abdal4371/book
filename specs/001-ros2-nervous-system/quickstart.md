# Quickstart: Docusaurus Book Project

This guide provides the essential steps to set up the Docusaurus environment, run the development server, and build the static site.

## Prerequisites

- [Node.js](https://nodejs.org/) (LTS version recommended)
- [npm](https://www.npmjs.com/) (usually included with Node.js)

## Setup

1.  **Initialize the project and install Docusaurus**:
    
    ```bash
    npx create-docusaurus@latest . classic
    ```
    
    This command scaffolds a new Docusaurus site in the current directory.

2.  **Install dependencies**:
    
    If the command above does not automatically install dependencies, run:
    
    ```bash
    npm install
    ```

## Running the Development Server

To preview the book website locally with live reloading:

```bash
npm run start
```

The site will be available at `http://localhost:3000`.

## Building the Static Site

To generate the static HTML files for deployment:

```bash
npm run build
```

The output will be placed in the `build/` directory. This is the directory that should be deployed to GitHub Pages.

## Deploying to GitHub Pages

1.  **Configure `docusaurus.config.js`**:
    
    Ensure the following properties are set correctly in `docusaurus.config.js`:
    
    ```js
    const config = {
      // ...
      url: 'https://<YOUR_GITHUB_USERNAME>.github.io', // Your GitHub pages URL
      baseUrl: '/<YOUR_REPOSITORY_NAME>/', // The name of your repository
      organizationName: '<YOUR_GITHUB_USERNAME>', // Your GitHub username
      projectName: '<YOUR_REPOSITORY_NAME>',      // Your repository name
      // ...
    };
    ```

2.  **Run the deployment command**:
    
    Docusaurus provides a convenient command to build and deploy the site to the `gh-pages` branch.
    
    ```bash
    GIT_USER=<YOUR_GITHUB_USERNAME> npm run deploy
    ```
    
    After this command completes, the site will be live at the URL specified in the config file.

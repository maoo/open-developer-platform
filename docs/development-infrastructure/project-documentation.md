---
id: project-documentation
title: Project Documentation
---

FINOS collaboration services include several ways to write, collaborate and publish project documentation.

After experimenting with few tools, services and technologies, we have chosen [Docusaurus](#using-docusaurus) (and GitHub Pages) to be the best choice.

## Docusaurus

Docusaurus is a static website generator written in React and available open source under the Apache 2.0 license. A similar technology is Jekyll, which provides a tighter integration with GitHub Pages, but lacks of development support (as in Ruby) and tools for local development.

Given the significant amount of FINOS projects that adopted this framework, it became our go-to solution to build project documentation websites; if you're getting started, you may want to use the [FINOS project blueprint](https://github.com/finos/project-blueprint) repository template, which already provides a built-in Docusaurus website; there is also a [Docusaurus Build action for GitHub.com](#docusaurus-build-action) available and documented below.

This page walks through the use of docusaurus on a local environment; full documentation can be found on [docusaurus website](https://docusaurus.io/).

### Docusaurus Build Action
FINOS have developed a GitHub Action to automatically build the Docusaurus websites and publish them into GitHub Pages; the action works on upstream repositories (ie https://github.com/finos/open-developer-platform) as well as forked ones (ie https://github.com/maoo/open-developer-platform), providing a simple way to stage the web version of a change to a Docusaurus website.

The action also intercepts Pull Requests and adds a comment with the link to the website (preview) of the PR submitter; this way issue reviewers can easily and visually see the changes.

#### Enabling the action
If you're forking a repository with the Docusaurus build action, you need to:

1. Create a new branch called `gh-pages`, if it doesn't already exist. Check [the GitHub guide](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-and-deleting-branches-within-your-repository) if you don't know how to create a branch.
2. Access the `Actions` tab of your GitHub (forked) repository (on `https://github.com/<your username>/<repository name>/actions`) and click on the `Enable GitHub Actions` button.

At this point, any change on your `master` branch (within the `docs/` or `website/` folders) will be published on `https://<github username>.github.io/<repository name>`. Go ahead and make a change to any `docs/*.md` file and see if they get published on the website.

#### Known issue
If you don't create a `gh-pages` branch prior to the first action run, Docusaurus will, but for some reason, it doesn’t correctly update the HTTP(s) endpoint, and as a result the website returns a `404` error.

To fix it, follow these steps:

1. Visit `https://github.com/<org/user name>/<repository name>/settings`
2. Find the `GitHub Pages` section
3. Select the `master` branch from the `Source` dropdown
4. Scroll down again to the `GitHub Pages` section
5. At the top it should say Your site is published at `https://<org/user name>.github.io/<repository name>`
6. Select the gh-pages branch from the `Source` dropdown

It may take 20 to 40 minutes to enable the URL, until then you may get a 404 error.

The action can be [copied from the ODP GitHub repository](https://github.com/finos/open-developer-platform/blob/master/.github/workflows/docusaurus.yml)

In order to use the action, simply copy the [`Raw` file content](https://github.com/finos/open-developer-platform/blob/master/.github/workflows/docusaurus.yml) into a file called `.github/workflows/docusaurus.yml` , then push the changes; the action should automatically run and deploy the HTML files into the `gh-pages` branch.

### Docusaurus 2 (alpha)
This document refers to Docusaurus 1.x version.

The new version of docusaurus is currently (March 2020) in alpha, and brings lots of improvements; for those starting to use Docusaurus now, it is strongly suggested to start using this version. This version is built with Docusaurus 2.

### Get Started in 5 Minutes
Regardless of the language, eco-system or platform you're using, run the following commands.

Check if NodeJS and NPM command-line tools are installed:
```
npm -v
node -v
```

Create a Docusaurus project and start the website locally:
```
npm i -g docusaurus-init docusaurus
mkdir docusaurus-test
cd docusaurus-test
docusaurus-init
mv docs-examples-from-docusaurus docs
mv website/blog-examples-from-docusaurus website/blog
cd website
docusaurus-start
```

### Directory Structure
Your project file structure should look something like this:

```
/project_root
  docs/
    doc-1.md
    doc-2.md
    doc-3.md
  website/
    blog/
      2016-3-11-oldest-post.md
      2017-10-24-newest-post.md
    core/
    node_modules/
    pages/
    static/
      css/
      img/
    package.json
    sidebar.json
    siteConfig.js
```

In Docusaurus 2, the [project structure have slightly changed](https://v2.docusaurus.io/docs/installation#project-structure). 

### Editing Content
Website pages are represented by MarkDown files in the the `docs/` folder (in Docusaurus 2, also MarkDown React files are supported); the page `id` maps to the URL slug of the page; the title renders out as `<h1>`.

```
---
id: page-needs-edit
title: This Doc Needs To Be Edited
---

This is the page content!
```

### Managing the navigation bar
Add links to docs, custom pages or external links by editing the `headerLinks` field of `website/siteConfig.js`:

```
{
  headerLinks: [
    ...
    /* you can add docs */
    { doc: 'my-examples', label: 'Examples' },
    /* you can add custom pages */
    { page: 'help', label: 'Help' },
    /* you can add external links */
    { href: 'https://github.com/facebook/Docusaurus', label: 'GitHub' },
    ...
  ],
  ...
}
```
For more information, checkout the [docusaurus navigation docs](https://docusaurus.io/docs/en/navigation).

In Docusaurus 2, [navigation have been greatly improved](https://v2.docusaurus.io/docs/sidebar).

## Other solutions
Before Docusaurus, FINOS Community have experimented few other solutions, which are worth mentioning for historical reasons:

1. [GitHub Wiki](https://help.github.com/en/github/building-a-strong-community/about-wikis) provides a great integration with code, as it's hosted as a git endpoint; however, Pull Requests are not available, therefore the collaboration mechanism can only be extended to the project team, which is a big limitation. Also, installing a local development environment [is not trivial](https://gist.github.com/suewonjp/7493de784f4a88c63d1810031609ee35) Its biggest advantage is the possibility to preview the changes via the GitHub web UI
2. [Jekyll](https://jekyllrb.com/), the page generator engine used by GitHub Pages, which also supports templating; the downside of Jekyll is that the installation of a local development environment (Ruby) is hard.
3. [GoHugo](https://gohugo.io/), a generic static page generator written in Go, with a strong community and eco-system

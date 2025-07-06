# mike yu personal website

Generated with [Hugo](https://gohugo.io/) using the [Hugo-Coder](https://github.com/luizdepra/hugo-coder/) theme.

## config

Most of the config per theme is handled in the config.toml file. Here's an example of a [minimal config](https://github.com/luizdepra/hugo-coder/blob/main/docs/configurations.md#complete-example) for the Hugo Coder theme along with documentation on every possible setting.

The most important things are:

1. markdown content is managed in the content folder
2. the different menu options are managed through the `[[menu.main]]` sections
3. The values associated with the url keyword in the menu.main sections must correspond with a filename or folder name in content
    - For example if you have a blog section with url=posts as the keyword, your folder of markdown blog posts must be called posts

## dev

`hugo server -D` will start a local server with all pages built including any marked as draft

## deployment

1. Create a new branch
2. Run `hugo` to build everything
3. Commit and push
4. Merge to main

Netlify will auto-deploy on all PRs merged into main

## helpful links

* [Hosting on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)
* [Netlify DNS](https://docs.netlify.com/domains-https/netlify-dns/) (if using a domain not registered with Netlify)

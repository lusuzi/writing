---
title: "Hello World"
date: 2020-10-16T20:45:35+08:00
draft: true
tags: ["hugo"]
---

## Test Installation

Once you have [installed Hugo](https://gohugo.io/getting-started/installing/), make sure it is in your `PATH`. You can test that Hugo has been installed correctly via the `help` command: 

```
hugo help
```

## The `hugo` Command

The most common usage is probably to run `hugo` with your current directory being the input directory.

This generates your website to the `public/` directory by default, although you can customize the output directory in your [site configuration](https://gohugo.io/getting-started/configuration/) by changing the `publishDir` field.

The command `hugo` renders your site into `public/` dir and is ready to be deployed to your web server.

## Draft, Future, and Expired Content 

Hugo allows you to set `draft`, `publishdate`, and even `expirydate` in your content’s [front matter](https://gohugo.io/content-management/front-matter/). By default, Hugo will not publish:

1. Content with a future `publishdate` value
2. Content with `draft: true` status
3. Content with a past `expirydate` value

All three of these can be overridden during both local development *and* deployment by adding the following flags to `hugo` and `hugo server`, respectively, or by changing the boolean values assigned to the fields of the same name (without `--`) in your [configuration](https://gohugo.io/getting-started/configuration/):

1. `--buildFuture`
2. `--buildDrafts`
3. `--buildExpired`

## LiveReload 

Hugo comes with [LiveReload](https://github.com/livereload/livereload-js) built in. There are no additional packages to install. A common way to use Hugo while developing a site is to have Hugo run a server with the `hugo server` command and watch for changes.

This will run a fully functioning web server while simultaneously watching your file system for additions, deletions, or changes within the following areas of your [project organization](https://gohugo.io/getting-started/directory-structure/):

- `/static/*`
- `/content/*`
- `/data/*`
- `/i18n/*`
- `/layouts/*`
- `/themes//*`
- `config`

Whenever you make changes, Hugo will simultaneously rebuild the site and continue to serve content. As soon as the build is finished, LiveReload tells the browser to silently reload the page.

Most Hugo builds are so fast that you may not notice the change unless looking directly at the site in your browser. This means that keeping the site open on a second monitor (or another half of your current monitor) allows you to see the most up-to-date version of your website without the need to leave your text editor.

Hugo injects the LiveReload `` before the closing `` in your templates and will therefore not work if this tag is not present..

## Disable LiveReload 

LiveReload works by injecting JavaScript into the pages Hugo generates. The script creates a connection from the browser’s web socket client to the Hugo web socket server.

LiveReload is awesome for development. However, some Hugo users may use `hugo server` in production to instantly display updated content. The following methods make it easy to disable LiveReload:

```
hugo server --watch=false
```

 Or… 

```
hugo server --disableLiveReload
```

The latter flag can be omitted by adding the following key-value to your `config.toml` or `config.yml` file, respectively: 

```
disableLiveReload = true
```

```
disableLiveReload: true
```

## Deploy Your Website 

After running `hugo server` for local web development, you need to do a final `hugo` run *without the `server` part of the command* to rebuild your site. You may then deploy your site by copying the `public/` directory to your production web server.

Since Hugo generates a static website, your site can be hosted *anywhere* using any web server. See [Hosting and Deployment](https://gohugo.io/hosting-and-deployment/) for methods for hosting and automating deployments contributed by the Hugo community.

Running `hugo` *does not* remove generated files before building. This means that you should delete your `public/` directory (or the publish directory you specified via flag or configuration file) before running the `hugo` command. If you do not remove these files, you run the risk of the wrong files (e.g., drafts or future posts) being left in the generated site.

## See Also

- [External Learning Resources](https://gohugo.io/getting-started/external-learning-resources/)
- [Use Hugo Modules](https://gohugo.io/hugo-modules/use-modules/)
- [Quick Start](https://gohugo.io/getting-started/quick-start/)
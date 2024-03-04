# One Blog

## Introduction

My Chinese Personal Blog Monorepo(😂), named *OneBlog*.

## Commands Memo

### Build Environment

Use [hugo extended version v0.123.7](https://github.com/gohugoio/hugo/releases/tag/v0.123.7) for site generation.

### Check Hugo Version

```shell
hugo version
```

### Create New Post

```shell
hugo new posts/20xx-0x-xx-PostTitle.md
```

### Live Server

```shell
hugo server
```

### Generate Static Site

First delete folder of `dist\`
```shell
rm -rf ./dist/  # Bash Like
rm -r -fo ./dist/ # Pwsh
```

Then use hugo to generate.
```shell
hugo
```

### Publish

Don't directly push to branch main (main is protected), make a PR and merge to it. This will auto trigger CI/CD to publish.

## Project Simple Description

This project using hugo with theme puppet to generate site. 

Additional change are made to theme puppet to fit this project's requirements. The description of changes made to theme puppet are listed in the file [CHANGELOG](./themes/puppet/CHANGELOG.md) file inside theme puppet folder.

## LICENSE

This project's code part follow [GPL v3 License](./LICENSE).\
The blog's [posts part](./content/) follow [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en) copyright agreement.

## Thanks

* [Project Hugo](https://github.com/gohugoio/hugo)
* [Hugo Theme Puppet](https://github.com/roninro/hugo-theme-puppet)
* [Jekyll Theme Hux Blog](https://github.com/Huxpro/huxpro.github.io)
* [Wallpaper site Pexels](https://www.pexels.com) (used detail see in [Wallpapers.md](./Wallpapers.md))

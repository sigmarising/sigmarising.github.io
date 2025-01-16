# CHANGELOG

Base on Hugo Theme Puppet [commit 71e55a8](https://github.com/roninro/hugo-theme-puppet/tree/71e55a893702ea6e1a15a71a3d0e2248673794f1), the changes made are listed below:
* delete folder `exampleSite\` and `images\`
* delete file `netlify.toml`
* delete images:
  * `static\favicon.ico`
  * `static\img\`
* Modify Area Sidebar Bio to let it have ability to fit multi-line bios, related to files:
  * `assets\sass\_sidebar.scss`
  * `layouts\partials\short-about.html`
* To fix the iOS phone search issue, update the version of [Simple-Jekyll-Search](https://www.npmjs.com/package/simple-jekyll-search) to locally dev bundle version 1.10.0
* Remove the `zoom.js` related code, and replace it with `glightbox 3.3.0`:
  * delete:
    * `assets\zoomjs\` 
  * add:
    * `static\css\glightbox.min.css`
    * `static\js\glightbox.min.js`
  * modify:
    * `layouts\baseof.html`
    * `layouts\footer.html`
    * `layouts\render-image.html`
* Replace the font-awesome cdn version to local
  * add:
    * `static\css\fontawesome-free@all.min.css`
    * `static\webfonts\`
  * modify:
    * `layouts\baseof.html`
* Modify `assets\sass\main.scss` to let code part scrollbar more beautiful

Update to Hugo v0.133.1, fix the following errors:
```
ERROR deprecated: .Site.DisqusShortname was deprecated in Hugo v0.120.0 and will be removed in Hugo 0.134.0. Use .Site.Config.Services.Disqus.Shortname instead.
ERROR deprecated: .Site.IsServer was deprecated in Hugo v0.120.0 and will be removed in Hugo 0.134.0. Use hugo.IsServer instead.
```
by modify the following files:
* `layouts/partials/comments.html`
* `layouts/partials/footer.html`

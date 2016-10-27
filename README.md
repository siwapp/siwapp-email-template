# siwapp-email-template

Helps creating email templates to use in Siwapp.

<!-- MarkdownTOC depth=0 -->

- [Quick & Dirty](#quick--dirty)
  - [Start using the Framework:](#start-using-the-framework)
  - [Build Final Template](#build-final-template)
- [Caveats](#caveats)
  - [ERB Syntax](#erb-syntax)
  - [Minification](#minification)

<!-- /MarkdownTOC -->

This repository uses [Foundation for Emails](http://github.com/zurb/foundation-emails), refer to it for comprehensive instructions.

## Quick & Dirty

First of all install [foundation-cli](https://github.com/zurb/foundation-cli).

### Start using the Framework:

```bash
$ cd /path/to/repo
$ npm install
$ foundation watch
```

This will install dependencies and open a browser at `http://localhost:3000` where you will be able to preview your e-mail template.

### Build Final Template

```bash
$ cd /path/to/repo
$ foundation build
```

This will generate the inlined template inside the `dist` directory.

## Caveats

### ERB Syntax

To avoid conflicts when using the CSS inliner in a template that makes use of the ERB `<% %>` tags, we've modified the Gulpfile file so you can use `[% %]` instead and the framework will generate the proper tags after CSS inline process.

This code:

```
[% @invoice.items.each do |item| %]
  <tr>
    <td>[%= item.description %]</td>
    <td class="total">[%= display_money item.base_amount %]</td>
  </tr>
[% end %]
```

would be transformed into this:

```
<% @invoice.items.each do |item| %>
  <tr>
    <td><%= item.description %></td>
    <td class="total"><%= display_money item.base_amount %></td>
  </tr>
<% end %>
```

### Minification

To make code easier to understand we disabled minification by default. If you want to re-enable it you can do it in the `gulpfile.babel.js` file.

1. Open the Gulpfile in your editor and search: `function inliner(css)`
2. For the `htmlmin` plugin set the `collapseWhitespace` and `minifyCSS` options as `true`.

The function would be something like this:

```javascript
function inliner(css) {
  var css = fs.readFileSync(css).toString();
  var mqCss = siphon(css);

  var pipe = lazypipe()
    .pipe($.inlineCss, {
      applyStyleTags: false,
      removeStyleTags: false,
      removeLinkTags: false
    })
    .pipe($.replace, '<!-- <style> -->', `<style>${mqCss}</style>`)
    .pipe($.replace, '<link rel="stylesheet" type="text/css" href="css/app.css">', '')
    // START Fix Ruby's ERB tags
    .pipe($.replace, /\[%/g, '<%')
    .pipe($.replace, /%\]/g, '%>')
    // END
    .pipe($.htmlmin, {
      collapseWhitespace: false,
      minifyCSS: false
    });

  return pipe();
}
```

# siwapp-email-template

<!-- MarkdownTOC depth=0 -->

- [Quick & Dirty](#quick--dirty)
  - [Start using the Framework:](#start-using-the-framework)
  - [Build Final Template](#build-final-template)

<!-- /MarkdownTOC -->

Helps creating email templates to use in Siwapp.

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

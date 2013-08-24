# Jekyll Rake Boilerplate

*Jekyll Rake Boilerplate* is a small rake "boilerplate" for doing common tasks with the static site generator [Jekyll](http://jekyllrb.com/ "Jekyll"), such as generating your site, preview it in your default browser, creating a new post or page from a default template and transfering it to a remote git repository, a remote host/server or a local git repository.

Please note that the code is intentionally "messy" and quite un-DRY so that'll be easier to take the parts that you want and toss away the others.

## Usage

### Tasks

    rake post["Title"]
	rake draft["Title"]
	rake publish
    rake page["Title"]
        rake page["Title","Path/to/folder"]
    rake build
    rake watch
        rake watch[number]
        rake watch["drafts"]
    rake preview
    rake deploy["Commit message"]
    rake transfer

`rake post["Title"]` creates a new post in the `_posts` directory by reading the default template file, adding the title you've specified and generating a filename by using the current date and the title.

`rake draft["Title"]` creates a new post in the `_drafts` directory by reading the default template file, adding the title you've specified and generating a filename.

`rake publish` moves a post from the `_drafts` directory to the `_posts` directory and appends the current date to it. It'll list all drafts and then you'll get to choose which draft to move by providing a number.

`rake page["Title","path/to/folder"]` creates a new page. If the file path is not specified the page will get placed in the site's source directory.

`rake build` just generates the site.

`rake watch` generates the site and watches it for changes. If you want to generate it with a post limit, use `rake watch[1]` or whatever number of posts you want to see. If you want to generate your site with your drafts, use `rake watch["drafts"]`.

`rake preview` launches your default browser and then builds, serves and watches the site.

`rake deploy["Commit message"]` adds, commits and pushes your site to the site's remote git repository with the commit message you've specified. It also uses the `rake build` task to generate the site before it goes through the whole git process.

`rake transfer` uses either `robocopy` or `rsync` to transfer your site to a remote host/server or a local git repository. It also uses the `rake build` task to generate the site before it goes through the whole transfering process.

### _config.yml

    post:
      template:
      extension:
    page:
      template:
      extension:
    editor:
	git:
	  branch:
    transfer:
      command:
      settings:
      source:
      destination:

## Examples

### Post template

    ---
    title:
    layout: post
    ---

### Page template

    ---
    title:
    layout: page
    ---

### Editor

    editor: vim

### Git branch

    git:
      branch: master

### Remote transfer settings (for Windows)

    transfer:
      command: robocopy
      settings: /mir
      source: _site\
      destination: username@servername:\var\www\websitename\

### Remote transfer settings (for *nix)

    transfer:
      command: rsync
      settings: -av --delete
      source: _site/
      destination: username@servername:/var/www/websitename/

### Local transfer settings (for Windows)

    transfer:
      command: robocopy
      settings: /e
      source: _site\
      destination: C:\Git\username.github.com\

### Local transfer settings (for *nix)

    transfer:
      command: rsync
      settings: -av
      source: _site/
      destination: ~/Git/username.github.com/

## Known issues

Rake tasks doesn't play nice when it comes to including commas in arguments. For example, if you try to create a post by running `post["One, two, three"]` the name and title of the post will become `One`. The easiest work-around for this is to skip the commas when your creating a post and adding them later on.

## License

**The MIT License (MIT)**

*Copyright (c) 2012 Ellen Gummesson*

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

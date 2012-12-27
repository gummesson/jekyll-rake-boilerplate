# Jekyll Rake Boilerplate

*Jekyll Rake Boilerplate* is a small rake "boilerplate" for doing common tasks with the static site generator [Jekyll](http://jekyllrb.com/ "Jekyll"), such as generating your site (with an optional post limit), creating a new post from a default template and deploying it to it's remote git repository or a remote host/server.

## Usage

### Tasks

    rake build[number]
    rake post["Post title"]
    rake page["Page title", "Path/to/folder"]
    rake git["Commit message"]
    rake remote

`rake build` generates the site. If you want to generate it with a post limit, use `rake build[1]` or whatever number of posts you want to generate. 

`rake post["Post title"]` creates a new post in the `_posts` directory by reading the default template file, adding the title you've specified and generating a file name by using the current date and the title.

`rake page["Page title", "Path/to/folder"]` creates a new page. If the file path is not specified the page will get placed in the site's root directory.

`rake git["Commit message"]` adds, commits and pushes your site to the site's remote git repository with the commit message you've specified.

`rake remote` uses either `robocopy` or `rsync` to transfer your site to a remote host/server.

### _config.yml

Add the following to your `_config.yml` file:

    post:
      template:
      extension:
    remote:
      command:
      settings:
      source:
      destination:

You can leave out the `remote` parameters if you're planning to only deploy your site with `rake git`.

## Examples

### Post template

    ---
    title:
    layout: post
    ---

### Remote settings (for Windows)

    remote:
      command: robocopy
      settings: /mir
      source: _site/
      destination: username@servername:\var\www\websitename\

### Remote settings (for Unix)

    remote:
      command: rsync
      settings: -av --delete
      source: _site/
      destination: username@servername:/var/www/websitename/

## Known issues

Rake tasks won't unfortunately play nice when it comes to including commas in arguments. For example, if you try to run `post["One, two, three"]` the name and title of the post will become just *"One"*. The easiest work around for this is to skip using commas when your creating a post and adding them later on.

## License

**The MIT License (MIT)**

Copyright (c) 2012 Ellen Gummesson

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
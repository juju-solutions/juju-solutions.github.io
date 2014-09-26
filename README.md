# Juju Solutions Blog

Team blog for the Juju Ecosystem

## Workflow

1. Fork this repository
2. Write markdown files in the `_posts` and `_drafts` directories, the filename _must_ be in the form of:

       YYYY-MM-DD-title.md

    as and example, `2014-10-10-introducting-juju-charms.md`


    the top of your post needs this slug:

        ---
        layout: post
        title: "your title"
        comments: true
        author: "Your Name"
        author_url: "[optional] http://example.com or @gh_username"
        ---

3. Run `jekyll serve` locally to double check your work and check it in your browser at `http://localhost:4000`
3. Issue a pull request when you are ready to publish.

## More info

 - [jekyll from scratch](http://pixelcog.com/blog/2013/jekyll-from-scratch-core-architecture)

 - [github pages w/ jekyll](https://help.github.com/articles/using-jekyll-with-pages)

 - [gh blog for n00bs](http://in-the-attic.com/2013/01/04/building-a-blog-using-jekyll-bootstrap-and-github-pages-a-beginners-guide/)


## Installing Jekyll on your machine

You only need this if you want to preview your work locally before pushing:

    sudo apt-get install ruby ruby-dev make nodejs
    sudo gem install jekyll jekyll-mentions --no-rdoc --no-ri


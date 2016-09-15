# Steps

## Dependencies - install Ruby, bundler and nodejs

    sudo apt-add-repository ppa:brightbox/ruby-ng
    sudo apt-get update
    sudo apt-get install ruby2.3 ruby2.3-dev
    sudo gem install bundler

If not already installed, download and install nodejs binary from nodejs.org

## Clone repo and install gh-pages

    git clone git@github.com:angzhou/angzhou.github.io.git
    cd angzhou.github.io
    bundle install

## Create post and publish

    vi _posts/date-title.md

## Serve locally

    bundle exec jekyll serve
    # check site at localhost:4000, if all good, proceed to next step

## Create post and publish

    git add _posts/date-title.md
    git commit -a -m 'comment'
    git push

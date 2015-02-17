# Openproject Documentation [Proof-of-Concept]

Install
-------

With a Ruby version manager installed, installation should involve just a few steps:

    chruby 2.1.5 # rvm 2.1.5
    gem install bundler
    bundle install

Running
-------

### Building static pages

You can build the site into a `build`-directory:

    bundle exec middleman build

The `build` directory will contain the generated HTML, CSS and JavaScript and
the contents can be uploaded to any standard web server.

### With the built-in server

To run:

    bundle exec middleman server

You will see the generated HTML & CSS by navigating to <http://localhost:4567/>.

# Jekyll::Pagehooks

This plugin isn't useful on its own. It monkeypatches Jekyll's Site, Post, Page and Convertible classes to allow plugin authors to access page and post data before and after render, and after write. 

## Installation

Add this line to your application's Gemfile:

    gem 'jekyll-page-hooks'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install jekyll-page-hooks

## Usage

First require this plugin at the top of a plugin file, inside of Jekyll's plugin directory. Then if your plugin class inherits the PageHooks class, the methods, `pre_render`, `post_render`, `post_write` will execute automatically in turn.

Here's an example.

```
require 'jeykll-page-hooks'

module Jekyll
  class YourFancyPlugin < PageHooks

    # Manipulate page/post data before it has been processed with Liquid or
    # Converters like Markdown or Textile.
    #
    def pre_render(page)
      page.content = highlight_code(page.content)
    end

    # Manipulate page/post data after content has been processed to html.
    #
    def post_render(page)
      page.content = link_headings(page.content)
    end

    # Access page/post data after it has been succesfully written to disk.
    #
    def post_write(page)
      log_something(page.title)
    end

  end
end
```

### When to use what

With `pre_render` you can access page and post data before it has been
processed by Liquid, Markdown, Textile, etc. You might want to do this if your
plugin requires text which conflicts with some content convertors. This way
you can replace that content with the correctly generated HTML before Liquid
or other convertors sees it.

With `post_render` you can access pages and posts after it has been proccessed into HTML. You might use this option if you want to modify generated HTML, for example adding anchors for each heading element.

With `post_write` you can execute a code block after a page or post has been
successfully written to disk. You might use this for logging or triggering
some external process.

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
---
layout: docs
---

# Development Setup

## Ruby 

Running your Ruhoh static blog in ruby is as easy as intalling the Ruhoh gem.
The ruhoh gem depends on:

- **rack** - for web-server integration
- **directory\_watcher** - for watching files for updates in realtime.
- **mustache** - for templating.
- **maruku** - for Markdown parsing.

These gems will be installed along with Ruhoh if you don't already have them.

    $ gem install ruhoh
    
If you run into a problem you can [create a support issue](https://github.com/plusjade/ruhoh.rb/issues) using GitHub Issues.

Once the gem is installed your system should have an executable named `ruhoh`.
Use this to create your new blog scaffold: 

    $ ruhoh new myblog
    $ cd myblog
    $ rackup -p 9292

Your blog is now available at: [http://localhost:9292/](http://localhost:9292/)


# Pages

## Create a Page

Page stubs can automatically be created using the Ruhoh command-line client.

    $ ruhoh page about.md

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_posts</em></li>
  <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>about.md</em> &larr;</li>
</ul>
    
Create a page within a subdirectory:

    $ ruhoh page pages/about.md

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_posts</em></li>
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>pages</em>
    <ul>
      <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>about.md</em> &larr;</li>
    </ul>
  </li>
</ul>
    
Create a page with a "pretty" path:

    $ ruhoh page projects/android

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_posts</em></li>
    <li><span class="ui-silk inline ui-silk-folder">.</span> <em>android</em>
      <ul>
        <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>index.html</em> &larr;</li>
      </ul>
    </li>
</ul>

The client automatically creates a page file with properly formatted filename and YAML Front Matter.

# Posts

## Create a Post

Post stubs can automatically be created using the Ruhoh command-line client.

    $ ruhoh post 'Hello World'

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_posts</em>
    <ul>
      <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>2012-01-01-hello-world.md</em> &larr;</li>
    </ul>
  </li>
</ul>
    
The client automatically creates a file with properly formatted filename and YAML Front Matter.
By default, the date is the current date. You can additionally pass in a custom date:

    $ ruhoh post 'Hello World' 2012-10-10

The date must be in the format: `YYYY-MM-DD` else the client will give you a nice error.

The client will never overwrite existing posts unless you tell it to.

## Create a Draft

A post draft is useful for including a post into your blog that will preview in your development environment
but _will not_ be output via the Compiler. Additionally any meta-data on draft-posts are not added to the site-wide meta-data (e.g tag and category aggregates).

Create drafts as you would a post, but instead use the word 'draft':

    $ ruhoh draft 'Hello World' 2012-10-10

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_drafts</em>
    <ul>
      <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>2012-10-10-hello-world.md</em> &larr;</li>
    </ul>
  </li>
</ul>

When you are ready to publish, just move the file from the `_drafts` folder to the `_posts` folder.

## Tags

Add one or more tags to a post by including them into the post's YAML Front Matter.
This is the YAML block is at the top of the file:

    ---
    layout: post
    title: a nice title
    tags: javascript
    ---
    
To add multiple tags, use an Array:

    ---
    layout: post
    title: a nice title
    tags: [javascript, tutorials, expert]
    ---

or

    ---
    layout: post
    title: a nice title
    tags: 
      - javascript
      - tutorials
      - expert
    ---
    
## Categories

Add one or more categories to a post by including them into the post's YAML Front Matter.
This is the YAML block is at the top of the file:

    ---
    layout: post
    title: a nice title
    categories: code
    ---
   
A category can be multiple levels deep:

    categories: "code/android/games"
    
This defines **one** category named `code/android/games`.

Also note that `code`, and `code/android` **will not exist** unless you explicitly define them as categories themselves.


To place the post in multiple categories you'll need to pass in an Array:

    ---
    layout: post
    title: a nice title
    categories: ['code/android/games', 'game-downloads']
    ---

or

    ---
    layout: post
    title: a nice title
    categories :
      - 'code/android/games'
      - 'game-downloads'
    ---


# Media

## Embed Code

<span class="label label-important">Not Implemented</span>


A templating mechanism for embedding a Gist as well as embedding blocks of code specified in a remote file.
This will work similar to partials but be optimized for code.

## Insert Images

The media folder is used as a convenient place to store your blog's media:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template-light">_media</em><br>
    <ul>
      <li><span class="ui-silk inline ui-silk-picture">.</span> <em class="template">[...my-media-file...]</em> &larr;</li>
    </ul>
  </li>
</ul>

Organize your files any way you wish, then use the special `MEDIA_PATH` template variable to refer the media folder:

{{#raw_code}}
<img src="{{MEDIA_PATH}}/my-media-file.jpg">
{{/raw_code}}
    
Using a dynamic path is helpful when you want to switch to a CDN and or reorganize the way you handle your media.


# Syntax Highlighting

## Google Prettify

Content specified in `<pre></pre>` and/or `<code></code>` blocks is automatically highlighted
via [Google Prettify](http://google-code-prettify.googlecode.com/svn/trunk/README.html)

Google Prettify is a client-side javascript library for highlighting code.

Prettify (tries to) automatically detect the language and highlight the syntax appropriately. 

### Customize Styling Rules

The syntax styles are all contained in the `./_media/google_prettify.css` file.
Extend the styles as needed.

Google Prettify also has some [user-submitted themes](http://google-code-prettify.googlecode.com/svn/trunk/styles/index.html) available for use.


### Disable Google Prettify

Most likely you'd disable Prettify if you intend to use your own highlighting system, whether server-side or 
via another Javascript library.

Disable Google Prettify in your `_config.yml` file:

    # config.yml
    prettify_code : false

### Using other Custom Highlighting Providers

The initial version of Ruhoh supports only Google Prettify because it's the easiest
to get running. It is also accessible to the most users due to its client-side runtime.

[Pygments](http://pygments.org/), in contrast is likely much more powerful but requires server-side dependencies 
that are beyond the scope of Ruhoh beta.  More comprehensive code highlighting options will be available over time.


# Comments

Ruhoh provides widget codes for [Disqus](http://disqus.com), [Intense Debate](http://intensedebate.com), [livefyre](http://www.livefyre.com/), and [Facebook Comments](https://developers.facebook.com/docs/reference/plugins/comments/).

## Add Comments

To enable commenting for your blog you will need to have setup an account with one of these providers.
In the `_config.yml` you should see a hash named `comments` as shown below:

    # Settings for comments helper
    # Set 'provider' to the comment provider you want to use.
    # Set 'provider' to false to turn commenting off globally.

    comments :
      provider : disqus
      disqus :
        short_name : ruhoh
      livefyre :
        site_id : 123
      intensedebate :
        account : 123abc
      facebook :
        appid : 123
        num_posts: 5
        width: 580
        colorscheme: light


### Choose a Provider

Set `provider` to the provider you intend to use. Make sure to specify your account credentials for the relevant provider within the hash named for _that_ provider.

In the example above, the **disqus** provider will be used and will be provided with **ruhoh** as the account **short\_name**.

### Custom Providers

To use a custom provider, set `provider: custom`, then create a partial in the default partials folder:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_templates</em>
    <ul>
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em>partials</em>
        <ul>
          <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>custom_comments</em> &larr;</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

This file will load wherever the theme has included its comments so you can inject your own widget code via this file.

## Disable Comments

Set `provider: false` to disable comments globally. 

Disable comments for individual pages/posts by specifying `comments: false` in the page/post YAML Front Matter:

    ---
    layout: post
    categories : lessons
    comments : false
    tags : [yay]
    ---

Internally, the value of "comments" will be cast to a String. So you must specify exactly the value `false` or `"false"` for this to work.


# Analytics

Ruhoh provides analytics codes for [Google](http://google.com/analytics), and [GetClicky](http://getclicky.com).

## Add Analytics

To enable analytics for your blog you will need to have setup an account with one of these providers.
In the `_config.yml` you should see a hash named `analytics` as shown below:

    # Settings for analytics helper
    # Set 'provider' to the analytics provider you want to use.
    # Set 'provider' to false to turn analytics off globally.
    #        
    analytics :
      provider : google
      google : 
          tracking_id : 'UA-123-12'
      getclicky :
        site_id :


### Choose a Provider

Set `provider` to the provider you intend to use. Make sure to specify your account credentials for the relevant provider within the hash named for _that_ provider.

In the example above, the **google** provider will be used and will be provided with **UA-123-12** as the account **tracking\_id**.

### Custom Providers

To use a custom provider, set `provider: custom`, then create a partial in the default partials folder:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_templates</em>
    <ul>
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em>partials</em>
        <ul>
          <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>custom_analytics</em> &larr;</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

This file will load wherever the theme has included its analytics so you can inject your own widget code via this file.

## Disable Analytics

Set `provider: false` to disable analytics globally. 

Disable analytics for individual pages/posts by specifying `analytics: false` in the post/page YAML Front Matter:

    ---
    layout: post
    categories : lessons
    analytics : false
    tags : [yay]
    ---

Internally, the value of "analytics" will be cast to a String. So you must specify exactly the value `false` or `"false"` for this to work.


# Permalinks

Set the permalink format in the `_config.yml` file:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-page-white-gear">.</span> <em>_config.yml</em> &larr;</li>
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>_posts</em></li>
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em>...</em></li>
</ul>

    permalink: /:categories/:title


## Permalink Variables

<table class="table-striped table-bordered table-condensed">
  <thead>
    <tr>
      <th>Variable</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>

    <tr>
      <td>:year</td>
      <td>Year from the post’s filename</td>
    </tr>
    
    <tr>
      <td>:month</td>
      <td>Month from the post’s filename</td>
    </tr>
    
    <tr>
      <td>:day</td>
      <td>Day from the post’s filename</td>
    </tr>
    
    <tr>
      <td>:title</td>
      <td>Title from the post’s filename</td>
    </tr>
    
    <tr>
      <td>:categories</td>
      <td>The specified categories for this post. If more than one category is set, only the first one is used. If no categories exist, the URL omits this parameter.</td>
    </tr>
    
    <tr>
      <td>:i_month</td>
      <td>Month from the post’s filename without leading zeros.</td>
    </tr>
    
    <tr>
      <td>:i_day</td>
      <td>Day from the post’s filename without leading zeros.</td>
    </tr>
    
  </tbody>  
</table>
        

## Permalink Examples

Given the post filename: `2009-04-29-green-milk-tea.md`   
with categories: `['california/food', 'dairy']`

<table class="table-striped table-bordered">
  <thead>
    <tr>
      <th>Permalink Format</th>
      <th>Output</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>(default)</td>
      <td>/2009/04/29/green-milk-tea.html</td>
    </tr>
    
    <tr>
      <td>/:categories/:title</td>
      <td>/california/food/green-milk-tea/index.html</td>
    </tr>
    
    <tr>
      <td>/:month-:day-:year/:title.html</td>
      <td>/04-29-2009/green-milk-tea.html</td>
    </tr>
    
    <tr>
      <td>/blog/:year/:month/:day/:title</td>
      <td>/blog/2009/04/29/green-milk-tea/index.html</td>
    </tr>

  </tbody>  
</table>



# Layouts

Layout files are used to provide context around a page's content.

A page may specify a layout to render itself into. Additionally this layout may specify its own layout to render _itself_ into, thereby defining its sub-layout.

This makes it possible for a page to have a sub-layout and a master-layout.
Be aware that layouts will not be nested more than two levels deep.

## Create a layout

The Ruhoh command-line client can automatically create layouts for the active theme.

    $ ruhoh layout splash

The command will create a file at:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template-light">_templates</em><br>
    <ul class="template">
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em>themes</em>
        <ul>
          <li><span class="ui-silk inline ui-silk-folder">.</span> <em>[ACTIVE-THEME]</em>
            <ul>
              <li><span class="ui-silk inline ui-silk-folder">.</span> <em>layouts</em>
                <ul>
                  <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em>splash.html</em> &larr;</li>
                </ul>
              </li>
            </ul>  
          </li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

Edit your layout as desired, then make sure to specify your new layout within the pages' YAML Front Matter:

    ---
    layout: splash
    categories : ruby
    ---

## Insert page content into layout

Use the special template variable: {{#raw_code}}{{content}}{{/raw_code}} ... to render a page's content within the given layout.

{{#raw_code}}
---
layout: default
---
<body>
  <div id="sidebar"> ... </div>
  <div id="main">
    {{content}}
  </div>
</body>
{{/raw_code}}

# Partials

Partials are files which contain arbitrary layout code, usually HTML, that can be dynamically included
into any page or layout.

Partials are very useful when used in conjuction with the templating language as they can provide 
standardized layouts for data-structures used throughout your blog.

A good example would be defining a partial for an HTML list which you'd use to render a post's tags or categories.

## Create a Partial

Create a _default\_partial_ by creating a file in the default partials folder at: 

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template-light">_templates</em><br>
    <ul class="template">
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template">partials</em><br>
        <ul>
          <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em class="template">[...your-partial-file...]</em> &larr;</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>
    
These partials should be theme independent.

Additionally you may also create _theme\_specific_ partials by creating files at:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template-light">_templates</em><br>
    <ul class="template">
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em>themes</em><br>
        <ul>
          <li><span class="ui-silk inline ui-silk-folder">.</span> <em>[ACTIVE-THEME]</em>
            <ul>
              <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template">partials</em><br>
                <ul>
                  <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em class="template">[...your-partial-file...]</em> &larr;</li>
                </ul>
              </li>
            </ul> 
          </li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

Theme specific partials are useful when you want to include theme dependent HTML and/or css classes.

## Overload a Partial

Theme specific partials have a higher priority than default partials. That is they will overload default partials of the same name.


# Themes

A theme's primary role is to act as a namespace.
A theme is simply a collection of layouts, partials, and assets that those partials and layouts depend on (CSS, images, javascripts).

## Edit Your Theme

To edit your theme, just edit the layouts, partials, css, etc for the currently active theme.
Your active theme will be the theme specified in `_config.yml`.

The theme structure is as detailed below:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template-light">_templates</em><br>
    <ul class="template">
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template">partials</em></li>
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em>themes</em><br>
        <ul>
          <li><span class="ui-silk inline ui-silk-folder">.</span> <em>twitter</em>
            <ul>
              <li><span class="ui-silk inline ui-silk-folder">.</span> <em>css</em></li>
              <li><span class="ui-silk inline ui-silk-folder">.</span> <em>images</em></li>
              <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template">layouts</em><br>
                <ul>
                  <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em class="template">default.html</em></li>
                  <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em class="template">page.html</em></li>
                  <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em class="template">post.html</em></li>
                </ul>
              </li>
              <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template">partials</em><br>
                <ul>
                  <li><span class="ui-silk inline ui-silk-page-white-text">.</span> <em class="template">posts_collate</em></li>
                </ul>
              </li>
            </ul> 
          </li>
          <li><span class="ui-silk inline ui-silk-folder">.</span> <em>another-theme</em></li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

## Install New Theme

To install a new theme just download the folder and place it in the "themes" directory:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template-light">_templates</em><br>
    <ul class="template">
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em>themes</em><br>
        <ul>
          <li><span class="ui-silk inline ui-silk-folder">.</span> <em>[...NEW-THEME-NAME...]</em> &larr;</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

Then update your `_config.yml` to set the theme to this new theme name

    theme : new-theme-name


## Create New Theme

The Ruhoh command-line client can automatically create scaffolding for building a new theme.

    $ ruhoh theme new-theme-name

Scaffolding for _new-theme-name_ will be available at:

<ul class="folder-tree">
  <li><span class="ui-silk inline ui-silk-folder">.</span> <em class="template-light">_templates</em><br>
    <ul class="template">
      <li><span class="ui-silk inline ui-silk-folder">.</span> <em>themes</em><br>
        <ul>
          <li><span class="ui-silk inline ui-silk-folder">.</span> <em>new-theme-name</em> &larr;</li>
        </ul>
      </li>
    </ul>
  </li>
</ul>

# Mustache Overview

Ruhoh uses [Mustache](http://mustache.github.com/) as its primary Templating system.
If you are unfamiliar with Mustache's philosophy and syntax you can get up to speed in about 10 minutes by going through the 
[README Examples](https://github.com/defunkt/mustache#readme)

Mustache takes in three main parameters when expanding a template:

<dl class="dl-horizontal">
  <dt>Template</dt>
  <dd>This is just a string of content. In Ruhoh this will be the layout content plus the injected page content.</dd>
  
  <dt>View</dt>
  <dd>This is a ruby class which defines helper methods that can be used in the layout.</dd>
  
  <dt>Payload</dt>
  <dd>This is a Hash of your blog's data objects which are accessible in the Mustache Template.</dd>
</dl>

All pages in Ruhoh are expanded using one single, global Mustache View (ruby class).
This class defines helper methods useful for displaying your blog's data efficiently.

# Template Data

Your posts and pages exist as "data objects" in the Ruhoh system, which in turn have
other data objects associated with them, namely a URL, categories, tags, and so on.

These data objects are passed into the Templating system along with your layouts and partials
which all come together to render the final page views.

## Payload

This following hash outlines the **top-level endpoints** accessible within the Templating System.
The unabridged version is comprehensively documented in the API section.

    {
      "page"    => {},
      "site"    => {},
      "pages"   => {},
      "_posts"  => {
        "dictionary" => {...},
        "chronological" => [...],
        "collated" => [...],
        "tags" => {
          "tag1" => {...},
          "tag2" => {...},
        },
        "categories" => {
          "category1" => {...},
          "category2" => {...},
        }
      },
      "ASSET_PATH" => "/_templates/themes/some-theme/",
      "MEDIA_PATH" => "/_media/"
    }
    
Next we'll document how to use this data throughout your pages using the Templating system.


# Mustache Helpers

Ruhoh extends Mustache to include context-aware helper methods.
Helper methods act on a given context (a data-structure), usually transforming it into
a new data-structure then passing it back to the block context as if that data was was passed in directly.

This strategy allows us to pass around **ids** of objects rather than the objects themselves.
Now whenever we need an object, we use the helpers to _expand_ those ids into their full objects.
Let's take a look at some common usage examples:


## ?to_posts

This helper method takes in a single or Array of post ids and expands them to their corresponding Post Objects.

### List site-wide posts

{{#raw_code}}
  <ul>
  {{# _posts?to_posts }}
    <li><a href="{{url}}">{{title}}</a></li>
  {{/ _posts?to_posts }}
  </ul>
{{/raw_code}}


### List posts from a given category

{{#raw_code}}
  <ul>
  {{# _posts.categories.ruby.posts?to_posts }}
    <li><a href="{{url}}">{{title}}</a></li>
  {{/ _posts.categories.ruby.posts?to_posts }}
  </ul>
{{/raw_code}}

## ?to_pages

This helper method takes in a single or Array of page ids and expands them to their corresponding Page Objects.

### List site-wide pages.

{{#raw_code}}
  <ul>
  {{# pages?to_pages }}
    <li><a href="{{url}}">{{title}}</a></li>
  {{/ pages?to_pages }}
  </ul>
{{/raw_code}}

### List a user-specified list of pages.

Assume we define a navigation array in `_site.yml`:

    navigation:
      - index.md
      - about.md
      - projects/startup.html
      - contact.md

We can can expand these page ids:

{{#raw_code}}
  <ul>
  {{# site.navigation?to_pages }}
    <li><a href="{{url}}">{{title}}</a></li>
  {{/ site.navigation?to_pages }}
  </ul>
{{/raw_code}}


## ?to_categories

This helper method takes in a single or Array of category names and expands them to their corresponding Category Objects.

### List site-wide categories.

{{#raw_code}}
  <ul>
  {{# _posts.categories?to_categories }}
    <li><a href="{{url}}">{{name}} <span>{{count}}</span></a></li>
  {{/ _posts.categories?to_categories }}
  </ul>
{{/raw_code}}

### List categories on a given post.

Assuming the current `page` object is a post:

{{#raw_code}}
  <ul>
  {{# page.categories?to_categories }}
    <li><a href="{{url}}">{{name}} <span>{{count}}</span></a></li>
  {{/ page.categories?to_categories }}
  </ul>
{{/raw_code}}

### List categories on a collection of posts.

{{#raw_code}}
  {{# posts?to_posts }}
    <h3>{{title}}</h3>
    <h4>Tags</h4>
    <ul>
    {{# categories?to_categories }}
      <li><a href="{{url}}">{{name}} <span>{{count}}</span></a></li>
    {{/ categories?to_categories }}
    </ul>
  {{/ posts?to_posts }}
{{/raw_code}}


## ?to_tags

This helper method takes in a single or Array of tag names and expands them to their corresponding Tag Objects.

### List site-wide tags.

{{#raw_code}}
  <ul>
  {{# _posts.tags?to_tags }}
    <li><a href="{{url}}">{{name}} <span>{{count}}</span></a></li>
  {{/ _posts.tags?to_tags }}
  </ul>
{{/raw_code}}

### List tags on a given post.

Assuming the current `page` object is a post:

{{#raw_code}}
  <ul>
  {{# page.tags?to_tags }}
    <li><a href="{{url}}">{{name}} <span>{{count}}</span></a></li>
  {{/ page.tags?to_tags }}
  </ul>
{{/raw_code}}

### List tags on a collection of posts.

{{#raw_code}}
  {{# posts?to_posts }}
    <h3>{{title}}</h3>
    <h4>Tags</h4>
    <ul>
    {{# tags?to_tags }}
      <li><a href="{{url}}">{{name}} <span>{{count}}</span></a></li>
    {{/ tags?to_tags }}
    </ul>
  {{/ posts?to_posts }}
{{/raw_code}}


# Hosting


After you've added posts or made changes to your theme or other files, simply commit them to your git repo and push the commits up to GitHub.

    $ git add .
    $ git commit -m "Add new content"
    $ git push origin master

A GitHub post-commit hook will automatically deploy your changes to your hosted blog. You will receive a success or failure notice for every commit you make to your blog.

## ruhoh.com
## github.com
## Amazon S3
## rsync
## scp
## dropbox
## webhooks
## heroku.com
## ftp
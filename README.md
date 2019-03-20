# Thesis

<p align="center">
  <br/>
  <a href="http://community.infinite.red/">
    <img src="https://infiniteredcommunity.herokuapp.com/badge.svg">
  </a>
  <img src="https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat" alt="js-standard-style"/>
  <a href="https://semaphoreci.com/ir/thesis-phoenix"><img alt="Build Status" src="https://semaphoreci.com/api/v1/projects/0540dbb0-887a-45dd-9190-baa19b2ca9fb/816876/badge.svg"></a>
  <br/>
  <br/>
</p>

<table>
  <tr><td><h4>What Is Thesis?</h4></td></tr>
  <tr>
    <td>
      A CMS for Elixir/Phoenix that doesn't hijack your development workflow.
      <br/>
      <br/>
      Thesis is a lightweight and flexible Elixir/Phoenix CMS for quickly and easily
      adding content editing to any page on a Phoenix website, as well as creating new
      dynamically routed pages. It's ideal for either adding limited editing support to
      existing Phoenix websites or building dynamic websites.
      <br/>
      <br/>
      Watch Jamon Holmgren give a 5-minute lightning talk about Thesis at ElixirConf 2017: https://www.youtube.com/watch?time_continue=2656&v=YqOwzXNkOyg
    </td>
  </tr>

  <tr><td><h4>Thesis Features</h4></td></tr>
  <tr>
    <td>
      <ul>
        <li>Elixir/Phoenix hex package, uses React.js for its user interface</li>
        <li>Lightweight, bolt-on, doesn't hijack your development workflow</li>
        <li>On-page rich text editing</li>
        <li>On-page plain text editing</li>
        <li>Raw HTML editing for Youtube embeds or other flexible uses</li>
        <li>Image URL editing, both `img` tag and `div` with background image</li>
        <li>Page meta title and description editing</li>
        <li>Easily bring your own authentication system in one tiny function</li>
        <li>Create new dynamic pages, delete dynamic pages</li>
      </ul>
    </td>
  </tr>

  <tr><td><h4>What Thesis Isn't</h4></td></tr>
  <tr>
    <td>
      You can't have it all. Thesis isn't the same as other -bloated- full-function
      content management systems out there. This is a list of what it's not now and
      not likely to be in the future.
      <br/><br/>
      <ul>
        <li>Not a complete WordPress Replacement</li>
        <li>Not a full featured CMS</li>
        <li>Not a full featured WYSIWYG editor</li>
        <li>Not an authentication or permission system</li>
        <li>Not supported outside of a Phoenix app</li>
      </ul>
    </td>
  </tr>

  <tr><td><h4>Changelog</h4></td></tr>
  <tr>
    <td>
      <strong>0.3.1</strong><br/>
      - Fixes issue #155 [pr #156](https://github.com/infinitered/thesis-phoenix/pull/156) Ambiguous call to function repo/0 on `mix ecto.migrate` - Phoenix 1.4
      <br/><br/>
      <strong>0.3.0</strong><br/>
      - Removes the LZString compression for backups (page revisions) as per the conversation <a href="https://github.com/infinitered/thesis-phoenix/issues/129">here</a>. <br/>
      - <em>Adds migration (run `mix thesis.install && mix ecto.migrate`)</em>. It is important to do this before 0.4.0 as the LZString dependency will be removed then.<br/>
      - Adds <a href="https://github.com/infinitered/thesis-phoenix/issues/147">notifications</a>. This allows us to warn users of potential Thesis issues depending on page state, environment, Thesis version, etc. Notifications can also be set in the host app config to include custom logic. <br/>
      - Bug fixes: <a href="https://github.com/infinitered/thesis-phoenix/issues/128">page revision exception</a>, <a href="https://github.com/infinitered/thesis-phoenix/issues/148">add new page button hover issue</a>.
      <br/><br/>
      <strong>0.2.0</strong><br/>
      - Adds a backup (page revision history) feature. Every time a page is saved, a snapshot of the page and content is captured and can be restored later. <br/>
      - <em>Adds migration (run `mix thesis.install && mix ecto.migrate`)</em>.
      <br/><br/>
      <strong>pre 0.2.0</strong><br/>
      - Core Thesis functionality: content areas, global content areas, file uploads, import/export, dynamic pages, page templates.
    </td>
  </tr>

  <tr><td><h4>Caveats</h4></td></tr>
  <tr>
    <td>
      Thesis is currently considered <em>Beta-quality</em>, but is in production on several
      websites, including <a href="https://infinite.red">Infinite Red</a>. The API is
      largely stable and we are committed to providing a clear upgrade path and documentation.
    </td>
  </tr>
</table>

![screen capture on 2016-04-20 at 15-11-10 copy](https://cloud.githubusercontent.com/assets/1775841/24872094/0118ef42-1dd1-11e7-975a-26de44c9d9dc.gif)

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20Installation+%26+Configuration%20-)

_If you are having problems, view `README_INSTALL.md` for manual instructions._

##### 1. Add thesis to your `mix.exs`:

```elixir
def deps do
  [{:thesis, "~> 0.3.3"}]
end

def application do
  [applications: [:thesis]]
end
```

##### 2. Run `mix thesis.install`

This install script will add Thesis to your `config/config.exs` and `lib/yourapp_web.ex`, as well
as generate migrations and an authorization module in your `lib/thesis_auth.ex`.

##### 3. Add the Thesis editor to your layout

```eex
  <body>
    <%= thesis_editor(@conn) %>
```

##### 4. Run `mix ecto.migrate`

```
$ mix ecto.migrate
```

<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20%20%20%20%20Demo+App%20%20%20%20%20%20%20%20%20%20-)

Check out the example app in `apps/example` to see how Thesis can be implemented.
We'll keep this up to date with examples of the latest features as we develop Thesis.
<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20Making+Pages+Editable%20%20%20%20-)

Use the `Thesis.View.content/4` view helper function to make a content area
editable. If you have `use Thesis.View` in your `lib/yourapp_web.ex` file, this function
is already available on all of your views.

Thesis will add a wrapper `<div>` around editable HTML and plain-text content
areas, both in read mode and edit mode, so plan your CSS accordingly.

# Rich Text Areas

Simply wrap your HTML in a `content` function call, specifying `html` as the content type.

```eex
<h1>Title</h1>
<p>
  Here's my default description!
</p>
```

becomes...

```eex
<%= content(@conn, "Section identifier", :html) do %>
  <h1>Title</h1>
  <p>
    Here's my default description!
  </p>
<% end %>
```

### Custom HTML Editor

Don't like the MediumEditor? Write your own custom editor implementing the common editor interface.

```js
class MyCustomEditor {
  constructor(opts) {
    this.onChange = opts.onChange;
  }
  enable() {} // Setup Editor
  disable() {} // Teardown Editor
  content(editor) {} // Return content
  set(name, data) {} // Set content
}
```

For more detail, check out [HtmlEditor](https://github.com/infinitered/thesis-phoenix/tree/master/assets/js/content_types/html_editor.js) or [this gist](https://gist.github.com/ryanlntn/ac346d361d9e10a8f1888bf59cea0e37) implementing a custom editor using [Trumbowyg](https://alex-d.github.io/Trumbowyg/).

To enable, add this in your config/config.exs file:

```elixir
config :thesis,
  html_editor: "MyCustomEditor"
```

<br/>

---

# Plain Text Areas

For plain-text, provide a `do:` option for default text.

```eex
<h1>My Title</h1>
```

becomes...

```eex
<h1><%= content(@conn, "Title identifier", :text, do: "My Title") %></h1>
```

<br/>

---

# Custom HTML Areas

For video embeds, iframes, and any other custom HTML, use the `:raw_html` content type:

```eex
<iframe width="560" height="315" src="https://www.youtube.com/embed/5SVLs_NN_uY" frameborder="0" allowfullscreen></iframe>
```

becomes...

```eex
<%= content(@conn, "Section identifier", :raw_html) do %>
  <iframe width="560" height="315" src="https://www.youtube.com/embed/5SVLs_NN_uY" frameborder="0" allowfullscreen></iframe>
<% end %>
```

<br/>

---

# Images

You can have the user specify an image URL and display the image with the `image` content type.

```eex
<img src="http://placekitten.com/200/300">
```

becomes...

```eex
<%= content(@conn, "Image identifier", :image, alt: "My alt tag", do: "http://placekitten.com/200/300") %>
```

If you prefer to use a `div` with a background image, you can use the `background_image`
content type.

```eex
<div style="background-image: url(http://placekitten.com/200/300)"></div>
```

becomes...

```eex
<%= content(@conn, "Image identifier", :background_image, do: "http://placekitten.com/200/300") %>
```

### Image Uploads

Thesis offers support for a few different ways to handle image uploads: store files in the database,
point to an uploader/adapter inside your custom app, or use one of the prebuilt adapters (in progress).

##### Store Files in Database

For smaller websites and/or website that are hosted on the cloud, thesis offers a no-setup-required image uploader.
Files are stored in a separate table and contain all of the needed metadata (name, file type, and blobs themselves).
Keep in mind as you upload more and more files, your database will grow quickly. Don't use this for high-traffic,
content-heavy web applications. Smaller personal websites are probably fine.

To enable, add this in your config/config.exs file:

```elixir
config :thesis,
  uploader: Thesis.RepoUploader
```

##### Use Your Own Uploader Module

If you already set up file uploads in your custom app, point thesis to a module that can handle a `%Plug.Upload{}`
struct.

```elixir
config :thesis,
  uploader: <MyApp>.<CustomUploaderModule>
```

The module should have an `upload/1` function that accepts a `%Plug.Upload{}` struct. This function should return either `{:ok, "path/to/file.jpg"}` tuple with an image url or path, or `{:error, _}`. You can view
[/lib/thesis/uploaders/repo_uploader.ex](https://github.com/infinitered/thesis-phoenix/blob/master/lib/thesis/uploaders/repo_uploader.ex)
for an example.

That's it! Restart your server and image content areas will now contain a
file upload field.
<br/>

---

# Global Content Areas

Content areas in Thesis are page-specific. However, if you want an editable
area that can be displayed on multiple pages, use the
`Thesis.View.global_content/4` function. Any page using that content area identifier
will display the edited content across the whole website.

```eex
<%= global_content(@conn, "Footer Text", :html) do %>
  <h4>Contact Info</h4>
  <ul>
    <li>Call us at (800) 555-1212</li>
    <li>Email us at hello@example.com.</li>
  </ul>
<% end %>
```

# Custom Classes or ID

Thesis adds an additional `<div>` around your editable content areas. We suggest that
you not style these divs heavily, since Thesis uses them as editors and adds its own styles
in edit-mode. However, sometimes, you need to modify that markup slightly for better presentation.
You can provide an ID and additional classes by specifying `id` and `classes`, respectively.

```eex
<%= content(@conn, "Ident", :html, id: "my-id", classes: "more classes") do %>
  <h1>Title</h1>
<% end %>
```

<br/>

---

# Page Meta Title and Description

Thesis provides a settings tray to edit each page's title and description. In your
layout, you can output the current title and description like so:

```eex
<title><%= page_title(@conn, "Default Title") %></title>
<meta name="description" content="<%= page_description(@conn, "Default Description") %>" />
```

Some prefer to set the page title and description as assigns in their controller actions:

```eex
def about(conn, params) do
  @title = Thesis.View.page_title(conn, "About My Company")
  @description = Thesis.View.page_description(conn, "A relevant description here.")
end
```

<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20%20Dynamic+Pages%20%20%20%20%20%20%20-)

Thesis supports users creating and deleting dynamically routed pages. These
differ from static pages in that they are routed by Thesis rather than Phoenix,
and live only in your database. They can be rendered with different templates.

![add new page screenshot](http://d.pr/i/XLBq+)

To enable dynamic pages, add (or uncomment) this in your `config/config.exs` file:

```elixir
config :thesis, :dynamic_pages,
  view: <MyApp>.PageView,
  templates: ["index.html", "otherview.html"],
  not_found_view: <MyApp>.ErrorView,
  not_found_template: "404.html"
```

Replace `<MyApp>` with your app name. Use any view you want, and put any templates
contained in that view that you want to make available in the `templates` list.
These will be displayed as a drop-down to the user when they are creating the new
dynamic page.

You'll also need to make one change to your router.ex and a controller of your
choice.

```elixir
# web/router.ex
# should be added as the last route

  get "/*path", <MyApp>.PageController, :dynamic

# web/controllers/page_controller.ex (or similar)

  def dynamic(conn, _params) do
    render_dynamic(conn)
  end
```

You can pass in a default template (otherwise, it'll use the first template
option in your config) with `render_dynamic(conn, template: "index.html")`.

You can choose to make only a portion of your website support dynamic pages by
routing more specifically. For example, if you want a blog section:

```elixir
# web/router.ex

  get "/blog/*path", <MyApp>.BlogController, :dynamic
```

<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20%20Authorization%20%20%20%20%20%20%20-)

You probably don't want your website editable by the world. Thesis doesn't
force you to use any particular authorization strategy.

Instead, Thesis will call your auth module's `page_is_editable?/1` function
and provide the current `conn`, which can be used to extract current user
session data as well as the current page, and then you can decide how that
should affect authorization.

Here's an example which we use on our own website, [https://infinite.red](https://infinite.red):

```elixir
defmodule IrWebsite.ThesisAuth do
  @behaviour Thesis.Auth

  def page_is_editable?(conn) do
    IrWebsite.AuthController.logged_in?(conn)
  end
end
```

In our `auth_controller.ex` file, the `logged_in?/1` function looks something
like this:

```elixir
  def logged_in?(conn) do
    !!current_user(conn)
  end

  def current_user(conn) do
    get_session(conn, :current_user)
  end
```

So, in this case, we're simply checking to see if the user has been logged in
or not. Since only Infinite Red employees have logins, it's safe for us to
assume that if they're logged in, they have permission to edit the page.

If you use [Guardian](https://github.com/ueberauth/guardian) or something similar,
you may need additional manipulations to your `conn` to properly authenticate the
user. Add those to your auth module like this:

```eex
defmodule MyApp.ThesisAuth do
  @moduledoc """
  Contains functions for handling Thesis authorization.
  """

  def page_is_editable?(conn) do
    conn
    |> Guardian.Plug.VerifySession.call(%{})
    |> Guardian.Plug.LoadResource.call(%{})
    |> MyApp.SessionController.logged_in_and_admin?
  end
end
```

<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20%20Notifications%20%20%20%20%20%20%20-)

Notifications/alerts allow us to talk with the user about the various aspects of the editing experience. For us, the contributors, this means that we may warn the user or developer of a breaking change that requires migrations (if they were forgotten to be executed). Since this is configurable, the developer may elect to push custom notifications to various parts of the Thesis editor.

![](https://user-images.githubusercontent.com/1775841/38072424-07657800-32db-11e8-8fe1-bdea4bfc76ab.png)

Notifications can be configured to be static:

```elixir
config :thesis, :notifications,
  page_settings: ["The changes made here will affect your SEO", "Example notification 2"],
  add_page: ["You are about to add a new page to the product site!"],
  import_export_restore: ["Example notification 4"]
```

Or, you may elect to add some logic and make them more dynamic:

```elixir
config :thesis, :notifications,
  page_settings: ["Example notification 1", "Example notification 2"],
  add_page: &MyApp.CustomModule.generate_notifications/1,
  import_export_restore: &MyApp.CustomModule.import_warning/1
```

In either case, there are only 2 things that matter: 1 - you must provide a List of String(s) for each notification type, whether static or the result of a custom function; 2 - if you are using a custom function, it must be able to accept 1 argument: a `%Plug.Conn{}` struct. You can see an example [here](https://github.com/infinitered/thesis-phoenix/blob/master/examples/example-phx-1_3/lib/example_phx/notifications.ex).

<strong>Note</strong>: right now, there are 3 spots to which you can push notifications: the '[Add New Page](https://user-images.githubusercontent.com/1775841/38072348-acfb3d46-32da-11e8-8fed-97b5d17ba027.png)' tray, '[Page Settings](https://user-images.githubusercontent.com/1775841/38072314-8b056a86-32da-11e8-8e75-df4383eabdee.png)' tray, and '[Import/Export/Restore](https://user-images.githubusercontent.com/1775841/38072274-67b43a62-32da-11e8-8752-664930d4ce9d.png)' tray. As more features are developed, the notifications will be extended to support those features as well.

<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20%20Common+Issues%20%20%20%20%20%20%20-)

#### Thesis's menu/editor/tray is borked

This is pretty common. While we try to be good citizens by properly namespacing
all Thesis elements, we embed Thesis code into your existing web page, and so
we're at the mercy of your application's existing CSS.

Inspect the element(s) that are screwed up and see if any of your styles are
conflicting. For example, here's a screenshot of an issue:

[borked Thesis editor](https://cloud.githubusercontent.com/assets/1479215/18256127/223706ca-7366-11e6-971e-018ef8e656cb.png)

Note that there is a `.tooltip` CSS rule originating in a different CSS file
that is affecting our editor.

In future releases, we will namespace all Thesis classes and IDs. But if your
application is overriding whole elements (like `div` or `img`), it's up to you
to fix the issue in your own CSS.
<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20%20Contributing%20%20%20%20%20%20%20-)

We're committed to making Thesis the go-to content editing system for Phoenix
websites. Please help us improve!

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Run `npm run webpack` during development
5. Use the `apps/example` Phoenix app to manually test your feature
6. Write tests for your new feature
7. Run `./bin/ci` in the root directory to ensure that Thesis tests pass.
8. Push to the branch (`git push origin my-new-feature`)
9. Create new Pull Request
   <br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20Premium+Support%20%20%20%20%20%20-)

[Thesis Phoenix](https://github.com/infinitered/thesis-phoenix), as an open source project, is free to use and always will be. [Infinite Red](https://infinite.red/) offers premium Thesis Phoenix support and general web app design/development services. Email us at [hello@infinite.red](mailto:hello@infinite.red) to get in touch with us for more details.
<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20Key+Contributors%20%20%20%20%20%20-)

| [Jamon Holmgren](https://twitter.com/jamonholmgren)                                 | [Yulian Glukhenko](https://github.com/yulolimum)                                     | [Ken Miller](https://github.com/kemiller)                                         | [Daniel Berkompas](https://twitter.com/dberkom)                                      |
| ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| <img src="https://infinite.red/images/avatars/img-avatar-jamon@2x.jpg" width="165"> | <img src="https://infinite.red/images/avatars/img-avatar-yulian@2x.jpg" width="165"> | <img src="https://infinite.red/images/avatars/img-avatar-ken@2x.jpg" width="165"> | <img src="https://infinite.red/images/avatars/img-avatar-daniel@2x.jpg" width="165"> |

Also supported by others on the [Infinite Red](https://infinite.red) team.
<br/>

---

# ![](http://placehold.it/890x200/2b1e34/ffffff?text=-%20%20%20%20%20%20License:+MIT%20%20%20%20%20%20-)

Copyright (c) 2016 Infinite Red, Inc.

Thesis depends on Elixir, which is under the Apache 2 license, and
Phoenix, which is MIT.

See LICENSE.md for more information.

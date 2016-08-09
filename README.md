
We will use zurb-foundation version 6 with Rails 5. The foundation-rails gem version used is 6.2.3.0. Create a new Rails 5 project.

```
rails new zurb6
```

Create a product resource.

```
rails g scaffold product name price:decimal --skip-stylesheets
```

Migrate the database.

```
rails db:migrate
```

Add:

```ruby
gem 'foundation-rails'
```

to Gemfile. Run bundle. Install zurb foundation.

```
rails g foundation:install
```

Say yes to overwrite application.html.erb. Include:

```rhtml
<%= javascript_include_tag "vendor/modernizr" %>
```

inside the head section of the layout file.

Add navigation to the layout file:

```rhtml
<div class="top-bar">
  <div class="top-bar-left">
    <ul class="dropdown menu" data-dropdown-menu>
      <li class="menu-text">Site Title</li>
      <li>
        <%= link_to "Browse Products", products_path %>
      </li>
      <li><%= link_to "Price List" %></li>
      <li><%= link_to "Contact Us" %></li>
      <li><%= link_to "Cart" %></li>
    </ul>
  </div>
</div>
```

It now looks like this:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Store</title>
    <%= stylesheet_link_tag    "application" %>
    <%= javascript_include_tag "application", 'data-turbolinks-track' => true %>
	<%= javascript_include_tag "vendor/modernizr" %>
    <%= csrf_meta_tags %>
  </head>
  <body>
  <div class="top-bar">
    <div class="top-bar-left">
      <ul class="dropdown menu" data-dropdown-menu>
        <li class="menu-text">Site Title</li>
        <li>
          <%= link_to "Browse Products", products_path %>
        </li>
        <li><%= link_to "Price List" %></li>
        <li><%= link_to "Contact Us" %></li>
        <li><%= link_to "Cart" %></li>
      </ul>
    </div>
  </div>
  <%= yield %>
  </body>
</html>
```

Define the route for the home page.

```ruby
Rails.application.routes.draw do
  resources :products
  root 'products#index'
end
```

Go to the home page. You will see the home page styled using Zurb foundation. Add modernizr.js to assets.rb:

```ruby
Rails.application.config.assets.precompile += %w( vendor/modernizr.js )
```

Add a sidebar to the layout file:

```rhtml
<!-- Sidebar -->
<aside class="large-3 columns">
  <h5>Categories</h5>
  <ul class="side-nav">
    <li><a href="#">News</a></li>
    <li><a href="#">Code</a></li>
    <li><a href="#">Design</a></li>
    <li><a href="#">Fun</a></li>
    <li><a href="#">Weasels</a></li>
  </ul>
  <div class="panel">
    <h5>Featured</h5>
    <p>Pork drumstick turkey fugiat. Tri-tip elit turducken pork chop in. Swine short ribs meatball irure bacon nulla pork belly cupidatat meatloaf cow.</p>
    <a href="#">Read More &rarr;</a>
  </div>
</aside>
<!-- End Sidebar -->
```

It now looks like this:

```rhtml
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Store</title>
    <%= stylesheet_link_tag    "application" %>
    <%= javascript_include_tag "application", 'data-turbolinks-track' => true %>
	<%= javascript_include_tag "vendor/modernizr" %>
    <%= csrf_meta_tags %>
  </head>
  <body>
  <div class="top-bar">
    <div class="top-bar-left">
      <ul class="dropdown menu" data-dropdown-menu>
        <li class="menu-text">Site Title</li>
        <li>
          <%= link_to "Browse Products", products_path %>
        </li>
        <li><%= link_to "Price List" %></li>
        <li><%= link_to "Contact Us" %></li>
        <li><%= link_to "Cart" %></li>
      </ul>
    </div>
  </div>
  <%= yield %>
  <!-- Sidebar -->
    <aside class="large-3 columns">
      <h5>Categories</h5>
      <ul class="side-nav">
        <li><a href="#">News</a></li>
        <li><a href="#">Code</a></li>
        <li><a href="#">Design</a></li>
        <li><a href="#">Fun</a></li>
        <li><a href="#">Weasels</a></li>
      </ul>
      <div class="panel">
        <h5>Featured</h5>
        <p>Pork drumstick turkey fugiat. Tri-tip elit turducken pork chop in. Swine short ribs meatball irure bacon nulla pork belly cupidatat meatloaf cow.</p>
        <a href="#">Read More &rarr;</a>
      </div>
    </aside>
    <!-- End Sidebar -->
  </body>
</html>
```

Let's style the form. Change the form partial:

```rhtml
<%= form_for(product) do |f| %>
  <% if product.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(product.errors.count, "error") %> prohibited this product from being saved:</h2>
      <ul>
      <% product.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>
  <div class="row">
	<div class='large-4 columns'>
      <%= f.label :name %>
      <%= f.text_field :name %>		
	</div>
  </div>
  <div class="row">
	<div class='large-4 columns'>
      <%= f.label :price %>
      <%= f.text_field :price %>		
	</div>
  </div>
  <div class="row">
	<div class='large-4 columns'>
	  <%= f.submit 'Submit', class: 'button' %>	
	</div>  
  </div>
<% end %>
```

Display the sidebar only in the products index page:

```rhtml
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>My Store</title>
    <%= stylesheet_link_tag    "application" %>
    <%= javascript_include_tag "application", 'data-turbolinks-track' => true %>
	<%= javascript_include_tag "vendor/modernizr" %>
    <%= csrf_meta_tags %>
  </head>
  <body>
  <div class="top-bar">
    <div class="top-bar-left">
      <ul class="dropdown menu" data-dropdown-menu>
        <li class="menu-text">Site Title</li>
        <li>
          <%= link_to "Browse Products", products_path %>
        </li>
        <li><%= link_to "Price List" %></li>
        <li><%= link_to "Contact Us" %></li>
        <li><%= link_to "Cart" %></li>
      </ul>
    </div>
  </div>
  <%= yield %>
  <%= yield :sidebar %>	
  </body>
</html>
```

Add `content_for` in products/index.html.erb to display the sidebar:

```rhtml
<p id="notice"><%= notice %></p>
<h1>Products</h1>
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Price</th>
      <th colspan="3"></th>
    </tr>
  </thead>
  <tbody>
    <% @products.each do |product| %>
      <tr>
        <td><%= product.name %></td>
        <td><%= product.price %></td>
        <td><%= link_to 'Show', product %></td>
        <td><%= link_to 'Edit', edit_product_path(product) %></td>
        <td><%= link_to 'Destroy', product, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>
<br>
<% content_for :sidebar do %>
  <!-- Sidebar -->
    <aside class="large-3 columns">
      <h5>Categories</h5>
      <ul class="side-nav">
        <li><a href="#">News</a></li>
        <li><a href="#">Code</a></li>
        <li><a href="#">Design</a></li>
        <li><a href="#">Fun</a></li>
        <li><a href="#">Weasels</a></li>
      </ul>
      <div class="panel">
        <h5>Featured</h5>
        <p>Pork drumstick turkey fugiat. Tri-tip elit turducken pork chop in. Swine short ribs meatball irure bacon nulla pork belly cupidatat meatloaf cow.</p>
        <a href="#">Read More &rarr;</a>
      </div>
    </aside>
    <!-- End Sidebar -->
<% end %>
```

We can style the 'New Product' link as button:

```rhtml
<%= link_to 'New Product', new_product_path, class: 'button' %>
```

Change the value for:

```css
$grid-column-gutter: 30px;
```
in _settings.scss

This will provide the gap on the left for the new product form. You can add tool tip to the price label in the new product form:

```rhtml
<span data-tooltip aria-haspopup="true" class="has-tip" data-disable-hover="false" tabindex="1" title="Price in USD.">
  <%= f.label :price %>	
</span> 
```

Reload the new product page. Now, if you hover over the Price label, you will see the tooltip.

## Summary

In this article, you learned how to setup and use Zurb 6 with Rails 5 projects.

## References

- [Zurb Top Bar](http://foundation.zurb.com/sites/docs/top-bar.html 'Zurb Top Bar')
- [Zurb Building Blocks](http://zurb.com/building-blocks 'Zurb Building Blocks')


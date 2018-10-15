# Rails Forms Overview

Imagine that you're on a roadtrip across the country (I'm already jealous) with a starting point of Santa Barbara and a final destination of New York City. If you enter the addresses into Google Maps, you'll be shown multiple routes, and each route has an associated duration.

How do you select which route to take? Some of the points that should be included in your decision are below:

* Duration

* Cities that you go through

* Landmarks that you want to drive through

Forms in Rails are similar to your roadtrip. Rails supplies a few different options to choose from when creating forms. How do you know the right option to select? Just like your roadtrip you consider the strengths of each form option and see how well it aligns with your intended behavior and the application requirements.

In this lesson we will review:

* Both of the main form implementations in Rails

* Discuss when one type of form should be selected over the other

* Walk through the different form options


## `form_tag`

Attributes of the `form_tag` helper:

- Most basic form helper that's available in Rails

- Uses tag form elements to build out a form

- Unlike the `form_for` helper, it does not use a form builder

Let's build out a form that lets users enter in their cat's name and their associated color:

```erb
<%= form_tag("/cats") do %>
  <%= label_tag('cat[name]', "Name") %>
  <%= text_field_tag('cat[name]') %>

  <%= label_tag('cat[color]', "Color") %>
  <%= text_field_tag('cat[color]') %>

  <%= submit_tag "Create Cat" %>
<% end %>
```

This will build a form and auto generate the following HTML:

```html
<form accept-charset="UTF-8" action="/cats" method="POST">
  <label for="cat_name">Name</label>
  <input id="cat_name" name="cat[name]" type="text">
  <label for="cat_color">Color</label>
  <input id="cat_color" name="cat[color]" type="text">
  <input name="commit" type="submit" value="Create Cat">
</form>
```

## `form_for`

Attributes for the `form_for` form helper method:

- More magical form helper in Rails

- `form_for` is a ruby method into which a Ruby object is passed. This means that a form that utilizes `form_for` is directly connected with an Active Record model

- `form_for` yields a `FormBuilder` object that lets you create form elements that correspond to attributes in the model

So, what does this all mean? When you're using the `form_for` method, the object is passed as a `form_for` parameter, and it creates corresponding inputs with each of the attributes. For example, if you have `form_for(@cat)`, the form field params would look like `cat[name]`, `cat[color]`, etc

Let's refactor the cat form that we discussed in the previous section. With `form_for`, it can be simplified to look like this:

```erb
<%= form_for(@cat) do |f| %>
  <%= f.label :name %>
  <%= f.text_field :name %>
  <%= f.label :color %>
  <%= f.text_field :color %>
  <%= f.submit %>
<% end %>
```

The `form_for` above will auto generate the following HTML:

```html
<form accept-charset="UTF-8" action="/cats" method="post">
  <label for="cat_name">Name</label>
  <input id="cat_name" name="cat[name]" type="text" />
  <label for="cat_color">Color</label>
  <input id="cat_color" name="cat[color]" type="text" />
  <input name="commit" type="submit" value="Create" />
</form>
```

## Differences between `form_for` and `form_tag`

Getting back to our roadtrip example, in order to make an informed decision on what route to take we need to know everything possible about both options. In like manner, in order to make a good choice for which form element to use, it's vital to understand the subtle but extremely important differences between the two:

* `form_for` is essentially an advanced form helper that will yield a `FormBuilder` object that you use to generate your form elements (text fields, labels, a submit button, etc.)

* `form_tag` is a lower-level form helper that simply generates a `form` element. To add fields to the `form_tag` block, you add form element tags, such as `text_field_tag`, `number_field_tag`, `submit_tag`, etc.

* `form_tag` makes no assumptions about what you're trying to do, and you're responsible for specifying exactly what the form is supposed to do (send a `POST` request, `PATCH` request, etc.)

* `form_for` handles the retrieval of values from your object model and will also try to route the form to the appropriate action specified in the controller

So, when would you choose one over the other? Below are some real world examples:

* `form_for` - this works well for forms that manage CRUD. Imagine that you have a blog posting application. A great fit for the `form_for` method would be the `Post` model. This is because the `Post` model would have the standard Active Record setup, and therefore it's smart to take advantage of the prepackaged functionality built into `form_for`.

* `form_tag` - this works well for forms that are not directly connected with models. For example, let's say that our blog posting application has a search engine. The search form would be a great fit for using a `form_tag`.

## Does this need an update?

Please open a [GitHub issue](https://github.com/learn-co-curriculum/phrg-rails-forms-readme/issues) or [pull-request](https://github.com/learn-co-curriculum/phrg-rails-forms-readme/pulls). Provide a detailed description that explains the issue you have found or the change you are proposing. Then "@" mention your instructor on the issue or pull-request, and send them a link via Connect.

<p data-visibility='hidden'>PHRG Rails Forms Overview</p>

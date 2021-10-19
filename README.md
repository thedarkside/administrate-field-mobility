# Administrate::Field::Mobility 

[![Gem Version](https://badge.fury.io/rb/administrate-field-mobility.svg)](https://badge.fury.io/rb/administrate-field-mobility)

## Warning
This gem is under early development.

This Administrate plugin adds support for string fields that use the Mobility gem to hold
translated values. 

Requirements in Mobility
----------------------

Set `locale_accessors` to true in `config/initializers/mobility`

```ruby
  config.default_options[:locale_accessors] = true
```

Getting Started
----------------------

Add Administrate::Field::Mobility gems to your Gemfile:

```ruby
gem "administrate-field-mobility", "0.0.1"
```

Add the Administrate Mobility Field to your dashboard.

```ruby
  ATTRIBUTE_TYPES = {
    created_at: Field::DateTime,
    update_at: Field::DateTime,
    name: Field::Mobility::String,
    description: Field::Mobility::Text,
  }.freeze
```

There is an `type` option you can set to change the partial path. 
This is useful when using different mobility-backends which need different renderings 
like `mobility-actiontext`.

```ruby
  ATTRIBUTE_TYPES = {
    description: Field::Mobility::Text.with_options(type: 'actiontext'),
  }.freeze
```

```erb
# file app/views/fields/mobility/actiontext/_form.html.erb
<div class="field-unit__label">
  <%= f.label field.attribute %>
</div>
<div class="field-unit__field">
  <% I18n.available_locales.each do |locale| %>
    <%= f.rich_text_area "#{field.attribute}_#{locale}".downcase.underscore %>
    (<%= locale %>)
  <% end %>
</div>
```

```erb
# file app/views/fields/mobility/actiontext/_show.html.erb
<% I18n.available_locales.each do |locale| %>
  <% I18n.with_locale(locale) do %>
    <%== "#{field.resource.send(field.attribute.to_sym)}" %>
    <%= "(#{locale})" %>
  <% end %>
  <br>
<% end %>
```

Credits
----------------------

This gem is inspired by [administrate-field-globalize](https://github.com/arkirchner/administrate-field-globalize)

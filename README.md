First of all make sure you've created a rails app

```bash
rails new APP_NAME
```

## Setup

Ensure you have bootstrap and it's dependencies

```bash
yarn add bootstrap @popperjs/core
```

Ensure you have the following gems in your Rails `Gemfile`

```ruby
# Gemfile
gem 'autoprefixer-rails'
gem 'font-awesome-sass', '~> 5.6.1'
gem 'simple_form'
```

In your terminal, generate SimpleForm Bootstrap config.

```bash
bundle install
rails generate simple_form:install --bootstrap
```

Replace **all the content** of your `config/initializers/simple_form_bootstrap.rb` file with [this](https://github.com/heartcombo/simple_form-bootstrap/blob/main/config/initializers/simple_form_bootstrap.rb).

Then replace Rails' stylesheets by Le Wagon's stylesheets:

```
rm -rf app/assets/stylesheets
curl -L https://github.com/lewagon/stylesheets/archive/master.zip > stylesheets.zip
unzip stylesheets.zip -d app/assets && rm stylesheets.zip && mv app/assets/rails-stylesheets-master app/assets/stylesheets
```

**On Ubuntu/Windows**: if the `unzip` command returns an error, please install it first by running `sudo apt install unzip`.


## Bootstrap JS

Import bootstrap:

```js
// app/javascript/packs/application.js
import 'bootstrap';
```

## Adding new `.scss` files

Look at your main `application.scss` file to see how SCSS files are imported. There should **not** be a `*= require_tree .` line in the file.

```scss
// app/assets/stylesheets/application.scss

// Graphical variables
@import "config/fonts";
@import "config/colors";
@import "config/bootstrap_variables";

// External libraries
@import "bootstrap/scss/bootstrap"; // from the node_modules
@import "font-awesome-sprockets";
@import "font-awesome";

// Your CSS partials
@import "components/index";
@import "pages/index";
```

Note that the text color of your buttons might change from white to black when you update the colors in `config/colors`. This is done automatically by Bootstrap using the [WCAG 2.0 algorithm](https://getbootstrap.com/docs/5.1/customize/sass/#color-contrast) to make sure the contrast between the text and the background color meets accessibility standards.

For every folder (**`components`**, **`pages`**), there is one `_index.scss` partial which is responsible for importing all the other partials of its folder.

**Example 1**: Let's say you add a new `_contact.scss` file in **`pages`** then modify `pages/_index.scss` as:

```scss
// pages/_index.scss
@import "home";
@import "contact";
```

**Example 2**: Let's say you add a new `_card.scss` file in **`components`** then modify `components/_index.scss` as:

```scss
// components/_index.scss
@import "card";
```

## Navbar template

Our `layouts/_navbar.scss` code works well with our home-made ERB template which you can find here:

- [version without login](https://github.com/lewagon/awesome-navbars/blob/master/templates/_navbar_wagon_without_login.html.erb)
- [version with login](https://github.com/lewagon/awesome-navbars/blob/master/templates/_navbar_wagon.html.erb)

Don't forget that `*.html.erb` files go in the `app/views` folder, and `*.scss` files go in the `app/assets/stylesheets` folder. Also, our navbars have a link to the `root_path`, so make sure that you have a `root to: "controller#action"` route in your `config/routes.rb` file.

# WordPress theme development

- WordPress Codex: [Theme Development](https://codex.wordpress.org/Theme_Development)
- Template Hierarchy: [Template Hierarchy](https://developer.wordpress.org/themes/basics/template-hierarchy/)

---

## Assignment 1 - Setup

- Install WordPress locally or on Metropolia's server if you haven't done it already
- Create a new theme folder: `wp-content/themes/example-theme`
- Create a new file `style.css` in the theme folder
- Add the following code to `style.css`:

```css
/*
Theme Name: Example Theme
Theme URI: https://example.com
Author: Your Name
Author URI: https://example.com
Description: A brief description of your theme
Version: 1.0
License: GNU General Public License v2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html
Tags: some tags separated by commas
Text Domain: example-theme
*/
```

- If you are not plannig to publish your theme, only necessary fields are `Theme Name` and `Template Name`
- `Text Domain` is used for translations, so it's good to have it there, but it's not necessary
- Now you can activate the theme in the WordPress admin panel.

## Template files

- Here are the most common template files used in WordPress themes (\* required):
  - `index.php` - The main template file \*
  - `style.css` - The main stylesheet \*
  - `screenshot.png` - The screenshot for the theme
  - `header.php` - The header template
  - `footer.php` - The footer template
  - `sidebar.php` - The sidebar template
  - `single.php` - The single post template
  - `page.php` - The single page template
  - `search.php` - The search results template
  - `404.php` - The 404 template
  - `functions.php` - The theme functions file

---

## The Loop

- [WordPress Codex](https://codex.wordpress.org/The_Loop)
- The Loop is PHP code used by WordPress to display posts. Using The Loop, WordPress processes each post to be displayed
  on the current page, and formats it according to how it matches specified criteria within The Loop tags. Any HTML or
  PHP code in the Loop will be processed on each post.

```php
<?php
if ( have_posts() ) {
 while ( have_posts() ) {
  the_post();
  //
  // Post Content here
  //
 } // end while
} // end if
?>
```

- Practical example:

```php
<?php
if ( have_posts() ) :
    while ( have_posts() ) :
        the_post();
        the_title();
        the_content();
    endwhile;
else :
    _e( 'Sorry, no posts matched your criteria.', 'textdomain' );
endif;
?>
```

- Note the `_e()` function. It is used instead of `echo` to allow for translation. It is a good practice to use it for
  all strings that are visible to the user.

---

## The Query

[WP_Query](https://developer.wordpress.org/reference/classes/wp_query/) class is used to get posts from the database. It
is used in The Loop to get posts to display.

- If query is not defined in the code, the main query is used. The main query is defined by the URL and the settings in
  the admin panel.
- More in depth explanation in [this video](https://learn.wordpress.org/tutorial/the-wordpress-request-lifecycle/).
- In the following example, we are creating a new query to get 5 posts from the category `news`:

```php
<?php
$args = array(
  'category_name' => 'news',
  'posts_per_page' => 5
);

$query = new WP_Query( $args );

if ( $query->have_posts() ) :
    while ( $query->have_posts() ) :
        $query->the_post();
        the_title(); // Note missing $query-> before the function. That is because the_post()
        the_content(); // sets the global $post variable to the current post in the loop.
    endwhile;
else :
    _e( 'Sorry, no posts matched your criteria.', 'textdomain' );
endif;

wp_reset_postdata();

?>
```

- In most WordPress examples you can see that arrays are created with `array()` function. It is not necessary to use it.
  You can use `[]` instead. It is a newer and more readable way to create arrays.

---

## Assignment 2 - Template files

1. Download 'leiska.zip' from Oma and extract the contents somewhere convenient but
   not in your WordPress installation.
   - The zip file contains `index.html` and `style.css` files and some images.
2. Create `index.php` file in your theme folder and copy the content of `index.html` to `index.php`.
3. Also copy the images to your theme folder and the content of `style.css` to your `style.css` file.
4. Create `header.php`, `footer.php` and `sidebar.php` files in your theme folder and move the elements you think are
   repeating and belong to those files from `index.php` to the new files.
5. Use `get_header()`, `get_footer()` and `get_sidebar()` functions in `index.php` to include the files.
6. Use `language_attributes();` and `bloginfo( 'charset' );` functions in `header.php` to display the language
   attributes and the character set of the blog.
   and the name of the blog.
7. Open the front page of your WordPress site and check that everything looks the same as in the original `index.html`.

---

## Assignment 3 - The Loop

1. Edit `index.php` and replace the content of `<div class="hero-text">` with The Loop.
2. Open the front page of your WordPress site and check the result.
3. [Modify](https://developer.wordpress.org/reference/functions/the_title/#user-contributed-notes) `the_title()` to
   use `<h1>` element in the title.

---

## functions.php

- The `functions.php` file is where you add unique features to your theme. It is a file that is included in the theme
  and
  is loaded on every request. It is a good place to add custom functions and actions.

### Hooks

- [WordPress hooks](https://wpengine.com/resources/wordpress-hooks/) are at the core of how WordPress works. They allow
  you to "hook" a custom function to an existing
  function, which allows you to modify WordPress's functionality without editing core files.
- There are two types of hooks: actions and filters.
- Actions are events that occur when WordPress is running. They let you run a function at a specific time.
  - [Wordpress Page Lifecycle](https://tommcfarlin.com/wordpress-page-lifecycle/).

### Action Hooks

- Modify data in the WordPress database
- Modify what is displayed in the browser
- Do something when an event occurs. Event in this case means something like when
  a post is published, a user logs in, etc.
- Add a menu, a widget, a script, a style or a custom function
- Example, add a custom menu:

  ```php
  function register_my_menu() {
      register_nav_menu( 'main-menu', __( 'Main Menu' ) );
  }

  add_action( 'after_setup_theme', 'register_my_menu' );
  ```

  - This example adds the function `register_my_menu` to the `after_setup_theme` action. This means that the function
    is
    called after the theme is loaded. It is a good place to add custom features to the theme.
  - Note the double underscore `__()` function. It is used for translation. It is a good practice to use it for all
    strings that are visible to the user. The difference between `_e()` and `__()` is that `_e()` prints the string
    and `__()` returns it.

### Filter Hooks

- Filters allow the interception of data at the time it is processed and the ability to modify or replace that data.
- For example, you can modify existing WordPress functions, like `the_title()`, `the_content()`, etc.
- You can also add new functions to existing hooks.
- Example, always add `<h1>` to the title of the post (maybe not the best idea):

  ```php
  function add_h1_to_title( $title ) {
      return '<h1>' . $title . '</h1>';
  }

  add_filter( 'the_title', 'add_h1_to_title' );
  ```

---

## Assignment 4 - theme setup

1. Create a new file `functions.php` in your theme folder.
2. Add the following code to `functions.php`:

   ```php
   <?php
   function theme_setup() {
       add_theme_support( 'title-tag' );
       add_theme_support( 'post-thumbnails' );
       add_theme_support( 'custom-header' );
       add_theme_support( 'custom-logo', array(
           'height'      => 100,
           'width'       => 200,
           'flex-height' => true,
       ) );

       // Set the default Post Thumbnail size
       set_post_thumbnail_size( 200, 200, true ); // 200px wide by 200px high, hard crop mode

       // Add custom image sizes
       add_image_size( 'custom-header', 1200, 400, true ); // Custom header size
   }

   add_action( 'after_setup_theme', 'theme_setup' );

   ?>
   ```

   - This code adds support for title tag, post thumbnails, custom header, custom logo and custom image sizes.

3. To properly add title tag support, you need to remove the `<title>` tag from your `header.php` file and add
   `wp_head()` function before the closing `</head>` tag.
4. Add a custom header to the hero section of your `index.php` file. Use `the_custom_header_markup()` function to get
   the custom header image.
5. Use `the_custom_logo();` function to display the custom logo in the header of your site.
6. To add custom header and custom logo to your site, you need to go to the admin panel / Appearance / Customize. Use
   the images in the `images` folder.
7. Open the front page of your WordPress site and check that everything looks the same as in the original `index.html`.

---

## Assignment 5 - Main Menu

1. Add a new category and sub categories to your WordPress installation:
   - Products
     - Hobbies
     - Cosmetics
     - Household
2. Add new posts to your WordPress installation. Each post should have a title, some text content and an image.

   - Add at least three posts to each sub category.
   - You can get images from the zip in Oma.

3. Set up menu in `theme_setup()` in `functions.php` file by using the `register_nav_menu()` function as earlier in the
   theory part.

4. Add the menu to your `header.php` file by using the `wp_nav_menu()` function.

5. Open admin panel / Appearance / Menus and create a new menu. Add `Home` page, `Products` category and `About Us` page
   to the menu. Set the menu as the main menu.

6. Open the front page of your WordPress site and check that the menu works.

---

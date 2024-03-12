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

- Here are the most common template files used in WordPress themes (* required):
    - `index.php` - The main template file *
    - `style.css` - The main stylesheet *
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

[WP_Query](https://developer.wordpress.org/reference/classes/wp_query/) class is used to get posts from the database. It is used in The Loop to get posts to display.

- If query is not defined in the code, the main query is used. The main query is defined by the URL and the settings in
  the admin panel.
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

1. Download this Zip file: [template-files.zip](template-files.zip) and extract the contents somewhere convenient but not in your WordPress installation.
   - The zip file contains `index.html` and `style.css` files and `images` folder with images.
2. Create `index.php` file in your theme folder and copy the content of `index.html` to `index.php`.
3. Also copy the `images` folder to your theme folder and the content of `style.css` to your `style.css` file.
4. Create `header.php`, `footer.php` and `sidebar.php` files in your theme folder and move the elements you think are repeating and belong to those files from `index.php` to the new files.
5. Use `get_header()`, `get_footer()` and `get_sidebar()` functions in `index.php` to include the files.
6. Open the front page of your WordPress site and check that everything looks the same as in the original `index.html`.

---

## Assignment 3 - The Loop

1. Edit `index.php` and replace the content of `<div class="hero-text">` with The Loop.
2. Open the front page of your WordPress site and check the result.
3. [Modify](https://developer.wordpress.org/reference/functions/the_title/#user-contributed-notes) `the_title()` to use `<h1>` element in the title.


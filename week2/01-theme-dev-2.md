# WordPress theme development 2

- WordPress Codex: [Theme Development](https://codex.wordpress.org/Theme_Development)
- Template Hierarchy: [Template Hierarchy](https://developer.wordpress.org/themes/basics/template-hierarchy/)

---

## Assignment 1 - Featured products

In this assignment, you will create a function that generates and prints articles/products for all the pages where products are displayed. The function will be used to display featured products on the front page and category pages.

1. Add tag `featured` to three products of different categories.
2. Create a new folder `inc` in your theme folder. Add new file `article-function.php` to the `inc` folder. This function will generate and print articles/products for all the pages where products are displayed. (e.g. front page, product category page, etc.)
3. Create a new function `generate_article($products)` in `article-function.php` that generates an article for a product using The Loop.
   - The function takes in an array of products as a parameter. The array is a result of a query that selects three posts with the tag `featured`. Use the `<article>` element from `index.php` as a template for the function.
   - The Loop uses the `$products` array to generate the articles: `$products->have_posts()`
   - Set the post data for the current post in the loop: `$products->the_post()`
     - `the_post_thumbnail()` can be used to get the featured image
     - Use `the_title()` to get the title
     - Use `get_the_excerpt()` to get the excerpt and then use string functions to limit the length of the excerpt to 50 characters. Add three dots at the end of the excerpt.
     - Use `get_permalink()` to get the URL of the post.
     - Use `echo` to output the HTML when needed.
4. Include the `article-function.php` in `functions.php`: `require_once( __DIR__ . '/inc/article-function.php' );`
   - `__DIR__` is a magic constant in WordPress that returns the directory of the file, in this case `functions.php`.
5. Now you can use the `generate_article($products)` function in `index.php` to display the featured products. Make a query that selects three posts with the tag `featured` and pass the result to the function.
6. Open the front page and check that the featured products are displayed.

---

## Assignment 2 - Single product page

In this assignment, you will create a single product page for the products. The single product page will display the product's title, image and description.

1. Create a new file `single.php` in your theme folder. This file will be used to display the single product page.
2. Use `index.php` as a template for `single.php`. Copy the content of `index.php` to `single.php`and remove the hero section.
3. Replace the content of `<section class="products">` with the basic version of The Loop. The Loop will display the product's title and content.
4. Change the class of `<section class="products">` to `<section class="single">`
5. Remove the sidebar from `single.php` and add a new class `full-width` to the `<main>` element.
6. Navigate to the front page and click on a product to check that the single product page is displayed.

---

## Assignment 3 - Category page

In this assignment, you will create a category page for the sub categories of products. The category page will display the title of the category in the hero section and the products in the category. Header image in the hero section will be from a random product in the category.

1. Create a new file `category.php` in your theme folder. This file will be used to display the category page.
2. Use `index.php` as a template for `category.php`.
3. To get the category title and description, use `single_cat_title()` and `category_description()` in the hero section instead of The Loop.
4. To add a random product image to the hero section, use the following steps:

   - Create a new file `random-image.php` in the `inc` folder.
   - Create a new function `get_random_post_image($category_id)` in `random-image.php` that returns the URL of a random product image from the category.
   - The function takes in the category ID as a parameter. Use the `WP_Query` class to get the posts in the category. Use the `rand` parameter to get a random post from the category:

     ```php
     $args = array(
       'post_type'      => 'post',
       'cat'            => $category_id,
       'posts_per_page' => 1,
       'orderby'        => 'rand',
     );
     $random_post = new WP_Query( $args );
     ```

   - Use `$random_post->the_post()` to set the post data for the random post.

5. To get the full size image URL of the random post, also called an _attachment_, do the following steps:

   - Check that $random_post has posts
   - Set the post data for the current post in the loop: `$random_post->the_post()`
   - Use `wp_get_attachment_url( get_post_thumbnail_id())` to get the URL of the featured image.
   - If the featured image URL exists, return it else return an empty string.
   - Reset the post data: `wp_reset_postdata()`

6. Include the `random-image.php` in `functions.php`: `require_once( __DIR__ . '/inc/random-image.php' );`
7. Now you can use the `get_random_post_image($category_id)` function in `category.php` to get the URL of a random product image from the category. You can get the category ID with `get_queried_object_id()`.
8. In the products section use `generate_article()` to display the products in the category. You can use `$wp_query` [global variable](https://codex.wordpress.org/Global_Variables) as the parameter for `generate_article()`.
9. Open a category page and check that the category page is displayed. Currently you'll need to do that manually by typing the URL of the category page to the browser: `http://localhost/your-folder-name/category/products/your-category-name`

---

## Assignment 4 - Custom category page

In this assignment, you will create a custom category page for the products. The custom category page will display the titles of all sub categories and two first products of each sub category and a link to the sub category page to see all products in the sub category.

1. Create a new file `category-products.php` in your theme folder. You have to have a category with a slug `products` to use this file.
2. Use `category.php` as a template for `category-products.php`.
3. `<div class="hero-text">` stays the same as in `category.php`.
4. Add a second header image in the admin panel for the hero image in this page. `<img>` tag in the hero section will be used to display the second header image. Use `get_uploaded_header_images()` and `array_shift()` to get the second header image URL. Use `echo` to output the `<img>` tag with the URL of the second header image.
5. Also add width and height attributes to the `<img>` tag. You can get the size of the header image with `get_custom_header()->width` and `get_custom_header()->height`.
6. To get all sub categories of the category to the `<main>` section, first get the main category ID with `get_queried_object_id()`. Then use `get_categories()` to get all sub categories of the main category.
7. Use `foreach` loop to go through all sub categories and display the title of the sub category. To get two first products of the sub category you need to make a new query for each sub category. Use `WP_Query` class to get the posts in the sub category with these arguments:

   ```php
   $args = array(
     'post_type'      => 'post',
     'cat'            => $sub_category->term_id,
     'posts_per_page' => 2,
   );
   $sub_category_products = new WP_Query( $args );
   ```

8. Use `generate_article()` to display the two first products of the sub category.
9. Add a link to the sub category page to see all products in the sub category:

   ```php
   <article class="product all">
       <a href="<?php echo get_category_link( $subcategory->term_id ); ?>">View All</a>
   </article>
   ```

10. Reset post data at the end of foreach loop: `wp_reset_postdata()`.
11. Open the site and navigate to products page to check that the custom category page is displayed.

---

## Assignment 5 - Search

In this assignment, you will create a search form and results page for the products. The search form will be displayed in the sidebar and the search results will be displayed in the main section.

1. Add search functionality to `functions.php`:
   - Add `add_theme_support( 'html5', array( 'search-form' ) );` to `theme_setup()` function in `functions.php`.
2. Add `get_search_form()` to `sidebar.php` to display the search form in the sidebar.
3. Create a new file `search.php` in your theme folder. This file will be used to display the search results. Use `category.php` as a template for `search.php`. Remove the hero section.
4. To limit the search results only to the products, add a new filter to `functions.php`:

   ```php
   function search_filter($query) {
     if ($query->is_search) {
       $query->set('category_name', 'products');
     }
        return $query;
    }
    add_filter('pre_get_posts','search_filter');
   ```

5. Test the search functionality in your site.

---

## Assignment 6 - Bread crumbs

In this assignment, you will use a plugin to add bread crumbs to your site.

1. Go to plugins in the admin panel and install and activate the plugin Breadcrumb NavXT.
2. Go to settings and select Breadcrumb NavXT.
3. To add the bread crumbs to your site, add the following code to `header.php`:

   ```php
   <?php if ( function_exists('bcn_display') ) {
     bcn_display();
   } ?>
   ```

4. Test the bread crumbs in your site.
5. As home, the plugin displays the name of the site. To display `Home` instead of the site name, add a filter to `functions.php`:

   ```php
   function my_breadcrumb_title_swapper( $title,  $type, $id ) {
      if ( in_array( 'home', $type ) ) {
          $title = __( 'Home' );
      }

      return $title;
   }
   add_filter( 'bcn_breadcrumb_title', 'my_breadcrumb_title_swapper', 3, 10 );

   ```

---

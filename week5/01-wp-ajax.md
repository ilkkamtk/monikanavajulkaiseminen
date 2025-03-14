# JavaScript and AJAX in WordPress

---

## JavaScript in WordPress

To include JavaScript in your WordPress theme, you should use [the `wp_enqueue_script` function](https://developer.wordpress.org/reference/functions/wp_enqueue_script/#parameters). This function allows you to include JavaScript files in your theme and specify when they should be loaded and should they go to the header or footer of the page. Usually, you should include JavaScript files in the footer of the page to improve page load times or use the `defer` attribute in the script tag. You can choose between these two with the `$args` parameter of the `wp_enqueue_script` function.

### wp_enqueue_script

The `wp_enqueue_script` function is used to include JavaScript files in your theme. Here is an example of how to include a JavaScript file in your theme:

```php
function mytheme_enqueue_scripts(): void {
    wp_register_script(
       'my-script',
       get_template_directory_uri() . '/js/myScript.js',
       [],
       '1.0',
       ['strategy' => 'defer'],
     );
    wp_enqueue_script( 'my-script' );
}
add_action( 'wp_enqueue_scripts', 'mytheme_enqueue_scripts' );
```

### Adding styles

Also use the `wp_enqueue_style` function to include CSS files in your theme. Here is an example of how to include a CSS file in your theme:

```php
function mytheme_enqueue_styles(): void {
    wp_register_style(
       'my-style',
       get_template_directory_uri() . '/css/myStyle.css',
       [],
       '1.0',
     );
    wp_enqueue_style( 'my-style' );
}

add_action( 'wp_enqueue_scripts', 'mytheme_enqueue_styles' );
```

---

## AJAX in WordPress

Correct way to use AJAX in WordPress is to use the built-in AJAX functionality in WordPress which is provided by the `wp_ajax_` action hook. This way you can avoid conflicts with other plugins and themes.

## Using the built-in AJAX functionality in WordPress

To use the built-in AJAX functionality in WordPress, you need to use the `wp_ajax_` action hook. Here is an example of how to get single post data using AJAX:

```php
add_action( 'wp_ajax_single_post', 'single_post' );

function single_post(): void {
    header( 'Content-Type: application/json' );
    $post_id = $_POST['post_id'];
    $post    = get_post( $post_id );
    echo json_encode( $post );
    wp_die();
}
```

In WordPress all AJAX requests are sent to the `admin-ajax.php` file:

```javascript
const myAJAXFunction = async () => {
  const url = singlePost.ajax_url;
  const data = new URLSearchParams({
    action: 'single_post',
    post_id: 1,
  });
  const response = await fetch(url, {
    method: 'POST',
    body: data,
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
    },
  });
  const post = await response.json();
  console.log(post);
};

myAJAXFunction();
```

The `singlePost` object is created in the `functions.php` file:

```php
function mytheme_enqueue_scripts(): void {
    wp_register_script( 'single-post', get_template_directory_uri() . '/js/singlePost.js', [], '1.0', true );
    $script_data = array(
        'ajax_url' => admin_url( 'admin-ajax.php' ),
    );
    wp_localize_script( 'single-post', 'singlePost', $script_data );
    wp_enqueue_script( 'single-post' );
}
add_action( 'wp_enqueue_scripts', 'mytheme_enqueue_scripts' );
```

The `wp_localize_script` function is used to pass data from PHP to JavaScript. In this case, we are passing the `ajax_url` to the JavaScript file in the `singlePost` object.

All AJAX requests should be handled by the `admin-ajax.php` file. The reason for this is that the `admin-ajax.php` file is the only file that is guaranteed to be loaded when an AJAX request is made. This is because the `admin-ajax.php` file is loaded by WordPress itself. If you create your own AJAX handler file, there is no guarantee that the file will be loaded when an AJAX request is made.

---

## Assignment

Use the examples above and modify your template so that single posts are loaded using AJAX and displayed in a modal window using `<dialog>` element.

Also convert the like button plugin to update likes using AJAX without reloading the page.

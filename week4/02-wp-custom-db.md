# Wordpress Custom Database Queries

WordPress is made with PHP, so you could use PDO or mysqli to make custom database queries. However, WordPress has its own way of making database queries. This is done with the `wpdb` class.

## global $wpdb

WordPress has a global variable called `$wpdb`. This variable is an instance of the `wpdb` class. You can use this variable to get data from the database. 

---

## Prefix

WordPress uses a prefix for its database tables. The default prefix is `wp_`. You can get the prefix with the following code:

```php
$prefix = $wpdb->prefix;
```

You can also use the prefix in your queries like this:

```php
$results = $wpdb->get_results( "SELECT * FROM {$wpdb->prefix}posts" );
```

When creating your own tables in WordPress, name them with the same prefix as the default WordPress tables. This way you can use the `$wpdb-prefix`, and also you can be sure that your table names are unique.

--- 

## %s and %d

When using the `wpdb` class, you should use `%s` and `%d` for typing your data. `%s` is used for strings and `%d` is used for integers.

---

## get_results

The `wpdb` class has a method called `get_results`. This method is used to get data from the database. Here is an example of how to use this method:

```php

$table = $wpdb->prefix . 'my_table';

$results = $wpdb->get_results( "SELECT * FROM $table" );

foreach ( $results as $result ) {
    echo $result->name;
    echo $result->email;
    echo $result->weight;
}

```

### Prepare 

You should use the `prepare` method when using variables in your queries. This is to prevent SQL injection attacks. Here is an example of how to use the `prepare` method:

```php
$table = $wpdb->prefix . 'my_table';

$id = $_GET['id'];

$preparedQuery = $wpdb->prepare( "SELECT * FROM $table WHERE id = %d", $id )

$results = $wpdb->get_results( $preparedQuery );

```

## Insert

The `wpdb` class has a method called `insert`. This method is used to insert data into the database. Here is an example of how to use this method:

```php
$table = $wpdb->prefix . 'my_table';

$data = array(
    'name' => 'John Doe',
    'email' => 'john@metropolia.fi',
    'weight' => 80
);

$format = array(
    '%s',
    '%s',
    '%d'
);

$success = $wpdb->insert( $table, $data, $format );¨

if ( $success ) {
    echo 'Data inserted';
} else {
    echo 'Error inserting data';
}

```

## Update

`update` method is used to update data in the database:

```php
$table = $wpdb->prefix . 'my_table';

$data = array(
    'name' => 'Jane Doe',
    'email' => 'jane@metropolia.fi',
    'weight' => 70
);

$where = array(
    'id' => 1
);

$format = array(
    '%s',
    '%s',
    '%d'
);

$where_format = array(
    '%d'
);

$success = $wpdb->update( $table, $data, $where, $format, $where_format );

if ( $success ) {
    echo 'Data updated';
} else {
    echo 'Error updating data';
}
```

## Delete

`delete` is used to delete data from the database:

```php
$table = $wpdb->prefix . 'my_table';

$where = array(
    'id' => 1
);

$where_format = array(
    '%d'
);

$success = $wpdb->delete( $table, $where, $where_format );

if ( $success ) {
    echo 'Data deleted';
} else {
    echo 'Error deleting data';
}
```

---

## Custom Tables for a Plugin

When creating custom tables for plugins in WordPress, you should use the `dbDelta` function. This function is used to create tables in the database. Here is an example of how to use this function:

```php

function create_table() {
    global $wpdb;

    $table_name = $wpdb->prefix . 'my_table';

    $charset_collate = $wpdb->get_charset_collate();

    $sql = "CREATE TABLE $table_name (
        id mediumint(9) NOT NULL AUTO_INCREMENT,
        name varchar(100) NOT NULL,
        email varchar(100) NOT NULL,
        weight int(9) NOT NULL,
        PRIMARY KEY  (id)
    ) $charset_collate;";

    require_once( ABSPATH . 'wp-admin/includes/upgrade.php' );
    dbDelta( $sql );
}

register_activation_hook( __FILE__, 'create_table' );

```

To make sure this function is only run once, you should use the `register_activation_hook` function. This function is used to run a function when the plugin is activated.

However, if you are not making a plugin and just want to create a table, you can also make new tables to the database with phpMyAdmin or any other database management tool. Just remember to use the same prefix as the default WordPress tables.

### Simple plugin for liking posts

Here is an example of a simple plugin that adds a like button to posts with shortcode. It sends the data with a form and adds the like to the database without JavaScript.

Save the following to a file called `like-button.php` in the `wp-content/plugins` directory:

```php
<?php

/*
Plugin Name: Like Button
Description: Adds a like button to posts
Version: 1.0
Author: John Doe
*/

// Create table

function create_table() {
    global $wpdb;

    $table_name = $wpdb->prefix . 'likes';

    $charset_collate = $wpdb->get_charset_collate();

    $sql = "CREATE TABLE $table_name (
        id mediumint(9) NOT NULL AUTO_INCREMENT,
        post_id mediumint(9) NOT NULL,
        PRIMARY KEY  (id)
    ) $charset_collate;";

    require_once( ABSPATH . 'wp-admin/includes/upgrade.php' );
    dbDelta( $sql );
}

register_activation_hook( __FILE__, 'create_table' );

// Add like button

function like_button() {
    global $wpdb;

    $table_name = $wpdb->prefix . 'likes';

    $post_id = get_the_ID();

    $results = $wpdb->get_results( "SELECT * FROM $table_name WHERE post_id = $post_id" );

    $likes = count( $results );
    
    
    $output = '<form id="like-form" method="post" action="'. admin_url( 'admin-post.php' ) .'">';
    $output .= '<input type="hidden" name="action" value="add_like">';
    $output .= '<input type="hidden" name="post_id" value="' . $post_id . '">';
    $output .= '<button id="like-button"><ion-icon name="thumbs-up"></ion-icon></button>';
    $output .= '<span id="like-count">' . $likes . '</span>';
    $output .= '</form>';

    return $output;
}

add_shortcode( 'like_button', 'like_button' );

// Add like to database

function add_like() {
    global $wpdb;

    $table_name = $wpdb->prefix . 'likes';

    $post_id = $_POST['post_id'];

    $data = array(
        'post_id' => $post_id
    );

    $format = array(
        '%d'
    );

    $success = $wpdb->insert( $table_name, $data, $format );

    if ( $success ) {
        echo 'Like added';
    } else {
        echo 'Error adding like';
    }

    
	wp_redirect( $_SERVER['HTTP_REFERER'] );
	exit;
}

// add_action( 'wp_ajax_add_like', 'add_like' );

add_action( 'admin_post_add_like', 'add_like' );

// enqueue icons
function my_theme_load_ionicons_font() {
	// Load Ionicons font from CDN
	wp_enqueue_script( 'my-theme-ionicons', 'https://unpkg.com/ionicons@7.1.0/dist/ionicons/ionicons.js', array(), '5.2.3', true );
}

add_action( 'wp_enqueue_scripts', 'my_theme_load_ionicons_font' );

```

Here is how you can use the shortcode in your posts:

```
[like_button]
```

This will add a like button to your posts. When the button is clicked, it will add a like to the database.

If you want to add the like button to your theme, you can use the following code:

```php
<?php echo do_shortcode( '[like_button]' ); ?>
```

---

## Assignment

Use the above example to create a plugin that adds a like button to posts. Add functionalities to remove like and also make sure that a user can only like a post once.

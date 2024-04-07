# WordPress Security

1. Keep WordPress Updated 
   - WordPress is constantly being updated to fix security vulnerabilities. Make sure you are always running the latest
   version of WordPress.
2. Keep Plugins and Themes Updated 
   - Plugins and themes can also have security vulnerabilities. Make sure you are always running the latest versions of your
plugins and themes. 
4. Limit Login Attempts
   - Limit the number of login attempts allowed on your site to prevent brute force attacks. You can use a plugin
like [Limit Login Attempts](https://wordpress.org/plugins/limit-login-attempts/) to do this.

5. Use Two-Factor Authentication 
   - Two-factor authentication adds an extra layer of security to your site by requiring a second form of verification, such
as a code sent to your phone, in addition to your password. You can use a plugin
like [Two Factor Authentication](https://wordpress.org/plugins/two-factor-authentication/) to enable two-factor
authentication on your site.

6. Disable XML-RPC
   - XML-RPC is a feature in WordPress that allows remote publishing and other functions. However, it can also be used by
hackers to launch brute force attacks. You can disable XML-RPC by adding the following code to your site's `.htaccess`
file:

    ```
    # Block WordPress xmlrpc.php requests
    <Files xmlrpc.php>
    order deny,allow
    deny from all
    </Files> 
    ```

7. Use a Security Plugin

   - There are many security plugins available for WordPress that can help you secure your site. Some popular security
plugins
include [Wordfence](https://wordpress.org/plugins/wordfence/), [Sucuri Security](https://wordpress.org/plugins/sucuri-scanner/),
and [iThemes Security](https://wordpress.org/plugins/better-wp-security/).

## [Theme and Plugin Development Security](https://developer.wordpress.org/apis/security/)

1. Sanitize and Validate User Input
   - Always [sanitize](https://developer.wordpress.org/apis/security/sanitizing/) and [validate](https://developer.wordpress.org/apis/security/data-validation/) user input to prevent SQL injection, XSS attacks, and other security vulnerabilities.
   - You can use the `sanitize_text_field`, `sanitize_email`, and `sanitize_url` functions to sanitize user input.
   - Here is an example of how to sanitize user input:

    ```php
    <?php
        $input = '<script>alert("XSS attack")</script>';
        $sanitized_input = sanitize_text_field( $input );
        echo $sanitized_input;
    ?>
    ```

    ```php
    <?php
        $input = '<script>alert("XSS attack")</script>
                  <a href="http://example.com">Example</a>';
        $sanitized_input = wp_kses_post( $input );
        echo $sanitized_input;
    ?>
    ```
   - Validation example:
    
   ```php
    <?php
        $email = 'ilkka@metropolia.fi';
        if ( ! is_email( $email ) ) {
            echo 'Invalid email';
        } else {
            echo 'Valid email';
        }
    ?>
    ```
2. Use Escaping Functions
    - Use [escaping](https://developer.wordpress.org/apis/security/escaping/) functions like `esc_html`, `esc_attr`, `esc_url`, and `esc_js` to prevent XSS attacks.
    - Here is an example of how to use the `esc_html` function:

    ```php
    <?php
        $input = '<script>alert("XSS attack")</script>';
        echo esc_html( $input );
    ?>
    ```
      
3. Use Nonces
   - [Nonces](https://developer.wordpress.org/apis/security/nonces/) are security tokens that help prevent CSRF attacks. Make sure you are using nonces in your forms and AJAX requests.
   - You can generate a nonce using the `wp_create_nonce` function and verify it using the `wp_verify_nonce` function.
   - Here is an example of how to use a nonce in a form:

    ```php
    <?php
        $nonce = wp_create_nonce( 'mytheme_form_nonce' );
        ?>
        <form action="" method="post">
            <input type="hidden" name="mytheme_form_nonce" value="<?php echo $nonce; ?>">
            <input type="text" name="my_input">
            <input type="submit" value="Submit">
        </form>
    ```

    ```php
    <?php
        session_start();
        if ( ! isset( $_POST['mytheme_form_nonce'] ) || ! wp_verify_nonce( $_POST['mytheme_form_nonce'], 'mytheme_form_nonce' ) ) {
            die( 'CSRF token invalid' );
        }
    ?>
    ```

### Complete example from WordPress Codex

```php
/**
 * Generate a Delete link based on the homepage url.
 *
 * @param string $content   Existing content.
 *
 * @return string|null
 */
function wporg_generate_delete_link( $content ) {
	// Run only for single post page.
	if ( is_single() && in_the_loop() && is_main_query() ) {
		// Add query arguments: action, post, nonce
		$url = add_query_arg(
			[
				'action' => 'wporg_frontend_delete',
				'post'   => get_the_ID(),
				'nonce'  => wp_create_nonce( 'wporg_frontend_delete' ),
			], home_url()
		);

		return $content . ' <a href="' . esc_url( $url ) . '">' . esc_html__( 'Delete Post', 'wporg' ) . '</a>';
	}

	return null;
}


/**
 * Request handler
 */
function wporg_delete_post() {
	if ( isset( $_GET['action'] )
         && isset( $_GET['nonce'] )
         && 'wporg_frontend_delete' === $_GET['action']
         && wp_verify_nonce( $_GET['nonce'], 'wporg_frontend_delete' ) ) {

		// Verify we have a post id.
		$post_id = ( isset( $_GET['post'] ) ) ? ( $_GET['post'] ) : ( null );

		// Verify there is a post with such a number.
		$post = get_post( (int) $post_id );
		if ( empty( $post ) ) {
			return;
		}

		// Delete the post.
		wp_trash_post( $post_id );

		// Redirect to admin page.
		$redirect = admin_url( 'edit.php' );
		wp_safe_redirect( $redirect );

		// We are done.
		die;
	}
}


/**
 * Add delete post ability
 */
add_action('plugins_loaded', 'wporg_add_delete_post_ability');
 
function wporg_add_delete_post_ability() {    
    if ( current_user_can( 'edit_others_posts' ) ) {
        /**
         * Add the delete link to the end of the post content.
         */
        add_filter( 'the_content', 'wporg_generate_delete_link' );
      
        /**
         * Register our request handler with the init hook.
         */
        add_action( 'init', 'wporg_delete_post' );
    }
}
```

## Assignment

Secure the theme and plugin you created in the previous assignments. Add nonces, sanitize and validate user input, and use escaping functions to prevent security vulnerabilities. You can also add additional security measures like limiting login attempts and using two-factor authentication by adding plugins to your site.

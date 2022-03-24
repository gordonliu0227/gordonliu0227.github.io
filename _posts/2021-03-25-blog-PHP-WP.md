## php wordpress related notes

This blog is used for record some php used for wordpress related notes.

### php

#### redierct after user login 

Sometimes, Jquery cannot get the value from a form, need do more research about the reason, 
even that input has an id. usually, we can use 
```php
/**
 * Redirect user after successful login.
 *
 * @param string $redirect_to URL to redirect to.
 * @param string $request URL the user is coming from.
 * @param object $user Logged user's data.
 * @return string
 */
function my_login_redirect( $redirect_to, $request, $user ) {
    //is there a user to check?
    if ( isset( $user->roles ) && is_array( $user->roles ) ) {
        //check for admins
        if ( in_array( 'administrator', $user->roles ) ) {
            // redirect them to the default place
            return $redirect_to;
        } else {
            return home_url('/portal-page',$scheme = 'https');
        }
    } else {
        return $redirect_to;
    }
}
 
add_filter( 'login_redirect', 'my_login_redirect', 10, 3 );
```
Inaddition, home_url()
```php

$url = home_url();
echo $url;
Output: http://www.example.com

(Note the lack of a trailing slash)

$url = home_url( '/' );
echo $url;
Output: http://www.example.com/

$url = home_url( $path = '/', $scheme = 'https' );
echo $url;
Output: https://www.example.com/


$url = home_url( $path = 'example', $scheme = 'relative' );
echo $url;
Output: /example
```

### change the button content and link according if user login
Note: add_shortcode() must have a return;
is_user_logged_in return true or false
```php
add_shortcode( 'buttoncp', function () {
	if(is_user_logged_in()){
			$out = '<div class="">
				<a class="" href="">Portal Home</a>
			</div>';

	}else{$out = '<div class="et_pb_button_module_wrapper et_pb_button_0_wrapper  et_pb_module ">
				<a class="" href="">Ready to start? Join now</a>
			</div>';}


	return $out;
} );
```




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

### change button login or logout

```php
add_shortcode( 'login_logout', 'login_logout' );
/**
 * Add a login/logout shortcode button
 * @since 1.0.0
 */
function login_logout() {
ob_start();
    if (is_user_logged_in()) :
    // Set the logout URL - below it is set to the root URL
    ?>
<a href="<?php echo wp_logout_url('/'); ?>"><img src="/">Log Out</a>

<?php
    else :
    // Set the login URL - below it is set to get_permalink() - you can set that to whatever URL eg '/whatever'
?>
    <a href="https://xxx/login/"><img src="/">Log In</a>

<?php
    endif;

return ob_get_clean();
}
```

### get user related info from database

```php
//get user id
$uid = get_current_user_id();
echo $uid;
//get infn from database, it usually will return an object( array)
$results = $wpdb->get_results("SELECT `plan` FROM `database table name` WHERE `wpUserID` = '$uid'");
$plan = $results[0] -> plan;

echo $plan;

```

### get form data from html then transfer to php (function.php)

//list all subuser(students) and reset to another teacher

````php

      <?php $sids = is_school() ? students_of_school() : students_of_teacher(); ?>
      <div class=''>
        <form method="POST" action=''>
          <select name="studentsSelect" multiple="multiple" id="studentsSelect" class="custom-select" size="10">
            <?php foreach ($sids as $sid) { ?>
              <option value="<?php echo $sid; ?>" selected=""><?php echo get_username($sid); ?></option>
            <?php } ?>
          </select>
          <?php
          $ctid = get_current_user_id();
          $oldtids = teachers_of_school($ctid); ?>
          <select name="mySelect1" id="teacherSelect" class="custom-select">
            <?php foreach ($oldtids as $otid) { ?>
              <option value="<?php echo $otid; ?>" selected=""><?php echo get_username($otid); ?></option>
            <?php } ?>
          </select>
        </form>
        <button class="actionButton" id="submitButton" type="submit" value="Click To Submit">Reassign Students</button>
      </div>
      <!-- test for multi select -->
      <script>
        $(function() {
          $('#submitButton').click(function() {
            var studentsSelected = $('#studentsSelect').val();
            var newTeacherSelected = $('#teacherSelect').val();
            // alert(studentsSelected +"teacher selected"+ newTeacherSelected);
            $.ajax({
              type: 'POST',
              url: '<?php echo admin_url('admin-ajax.php'); ?>',
              data: {
                  //transfer data by ajax
                action: 'student_reassign',
                stustudentsSelecteddent_id: studentsSelected,
                newTeacherSelected: newTeacherSelected
              },
              success: function() {
                window.location.reload();
              }
            })
          });
        });
      </script>


      // in function.php
      function student_reassign()
  {

  $stustudentsSelecteddent_id = $_POST['stustudentsSelecteddent_id'];
  $newTeacherSelected = intval(sanitize_text_field($_POST['newTeacherSelected']));
  foreach ($stustudentsSelecteddent_id as $sid){
    $old_tid = teacher_of_student($sid);
    delete_user_meta($old_tid, 'students', $sid);
    delete_user_meta($old_tid, 'student_' . $sid);
    add_user_meta($newTeacherSelected, 'students', $sid);
    update_user_meta($newTeacherSelected, 'student_' . $sid, true);
    update_user_meta($sid, 'teacher', $newTeacherSelected);
  }
  }
  add_action('wp_ajax_student_reassign', 'student_reassign');
```
### Check if URL has certain string with PHP
//Try something like this. The first row builds your URL and the rest check if it contains the word "car".
```php
$url = 'http://' . $_SERVER['SERVER_NAME'] . $_SERVER['REQUEST_URI'];


if (strpos($url,'car') !== false) {
    echo 'Car exists.';
} else {
    echo 'No cars.';
}
```

### use checkbox to change the button clickable or unclickable
```html
  <div class="d-flex justify-content-center subDiv" style=" pointer-events: none;">
    <form method="POST" action=''>
      <input type="hidden" name="action" id="trialuserid" value="<?php echo get_current_user_id(); ?>">
    </form>
    <button type='submit' class='actionButton d-block' id="submitButton1 checkButton" >Subscribe Now!</button>


  </div>
  <div class="d-flex justify-content-center  mt-3 mb-5">
	<input class="align-self-center mx-2" type="checkbox" id="myCheck" onclick="myFunction()">By clicking Subscribe I understand that my trial subscription will be cancelled.</input>

  </div>
  ```
  ```php
  function myFunction() {
  var checkBox = document.getElementById("myCheck");
  var text = document.getElementById("checkButton");
  if (checkBox.checked == true){
    $('.subDiv').css({pointerEvents: ""})
  } else {
    $('.subDiv').css({pointerEvents: "none"})
  }
  }
```

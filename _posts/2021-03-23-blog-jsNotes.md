## Javescript notes

This blog is used for record some js related notes.

### JS

#### when the js not working well in a form

Sometimes, Jquery cannot get the value from a form, need do more research about the reason, 
even that input has an id. usually, we can use 
```js
var str = document.getElementById("test").value;
```
BUT, sometimes it will has an error shows cannot get value from null
use
```js 
$('form').serializeArray() 
```
to check the value from the form, and make sure the name is correct and then use

```js
var inpuValue = document.getElementById('formId').elements['name'].value;

```
to get the value.


### form steps
http://www.jquery-steps.com/Examples


### make the html content cant click
```js
  document.getElementById("trial").style.pointerEvents = "none";

```

### ajax post data
```js
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
```


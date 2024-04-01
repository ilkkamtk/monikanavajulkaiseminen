# Assignment
Use the `MediaItems` table from the previous course and create a PHP application that allows you to insert, update, select, and delete records from the table using PDO and prepared statements.

You need a form for inserting and updating records and a html table for selecting and deleting records.

Start by creating the html table and the form for inserting records. Then create the PHP scripts for inserting and selecting records. After that, create the form for updating records and the PHP script for updating records. Finally, create the PHP script for deleting records.

Use existing user_id and hard code into the form as a hidden field for inserting, updating and deleting records. Also add a couple existing filenames to the form as a dropdown list or similar, so you don't have to manually input the filenames every time.

Add a modal for updating records. When you click an edit button, the modal opens with the existing data in the form fields. When you click the update button, the modal closes and the record is updated.

---

### File uploads in PHP

To upload files in PHP, you need to use the `$_FILES` superglobal. The `$_FILES` superglobal is an associative array that contains information about uploaded files. The array contains the following elements:

- `$_FILES['file']['name']`: The original name of the file on the client machine.
- `$_FILES['file']['type']`: The MIME type of the file, if the browser provided this information.
- `$_FILES['file']['size']`: The size, in bytes, of the uploaded file.
- `$_FILES['file']['tmp_name']`: The temporary filename of the file in which the uploaded file was stored on the server.
- `$_FILES['file']['error']`: The error code associated with this file upload.
- `$_FILES['file']['error']` is an integer that corresponds to one of the predefined error codes.

Here is an example of a simple file upload form:

```html
<form action="upload.php" method="post" enctype="multipart/form-data">
    Select image to upload:
    <input type="file" name="file" id="file">
    <input type="submit" value="Upload Image" name="submit">
</form>
```

And here is an example of the PHP script that handles the file upload:

```php
<?php
// upload.php

if ( $_FILES['file'] == null ) {
    echo "No file uploaded";
    exit;
}

$filename    = $_FILES['file']['name'];
$filesize    = $_FILES['file']['size'];
$filetype    = $_FILES['file']['type'];
$tmp_name    = $_FILES['file']['tmp_name'];
$error       = $_FILES['file']['error'];
$destination = __DIR__ . '/uploads/' . $filename;


if ( $error > 0 ) {
    echo "Error: " . $error;
    exit;
}

if ( move_uploaded_file( $tmp_name, $destination ) ) {
    echo "File uploaded successfully";
} else {
    echo "Error uploading file";
}

?>
```
- The `enctype="multipart/form-data"` attribute is required when you are using forms that have a file upload control.
- The `move_uploaded_file()` function moves an uploaded file to a new location. It returns `true` on success and `false` on failure.
- Note that the example above is quite insecure. You should always validate and sanitize user input before using it in your application. For example, you should check the file type and size before moving the file to the server.

---

### Assignment continued

1. Add file upload functionality to your application.
2. Delete also files when you delete records from the database. That is done with `unlink()` function.

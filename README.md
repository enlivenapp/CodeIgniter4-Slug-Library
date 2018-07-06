# CodeIgniter4 Slug Library

## Notice: NOT Compatible with CodeIgniter < v4

 
 This library is designed to help you generate friendly uri strings for your content stored in the database.
 
 For example if you have a blog post table then you would want uri strings such as: example.com/post/my-post-title
 
 The problem with this is each post needs a unique uri string and this library is designed to handle that for you.
 
 So if you add another with the same uri or title it would convert it to: example.com/post/my-post-title-2
 
 # Requirements
 
 * CodeIgniter4
 * PHP > v7.0
 * Some form of database supported by Query Builder
 
 # Usage
 
 ## Here is an example setup:

 #### Controller  (`application/Controllers/MyController.php`)

```php
<?php namespace App\Controllers;

use CodeIgniter\Controller;
use App\Libraries\Slug;   // use the Slug Library

class MyController extends Controller
{
	public function index()
	{
		// create a new Slug object
		$Slug = new Slug([
				'field' => 'uri',
				'title' => 'title',
				'table' => 'slugs',
				'id' 	=> 'id',
		]);

		// get the new slug
		$newURI = $Slug->create_uri(['title' => 'My Title']);

		//...  the rest of your code
	}
}
 ```
 
 Please note that these fields map to your database table fields.
 
 ```php
 	$config = array(
 		'field' => 'uri',
 		'title' => 'title',
 		'table' => 'mytable',
 		'id' => 'id',
 	);
 	$Slug = new Slug($config);
 ```

 ## Adding and Editing Records: 
 
 When creating a uri for adding to the database you will use something like this:
 
 ```php	
 	$data = array(
        'title' => 'My title',
        'name'  => 'My Name',
        'date'  => 'My date',
        'uri'	=> $Slug->create_uri(['title' => 'My Title'])
    );

    $builder->insert($data);
```

 Then for editing: (Notice the create_uri uses the second param to compare against other fields).
 
 ```php
 	$id = 1;

 	$data = array(
        'title' => 'My title',
        'name'  => 'My Name',
        'date'  => 'My date',
        'uri'	=> $Slug->create_uri(['title' => 'My Title'], $id)
    );

    $builder->where('id', $id);
    $builder->update($data);
```

 
## Public Methods
 
### __construct($config = array())


Setup the library with your config options.
 
 ```php
     $Slug = new Slug([
			'table' => 'mytable',
			'id' => 'id',
			'field' => 'uri',
			'title' => 'title',
			'replacement' => 'dash' // Either dash or underscore
		]);
 ```
 
 ### create_uri($data = '', $id = '')
 
 Creates the actual uri string and in the background validates against the table to ensure it is unique.
  
  **Paramaters**
  
  * $data - (requied) Array or object of data
  * $id - (optional) Id of current record
  
  ```php
 $data = array(
 	'title' => 'My Test',
 );
 $Slug->create_uri($data)
 ```
 
 ```php
 $data = array(
 	'title' => 'My Test',
 );
  $Slug->create_uri($data, 1)
  ```
  
 ```php
 $data = (object)[];
 $data->title = 'My Test',
 
 $Slug->create_uri($data)
 ```
 
This returns a string of the new uri.

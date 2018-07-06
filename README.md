# CodeIgniter4 Slug Library

## Notice: NOT Compatible with CodeIgniter 2 or 3

 
 This library is designed to help you generate friendly uri strings for your content stored in the database.
 
 For example if you have a blog post table then you would want uri strings such as: mysite.com/post/my-post-title
 
 The problem with this is each post needs a unique uri string and this library is designed to handle that for you.
 
 So if you add another with the same uri or title it would convert it to: mysite.com/post/my-post-title-2
 
 # Requirements
 
 * CodeIgniter4
 * PHP >7.0
 * Some form of database supported by Query Builder
 
 # Usage
 
 ## Here is an example setup:

 #### Controller  (`application/Controllers/MyController`)

 ```
 <?php namespace App\Controllers;

use CodeIgniter\Controller;
use App\Libraries\Slug;

class MyController extends Controller
{
	public function index()
	{
		$Slug = new Slug([
				'field' => 'uri',
				'title' => 'title',
				'table' => 'slugs',
				'id' 	=> 'id',
		]);

		$newURI = $Slug->create_uri(['title' => 'My Title']);

		//...  the rest of your code
	}
}
 ```
 
 Please note that these fields map to your database table fields.
 
 	$config = array(
 		'field' => 'uri',
 		'title' => 'title',
 		'table' => 'mytable',
 		'id' => 'id',
 	);
 	$Slug = new Slug($config);
 
 ## Adding and Editing Records: 
 [CI4 Insert Doc](https://bcit-ci.github.io/CodeIgniter4/database/query_builder.html#inserting-data)
 
 When creating a uri for adding to the database you will use something like this:
 	
 	$data = array(
        'title' => 'My title',
        'name'  => 'My Name',
        'date'  => 'My date',
        'uri'	=> $Slug->create_uri(['title' => 'My Title'])
    );

    $builder->insert($data);

 
 Then for editing: (Notice the create_uri uses the second param to compare against other fields).
 
 	$id = 1;

 	$data = array(
        'title' => 'My title',
        'name'  => 'My Name',
        'date'  => 'My date',
        'uri'	=> $Slug->create_uri(['title' => 'My Title'], $id)
    );

    $builder->where('id', $id);
    $builder->update($data);


 
## Methods
 
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
 
 ### set_config($config = array())
 
 Pass an array of config vars that will override setup
 
 **Paramaters**
 
 * $config - (required) - Array of config options
 
 ```php
 $config = array(
 	'table' => 'mytable',
 	'id' => 'id',
 	'field' => 'uri',
 	'title' => 'title',
 	'replacement' => 'dash' // 'dash' or 'underscore'
 );
 $this->slug->set_config($config);
 ```
 ### create_uri($data = '', $id = '')
 
 Creates the actual uri string and in the background validates against the table to ensure it is unique.
  
  **Paramaters**
  
  * $data - (requied) Array of data
  * $data - (requied) Array or object of data
  * $id - (optional) Id of current record
  
  ```php
 $data = array(
 	'title' => 'My Test',
 );
 $this->slug->create_uri($data)
 ```
 
 ```php
 $data = array(
 	'title' => 'My Test',
 );
  $this->slug->create_uri($data, 1)
  ```
  
 ```php
 $data = (object)[];
 $data->title = 'My Test',
 
 $this->slug->create_uri($data)
 ```
 
 ```php
 $data = (object)[];
 $data->title = 'My Test',
 
 $this->slug->create_uri($data, 1)
 ```
 
  This returns a string of the new uri.

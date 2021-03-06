# create-rest-api-in-laravel

## You Tube==>https://youtu.be/bqxC8ap3nDI

## You can get full project using git clone
```
Run command on terminal git clone https://github.com/vermaboys/create-rest-api-in-laravel.git
Run command on terminal composer update
Run command on terminal php artisan migrate
Download Postman software https://www.getpostman.com/apps
```

## You can also write code in your own project using following instructions
```
In routes/api.php
Route::post('posts', 'APIController@getAllPosts');
Route::post('create-post', 'APIController@createPost');
```

```
Run command on terminal php artisan make:migration create_posts_table --create=posts

Write code in create_posts_table migration which is given below

<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreatePostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('description');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('posts');
    }
}
```

```
Run command on terminal php artisan make:controller APIController

Write code in app\Http\Controllers\APIController.php which is given below

<?php

namespace App\Http\Controllers;
use Illuminate\Http\Request;
use App\Post;
use Validator;

class APIController extends Controller
{
    function sendResponse($result,$message){
		$response = [
            'success' => true,
            'data'    => $result,
            'message' => $message,
        ];

		return response()->json($response, 200);
	}
	public function sendError($error, $errorMessages = [], $code = 404)
    {
    	$response = [
            'success' => false,
            'message' => $error,
        ];

        if(!empty($errorMessages)){
            $response['data'] = $errorMessages;
        }

        return response()->json($response, $code);
    }
    public function getAllPosts()
    {
        $posts = Post::all();
        return  $this->sendResponse($posts->toArray(), 'Successfully retrieved .');
    }
    public function createPost(Request $request)
    {
		$validator = Validator::make($request->all(),[
	      'name'=>'required',
	      'description' =>'required',
        ]);

        if($validator->fails()) {
            return $this->sendError(0,$validator->errors()->all());
        }
    		$name=$request->name;
    		$description=$request->description;
        $posts = Post::create(['name'=>$name,'description'=>$description]);
        return  $this->sendResponse(1, 'Successfully created.');
    }
}

```

```
Run command on terminal php artisan make:model Post

Write code in Post model which is given below

<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{

	protected $table='posts';
	protected $fillable = ['name', 'description'];
}
```

```
Run command on terminal php artisan migrate
```


## Create Post

<img src="https://github.com/vermaboys/create-rest-api-in-laravel/blob/master/public/images/create_post.png">


## Get All Posts

<img src="https://github.com/vermaboys/create-rest-api-in-laravel/blob/master/public/images/get_posts.png">
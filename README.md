## Membuat Blog dengan Laravel

Laravel adalah salah satu framework PHP yang populer dan sangat cocok digunakan untuk membuat website blog. Dalam tutorial ini, kita akan belajar bagaimana membuat blog sederhana dengan Laravel.

Langkah-langkah
1. Instal Laravel menggunakan Composer
Pertama-tama, pastikan composer sudah terinstall di komputer anda. Kemudian, jalankan perintah berikut di terminal untuk membuat project baru dengan Laravel:
```
composer create-project --prefer-dist laravel/laravel nama-project
```
2. Buat database untuk blog anda
Selanjutnya, buatlah database dengan nama yang sesuai dengan blog anda di MySQL atau database manajemen sistem lainnya.

3. Buat model, controller, dan view untuk post blog
Buat model, controller, dan view untuk post blog dengan menjalankan perintah berikut di terminal:

```
php artisan make:model Post -mc
```
Perintah tersebut akan membuat model, controller, dan migration untuk post blog. Selanjutnya, buat view untuk menampilkan post blog.

Contoh Koding
Controller:

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Post;

class PostController extends Controller
{
    public function index()
    {
        $posts = Post::all();
        return view('posts.index', compact('posts'));
    }

    public function show($id)
    {
        $post = Post::find($id);
        return view('posts.show', compact('post'));
    }
}
```
View:

```
@extends('layouts.app')

@section('content')
    <h1>Blog Posts</h1>
    @foreach ($posts as $post)
        <div class="post">
            <h2>{{ $post->title }}</h2>
            <p>{{ $post->body }}</p>
            <a href="{{ route('posts.show', $post->id) }}">Read More</a>
        </div>
    @endforeach
@endsection
```
4. Buat model, controller, dan view untuk kategori
Buat model, controller, dan view untuk kategori dengan menjalankan perintah berikut di terminal:
```
php artisan make:model Category -mc
```
Perintah tersebut akan membuat model, controller, dan migration untuk kategori. Selanjutnya, buat view untuk menampilkan kategori.

Contoh Koding
Controller:
```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Category;

class CategoryController extends Controller
{
    public function index()
    {
        $categories = Category::all();
        return view('categories.index', compact('categories'));
    }

    public function show($id)
    {
        $category = Category::find($id);
        return view('categories.show', compact('category'));
    }
}
```
View:
```
@extends('layouts.app')

@section('content')
    <h1>Categories</h1>
    @foreach ($categories as $category)
        <div class="category">
            <h2>{{ $category->name }}</h2>
            <a href="{{ route('categories.show', $category->id) }}">View Posts</a>
        </div>
    @endforeach
@endsection
```
5. Buat migration untuk membuat tabel post dan kategori
Buat migration untuk membuat tabel post dan kategori dengan menjalankan perintah berikut di terminal:
```
php artisan make:migration create_posts_table --create=posts
php artisan make:migration create_categories_table --create=categories
```
Perintah tersebut akan membuat migration untuk membuat tabel post dan kategori di database.

Contoh Koding
Migration untuk post:
```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreatePostsTable extends Migration
{
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('body');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('posts');
    }
}
```
Migration untuk kategori:
```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateCategoriesTable extends Migration
{
    public function up()
    {
        Schema::create('categories', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->timestamps();
        });
    }

    public function down()
    {
        Schema::dropIfExists('categories');
    }
}
```
6. Buat route untuk post dan kategori
Buat route untuk post dan kategori di file web.php dengan menjalankan perintah berikut di terminal:
```
php artisan make:controller PostController
php artisan make:controller CategoryController
```
Perintah tersebut akan membuat controller untuk post dan kategori. Selanjutnya, buat route untuk post dan kategori.

Contoh Koding
Route untuk post:
```
Route::get('/posts', [PostController::class, 'index'])->name('posts.index');
Route::get('/posts/{id}', [PostController::class, 'show'])->name('posts.show');
```
Route untuk kategori:
```
Route::get('/categories', [CategoryController::class, 'index'])->name('categories.index');
Route::get('/categories/{id}', [CategoryController::class, 'show'])->name('categories.show');
```
7. Buat form untuk menambahkan, mengedit, dan menghapus post dan kategori
Buat form untuk menambahkan, mengedit, dan menghapus post dan kategori dengan menggunakan Laravel Collective atau HTML biasa.

Contoh Koding
Form untuk menambah post:
```
<form method="POST" action="{{ route('posts.store') }}">
    @csrf
    <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" name="title" id="title" required>
    </div>
    <div class="form-group">
        <label for="body">Body</label>
        <textarea class="form-control" name="body" id="body" rows="5" required></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```
Form untuk mengedit post:
```
<form method="POST" action="{{ route('posts.update', $post->id) }}">
    @csrf
    @method('PUT')
    <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" name="title" id="title" value="{{ $post->title }}" required>
    </div>
    <div class="form-group">
        <label for="body">Body</label>
        <textarea class="form-control" name="body" id="body" rows="5" required>{{ $post->body }}</textarea>
    </div>
    <button type="submit" class="btn btn-primary">Update</button>
</form>
```
Form untuk menghapus post:
```
<form method="POST" action="{{ route('posts.destroy', $post->id) }}">
    @csrf
    @method('DELETE')
    <button type="submit" class="btn btn-danger">Delete</button>
</form>
```
8. Tambahkan fitur pagination untuk post
Tambahkan fitur pagination untuk post dengan menggunakan paginate() method di controller.

Contoh Koding
Controller:
```
public function index()
{
    $posts = Post::paginate(10);
    return view('posts.index', compact('posts'));
}
```
View:
```
{{ $posts->links() }}
```
9. Tambahkan fitur pencarian untuk post
Tambahkan fitur pencarian untuk post dengan menggunakan where() clause di controller.

Contoh Koding
Controller:
```
public function index(Request $request)
{
    $search = $request->input('search');
    $posts = Post::where('title', 'LIKE', "%$search%")->paginate(10);
    return view('posts.index', compact('posts', 'search'));
}
```
View:
```
<form method="GET" action="{{ route('posts.index') }}">
    <div class="form-group">
        <input type="text" class="form-control" name="search" value="{{ $search }}" placeholder="Search...">
    </div>
    <button type="submit" class="btn btn-primary">Search</button>
</form>
```
10. Tambahkan fitur komentar untuk post
Tambahkan fitur komentar untuk post dengan menggunakan hasMany relationship di model post dan belongsTo relationship di model comment.

Contoh Koding
Model Post:
```
public function comments()
{
    return $this->hasMany(Comment::class);
}
```
Model Comment:
```
public function post()
{
    return $this->belongsTo(Post::class);
}
```
Controller:
```
public function show($id)
{
    $post = Post::find($id);
    $comments = $post->comments;
    return view('posts.show', compact('post', 'comments'));
}

public function storeComment(Request $request, $id)
{
    $post = Post::find($id);
    $comment = new Comment();
    $comment->body = $request->input('body');
    $post->comments()->save($comment);
    return redirect()->route('posts.show', $post->id);
}
```
View:
```
<h2>Comments</h2>
@foreach ($comments as $comment)
    <div class="comment">
        <p>{{ $comment->body }}</p>
    </div>
@endforeach

<form method="POST" action="{{ route('posts.storeComment', $post->id) }}">
    @csrf
    <div class="form-group">
        <label for="body">Comment</label>
        <textarea class="form-control" name="body" id="body" rows="3" required></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
```
Kesimpulan
Dalam tutorial ini, kita telah belajar bagaimana membuat blog sederhana dengan Laravel. Selain itu, kita juga telah belajar beberapa fitur penting seperti pagination, pencarian, dan komentar. Semoga tutorial ini bermanfaat bagi anda yang ingin membuat website blog menggunakan Laravel.

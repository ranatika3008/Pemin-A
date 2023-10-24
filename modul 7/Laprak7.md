# Relasi One-to-Many dan Many-to-Many

## Dasar Teori
<b> 1. Relasi</b> <br>
Hubungan antar tabel yang dilakukan dengan pencocokan primary key dengan foreign key untuk mengombinasikan data dari satu tabel dengan tabel lainnya. <br>
<b> 2. Foreign Key</b>
Properti yang digunakan untuk menandai hubungan dua tabel atau lebih. Foreign key pada tabel anak (child) akan menunjuk tabel induk (parent) yang menjadi referensinya (reference). <br>
<b> 3. One-to-Many</b> <br>
Relasi yang menunjukkan hubungan antar tabel dimana baris pada tabel induk dapat terhubung dengan satu atau lebih baris di tabel anak. Sementara baris pada tabel anak hanya dapat terhubung dengan satu baris di tabel induk. <br>
Contoh penerapan one-to-many
- Satu tutorial dapat memiliki banyak komentar, namun satu komentar hanya dapat berada di satu tutorial
- Satu dosbing dapat memiliki banyak mahasiswa, namun mahasiswa hanya dapat dibimbing satu dosen 
![ss a](..//modul%207/ss%207/a.png)

<b> 4. Many-to-Many</b> <br>
Relasi yang menunjukkan hubungan antar tabel dimana baris pada tabel induk dapat terhubung dengan satu atau lebih baris di tabel anak. Berlaku sebaliknya pada tabel anak yang dapat terhubung dengan satu atau lebih baris di tabel induk. <br>
Contoh penerapan many-to-many
- Satu mahasiswa dapat mengambil banyak mata kuliah, namun satu mata kuliah dapat diambil banyak mahasiswa
- Postingan dapat memiliki banyak tag, namun satu tag dapat dimiliki banyak postingan.
![ss b](..//modul%207/ss%207/b.png)

<b> 5. Junction Table</b> <br>
Tabel yang digunakan untuk mengatur kombinasi baris pada relasi many-to-many. Junction table berisi foreign key dari kedua tabel yang memiliki relasi many-to-many. <br>
Contoh penerapan junction table adalah tabel Enrollment pada relasi many-to-many di atas

## Langkah Percobaan
### Pembuatan Tabel

Berikut adalah tabel yang akan digunakan pada percobaan ini
| posts | comments |tags | post_tag |
| :----:| :---: |:----:| :---: |
| id| id | id| postId |
| content (STRING) | review (STRING) | name| tagId |

1. Sebelum membuat migrasi database atau membuat tabel pastikan server database aktif kemudian pastikan sudah membuat database dengan nama `lumenpost` <br>
![ss 1.1](..//modul%207/ss%207/1.1.png)

2. Kemudian ubah konfigurasi database pada file .env menjadi seperti berikut
    ```
    DB_CONNECTION=mysql
    DB_HOST=127.0.0.1
    DB_PORT=3306
    DB_DATABASE=lumenpost
    DB_USERNAME=root
    DB_PASSWORD=
    ```
    ![ss 1.2](..//modul%207/ss%207/1.2.png)

3. Setelah mengubah konfigurasi pada file .env, kita juga perlu menghidupkan beberapa library bawaan dari lumen dengan membuka file app.php pada folder
bootstrap dan mengubah baris ini
    ```
    // $app->withFacades();
    // $app->withEloquent();
    ```
    menjadi
    ```
    $app->withFacades();
    $app->withEloquent();
    ```
    ![ss 1.3](..//modul%207/ss%207/1.3.png)

4. Setelah itu jalankan command berikut untuk membuat file migration
    ```
    php artisan make:migration create_posts_table
    php artisan make:migration create_comments_table
    php artisan make:migration create_tags_table
    php artisan make:migration create_post_tag_table
    ```
    ![ss 1.4](..//modul%207/ss%207/1.4.png)

5. Ubah fungsi up() pada file migrasi create_posts_table
    ```
    #sebelumnya
    ...
    public function up()
        {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
        });
        }
    ...
    #diubah menjadi
    ...
    public function up()
        {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->timestamps();
            $table->string('content');
            });
        }
    ...
    ```
    ![ss 1.5](..//modul%207/ss%207/1.5.png)

6. Ubah fungsi `up()` pada file `create_comments_table`
    ```
    #sebelumnya
    ...
    public function up()
        {
            Schema::create('comments', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });
        }
    ...
    #diubah menjadi
    ...
    public function up()
        {
            Schema::create('comments', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
                $table->string('review');
                $table->foreignId('postId')->unsigned();
            });
        }
    ...
    ```
    ![ss 1.6](..//modul%207/ss%207/1.6.png)

7. Ubah fungsi `up()` pada file `create_tags_table`
    ```
    #sebelumnya
    ...
    public function up()
        {
            Schema::create('tags', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
            });
        }
    ...
    #diubah menjadi
    ...
    public function up()
        {
            Schema::create('tags', function (Blueprint $table) {
                $table->id();
                $table->timestamps();
                $table->string('name');
            });
        }
    ...
    ```
    ![ss 1.7](..//modul%207/ss%207/1.7.png)

8. Ubah fungsi `up()` pada file `create_post_tag_table`
    ```
    #sebelumnya
    ...
    public function up()
    {
    Schema::create('post_tag', function (Blueprint $table) {
    $table->id();
    $table->timestamps();
    });
    }
    ...
    #diubah menjadi
    ...
    public function up()
    {
    Schema::create('post_tag', function (Blueprint $table) {
    $table->id();
    $table->timestamps();
    $table->foreignId('postId')->unsigned();
    $table->foreignId('tagId')->unsigned();
    });
    }
    ...
    ```
    ![ss 1.8](..//modul%207/ss%207/1.8.png)

9. Kemudian jalankan command
    ```
    php artisan migrate
    ```
    ![ss 1.9](..//modul%207/ss%207/1.9.png)

### Pembuatan Model

1. Buatlah file dengan nama `Post.php` dan isi dengan baris kode berikut <br>
    ![ss 2.1](..//modul%207/ss%207/2.1.png)
    ```
    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class Post extends Model
    {
        /**
        * The attributes that are mass assignable.
        *
        * @var string[]
        */
        protected $fillable = [
        'content'
        ];
        /**
        * The attributes excluded from the model's JSON form.
        *
        * @var string[]
        */
        protected $hidden = [];
    }
    ``` 
    ![ss 2.2](..//modul%207/ss%207/2.2.png)

2. Buatlah file dengan nama `Comment.php` dan isi dengan baris kode berikut <br>
    ![ss 2.3](..//modul%207/ss%207/2.3.png)
    ```
    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class Comment extends Model
    {
    /**
    * The attributes that are mass assignable.
    *
    * @var string[]
    */
    protected $fillable = [
    'review'
    ];
    /**
    * The attributes excluded from the model's JSON form.
    *
    * @var string[]
    */
    protected $hidden = [];
    }
    ```
    ![ss 2.4](..//modul%207/ss%207/2.4.png)

3. Buatlah file dengan nama `Tag.php` dan isi dengan baris kode berikut <br>
    ![ss 2.5](..//modul%207/ss%207/2.5.png)
    ```
    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class Tag extends Model
    {
        /**
        * The attributes that are mass assignable.
        *
        * @var string[]
        */
        protected $fillable = [
        'name'
        ];
        /**
        * The attributes excluded from the model's JSON form.
        *
        * @var string[]
        */
        protected $hidden = [];
    }
    ```
    ![ss 2.6](..//modul%207/ss%207/2.6.png)

### Relasi One-to-Many

1. Tambahkan fungsi `comments()` pada file `Post.php`
    ```
    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class Post extends Model
    {
        ...
        // fungsi comments
        public function comments()
        {
            return $this->hasMany(Comment::class, 'postId');
        }
    }
    ```
    ![ss 3.1](..//modul%207/ss%207/3.1.png)

2. Tambahkan fungsi `post()` dan atribut `postId` pada `$fillable` pada file `Comment.php`
    ```
    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class Comment extends Model
    {
        ...
        protected $fillable = [
            'review',
            'postId' // atribut postId
        ];
        /**
        * The attributes excluded from the model's JSON form.
        *
        * @var string[]
        */
        protected $hidden = [];
        public function post()
        {
            return $this->belongsTo(Post::class, 'postId');
        }
    }
    ```
    ![ss 3.2](..//modul%207/ss%207/3.2.png)

3. Buatlah file `PostController.php` dan isilah dengan baris kode berikut <br>
    ![ss 3.3](..//modul%207/ss%207/3.3.png)
    ```
    <?php
    namespace App\Http\Controllers;
    use App\Models\Post;
    use Illuminate\Http\Request;
    class PostController extends Controller
    {
        /**
        * Create a new controller instance.
        *
        * @return void
        */
        public function __construct()
        {
            //
        }
        //
        public function createPost(Request $request)
        {
            $post = Post::create([
                'content' => $request->content,
            ]);
            return response()->json([
                'success' => true,
                'message' => 'New post created',
                'data' => [
                    'post' => $post
                ]
            ]);
        }
        public function getPostById(Request $request)
        {
            $post = Post::find($request->id);
                return response()->json([
                'success' => true,
                'message' => 'All post grabbed',
                'data' => [
                    'post' => [
                        'id' => $post->id,
                        'content' => $post->content,
                        'comments' => $post->comments,
                    ]
                ]
            ]);
        }
    }
    ```
    ![ss 34](..//modul%207/ss%207/3.4.png)

4. Buatlah file `CommentController.php` dan isilah dengan baris kode berikut <br>
    ![ss 3.5](..//modul%207/ss%207/3.5.png)
    ```
    <?php
    namespace App\Http\Controllers;
    use App\Models\Comment;
    use Illuminate\Http\Request;
    class CommentController extends Controller
    {
        /**
        * Create a new controller instance.
        *
        * @return void
        */
        public function __construct()
        {
            //
        }
        //
        public function createComment(Request $request)
        {
            $comment = Comment::create([
                'review' => $request->review,
                'postId' => $request->postId,
            ]);
            return response()->json([
                'success' => true,
                'message' => 'New comment created',
                'data' => [
                    'comment' => $comment
                ]
            ]);
        }
    }
    ```
    ![ss 3.6](..//modul%207/ss%207/3.6.png)

5. Tambahkan baris berikut pada `routes/web.php`
    ```
    <?php
    ...
    $router->group(['prefix' => 'posts'], function () use ($router) {
        $router->post('/', ['uses' => 'PostController@createPost']);
        $router->get('/{id}', ['uses' => 'PostController@getPostById']);
    });
    $router->group(['prefix' => 'comments'], function () use ($router) {
        $router->post('/', ['uses' => 'CommentController@createComment']);
    });
    ```
    ![ss 3.7](..//modul%207/ss%207/3.7.png)

6. Buatlah satu post menggunakan Postman
![ss 3.8](..//modul%207/ss%207/3.8.png)

7. Buatlah satu comment menggunakan Postman
![ss 3.9](..//modul%207/ss%207/3.9.png)

8. Tampilkan post menggunakan Postman
![ss 3.10](..//modul%207/ss%207/3.10.png)

### Relasi Many-to-Many
1. Tambahkan fungsi `tags()` pada file `Post.php`
    ```
    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class Post extends Model
    {
        ...
        public function tags()
        {
            return $this->belongsToMany(Tag::class, 'post_tag', 'postId', 'tagId');
        }
    }
    ```
    ![ss 4.1](..//modul%207/ss%207/4.1.png)

2. Tambahkan fungsi posts() pada file `Tag.php`
    ```
    <?php
    namespace App\Models;
    use Illuminate\Database\Eloquent\Model;
    class Tag extends Model
    {
    ...
        public function posts()
        {
            return $this->belongsToMany(Post::class, 'post_tag', 'tagId', 'postId');
        }
    }
    ```
    ![ss 4.2](..//modul%207/ss%207/4.2.png)

3. Buatlah file `TagController.php` dan isilah dengan baris kode berikut <br>
    ![ss 4.3](..//modul%207/ss%207/4.3.png)
    ```
    <?php
    namespace App\Http\Controllers;
    use App\Models\Tag;
    use Illuminate\Http\Request;
    class TagController extends Controller
    {
        /**
        * Create a new controller instance.
        *
        * @return void
        */
        public function __construct()
        {
            //
        }
        //
        public function createTag(Request $request)
        {
            $tag = Tag::create([
                'name' => $request->name
            ]);
            return response()->json([
                'success' => true,
                'message' => 'New tag created',
                'data' => [
                    'tag' => $tag
                ]
            ]);
        }
    }
    ```
    ![ss 4.4](..//modul%207/ss%207/4.4.png)

4. Tambahkan fungsi addTag dan response tags pada `PostController.php`
    ```
    <?php
    namespace App\Http\Controllers;
    use App\Models\Post;
    use Illuminate\Http\Request;
    class PostController extends Controller
    {
        ...
        public function getPostById(Request $request)
        {
            $post = Post::find($request->id);
            return response()->json([
                'success' => true,
                'message' => 'All post grabbed',
                'data' => [
                    'post' => [
                        'id' => $post->id,
                        'content' => $post->content,
                        'comments' => $post->comments,
                        'tags' => $post->tags, //response tags
                    ]
                ]
            ]);
        }
        public function addTag(Request $request)
        {
            $post = Post::find($request->id);
        $post->tags()->attach($request->tagId);
        return response()->json([
        'success' => true,
        'message' => 'Tag added to post',
        ]);
        }
    }
    ```
    ![ss 4.5](..//modul%207/ss%207/4.5.png)

5. Tambahkan baris berikut pada routes/web.php
    ```
    $router->group(['prefix' => 'posts'], function () use ($router) {
        $router->post('/', ['uses' => 'PostController@createPost']);
        $router->get('/{id}', ['uses' => 'PostController@getPostById']);
        $router->put('/{id}/tag/{tagId}', ['uses' => 'PostController@getPostById']); //
    });
    ...
    $router->group(['prefix' => 'tags'], function () use ($router) {
        $router->post('/', ['uses' => 'TagController@createTag']);
    });
    ```
    ![ss 4.6](..//modul%207/ss%207/4.6.png)

6. Buatlah satu tag menggunakan Postman
![ss 4.7](..//modul%207/ss%207/4.7.png)

7. Tambahkan tag “jadul” pada post “disana engkau berdua”
![ss 4.8](..//modul%207/ss%207/4.8.png)

8. Tampilkan post “disana engkau berdua” menggunakan Postman
![ss 4.9](..//modul%207/ss%207/4.9.png)

9. Buatlah postingan “tanpamu apa artinya” menggunakan Postman
![ss 4.10](..//modul%207/ss%207/4.10.png)

10. Tambahkan tag “jadul” pada postingan “tanpamu apa artinya”
![ss 4.11](..//modul%207/ss%207/4.11.png)

11. Buatlah tag “lagu” menggunakan Postman
![ss 4.12](..//modul%207/ss%207/4.12.png)

12. Tambahkan tag “lagu” pada postingan “tanpamu apa artinya”
![ss 4.13](..//modul%207/ss%207/4.13.png)

13. Tampilkan post pertama
![ss 4.14](..//modul%207/ss%207/4.14.png)

14. Tampilkan post kedua
![ss 4.15](..//modul%207/ss%207/4.15.png)
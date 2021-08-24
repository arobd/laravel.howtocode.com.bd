# ভিউ

ভিউ পার্টের মধ্যে আপনার অ্যাপ্লিকেশনের সাধারনত HTML কন্টেন্ড গুলো থাকে, এবং কন্ট্রোলার ও মডেল\(বিজনেস লজিক\) সেপারেট করার এটি একটি ভাল পদ্ধ্যতি। ভিউস ফাইল গুলো **resources/views** ডিরেক্টরিতে থাকে। নিচে একটা খুব সাধারণ উধাহরন দেয়া হলঃ

```php
<!-- View stored in resources/views/welcome.php -->
<html>
    <body>
        <h1>Hello, <?php echo $name; ?> Welcome to the laravel world!</h1>
    </body>
</html>
```

ভিউ ফাইলটি ব্রাউজারে দেখতে চাইলে নিচের ন্যায় কোডটি route এ লিখতে হবে।

```php
Route::get('/', function()
{
    return view('welcome', ['name' => 'Sohel Amin']);
});
```

ভিউস ডিরেক্টরির মধ্যে নেস্টেট সাব-ডিরেক্টরি থাকতে পারে। উধাহরন হিসেবে বলা যায় আপনি যদি ভিউ ফাইলটিকে এইভাবে রাখেনঃ **resources/views/admin/profile.php** তাহলে আপনাকে নিম্নের ন্যায় ভিউ হেল্পার মেথডটিকে কল করতে হবেঃ

```php
return view('admin.profile', $data);
```

এখন আমি আপনাদেরকে কিভাবে ভিউ ফাইলের মধ্যে ডাটা পাস করতে হয় সেটা সম্পর্কে ধারণা দিব। আপনারা হয়ত খেয়াল করেছেন ভিউ মেথডটি কল করার সময় আমি দ্বিতীয় প্যারামিটার সহ দেখিয়েছি।

```php
return view('admin.profile', $data);
```

এখানে দ্বিতীয় প্যারামিটার **$data** দিয়ে অ্যারে পাস করা হয়েছে। এইক্ষেত্রে ভিউ ফাইলের মধ্যে আপনি ডাটা অ্যাকসেস করতে চাইলে **$data** এর পরিবর্তে আপনাকে **$data** অ্যারে এর ইনডেক্স কে কল করতে হবে। কারন কন্ট্রোলার অথবা রাউট থেকে ডাটা পাস করার সময় অ্যারে সয়ংক্রিয় ভাবে ভিউতে গিয়ে ভেরিয়েবল এ রুপান্তরিত হয়। আপনি যেকোনো ভিউ ফাইলকে চাইলে ভেরিয়েবেলের মধ্যে রাখতে পারেন এবং সেটি অন্য কোন ভিউ ফাইলে ইকো \(echo\) করতে পারেন। যেমন ধরুন আপনি কন্টাক্ট ফর্ম অথবা অন্য কিছু ছোট আকারের কোড একটা ভিউ ফাইলের মধ্যে রেখে দিয়েছেন আর অন্য ভিউ ফাইলে এইটা দেখাইতে চাচ্ছেন তাহলে আপনাকে নিচের মত করে লিখতে হবে।

```php
$view = view('welcome', $data);
```

ভিউ ফাইলে অ্যারে পাস করা ছাড়াও আমরা লারাভেলের প্রচলিত নিয়মে ডাটা পাস করতে পারি আর এইজন্য নিচের ন্যায় লিখতে হবে।

```php
// Using conventional approach
$view = view('welcome')->with('name', 'Sohel Amin');
```

এটা ছাড়াও আমরা ম্যাজিক মেথড ব্যাবহার করতে পারি যেটা আমাদেরকে আরেক রকম ভিন্ন স্বাদ দিবে।

```php
// Using Magic Methods
$view = view('welcome')->withName('Sohel Amin');
```

ধরুন এখন আপনি নাম এর পরিবর্তে ইমেইল পাস করবেন তাহলে আপনাকে নিচের মত করে লিখতে হবে।

```php
$view = view('welcome')->withEmail('sohelamin@example.com');
```

আপনি চাইলে নাম আর ইমেইল একসাথে লিখতে পারেন।

```php
return view('welcome')->withName('Sohel Amin')->withEmail('sohelamin@example.com');
```

এখন আপনি ভিউ ফাইলে যেভাবে ডাটা অ্যাকসেস করবেন সেটা নিচে দেখান হল।

```php
<!-- View stored in resources/views/welcome.php -->
<?php
    var_dump($name);
    var_dump($email);
?>
```

এছাড়াও আপনি আপনার ডাটা কিংবা অ্যারে সব ভিউ ফাইলের সাথে শেয়ার করতে পারবেন এইভাবে।

```php
view()->share('data', [1, 2, 3]);
```

## অনুশীলনঃ

ভিউ সম্পর্কে তো কিছু ধারনা হলো, আসুন [পূর্ববর্তী অনুশীলন](http://laravel.howtocode.com.bd/basic-routing.html) এর সাথে আজ আমরা ভিউ যোগ করি। আমরা **resources/views** ফোল্ডারটি খুললে `welcome.blade.php` নামে একটি ভিউ দেখতে পাবো। মজার এই নামটির ৩টি অংশঃ প্রথমটি ভিউ এর নাম, দ্বিতীয় অংশঃ .blade যার মানে এই ফাইলে ব্লেড টেমপ্লেট থাকলে সেটা PHP এবং HTML এ রেন্ডার হবে\(**ব্লেড টেমপ্লেটিং** সম্পর্কে আমরা পরবর্তী চ্যাপ্টারএ জানবো\), তৃতীয় অংশঃ .php, হা হা আপনি এটার মানে জানেন। এই আনুশীলনে আমরা HTML, CSS ও Twitter Bootstrap ব্যবহার করবো, যেহেতু আপনি লারাভেল শিখছেন সেহেতু আমি ধরে নিচ্ছি আপনি এগুলা জানেন তাই এই অংশ গুলর কোনও ব্যাখ্যা আমি দিয়ে আপনার সময় নষ্ট করব না।

আমাদের এপটির নাম ছিল blog.app, এবার blog.app/resources/views এর ভিতর `home.blade.php` নামে একটি ফাইল তৈরি করি। যার কন্টেন্ট নিম্নরূপ

```markup
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HowtoCode By Laravel</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
</head>
<body>
<div class="container">
    <div class="row">
        <div class="col-md-12 text-center">
            <h1>HowToCodeBD: Laravel 5.2.24</h1>
        </div>
    </div>
</div>

<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
</body>
</html>
```

এবার `app/Http/routes.php` ফাইলটি খুলি ও নিম্নরূপ পরিবর্তন করি

```php
Route::group(['middleware' => ['web']], function () {

    Route::get('/', function () {
        return view('home');
    });

});
```

লারাভেল জানে কোথায় ভিউ ফাইল থাকে আর কি ফাইল এক্সটেনশন, তাই শুধু ভিউ এর নামটা দিলেই কাজ হয়।

এবার আপনি `home.blade.php` এর মত করে `about.blade.php` এবং `contact.blade.php` দুইটা ফাইল তৈরি করেন একই পাথ এ। নতুন ভিউ দুটার জন্য `routes.php` তে কিছু কোড যোগ করিঃ

```php
Route::get('/about', function () {
        return view('about');
    });

    Route::get('/contact', function () {
        return view('contact');;
    });
```

এভাবে যদি সব ভিউ একই জায়গায় থাকে তাহলে বেশ সমস্যা হয়ে যায়, আসুন না আমরা একই টাইপের ভিউ গুলোকে এক একটি ফোল্ডার এর ভিতর রাখি। ধরি আমাদের এডমিন এরিয়ার জন্য কিছু ভিউ আছে, তাই `resources/views` এর মধ্যে `admin` নামে ফোল্ডার বানাই। এবং এই ফোল্ডার এর মধ্যে `dashboard.blade.php` এবং `users.blade.php` নামে ভিউ বানাই। হয়ে গেল তো? এবার এগুলোর জন্য `Routes.php` তে রাউট বানাই

```php
    Route::get('/admin', function () {
        return view('admin.dashboard');
    });

    Route::get('/admin/users', function () {
        return view('admin.users');
    });
```

admin ফোল্ডার এর জন্য admin.{view-name}, কিন্তু যদি admin ফোল্ডার এর ভিতর আরও একটি ফোল্ডার থাকত এবং তার ভিতর কিছু ভিউ থাকতো? ঠিক ধরেছন! এমন হতো - admin.another-folder.{view-name}

মনে আছে গত অনুশীলন এ Route Group বানিয়েছিলাম? দেখি অ্যাডমিন ভিউ এর রুট গুলোকে সেটার মধ্যে নিলে কেমন হয়।

```php
//Lets make some group route
    Route::group(['prefix' => 'admin'], function () {
        Route::get('/', function () {
            return view('admin.dashboard');
        });
        // this link: blog.app/admin

        Route::get('/users', function () {
            return view('admin.users');
        });
        // this link: blog.app/admin/users
    });
```

আমরা কিন্তু এই দুটা অনুশীলনএ বেসিক রাউট ও বেসিক ভিউ সম্পর্কে জানলাম।

পরবর্তী চ্যাপ্টারে **ব্লেড টেমপ্লেটিং** নিয়ে আলোচনা করা হবে যেটা আপনার ভিউ ফাইলকে আরও সহজভাবে উপস্থাপনে সহযোগিতা করবে।


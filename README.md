
# How to Send Mail using Queue in Laravel 9
Laravel 9 send mail using queue example; Through this tutorial, i am going to show you how to send mail using queue job in laravel 9 apps with smtp drivers like Mailgun, Postmark, Amazon SES, office365, gmail and sendmail.
<p align="center"><a href="https://wesley.io.ke" target="_blank"><img src="https://laratutorials.com/wp-content/uploads/2022/02/How-to-Send-Mail-using-Queue-in-Laravel-9.jpg" width="400"></a></p>

# How to Send Mail using Queue in Laravel 9
Use the below given steps to send mail using queue in laravel 9 apps:
|No|Title|
|---|---|
|Step 1 – |Install Laravel 9 App|
|Step 2 – |Configure SMTP & Database|
|Step 3 – |Create Mailable Class|
|Step 4 – |Add Email Send Route|
|Step 5 – |Create Directory And Mail Blade View|
|Step 6 – |Configuration Mail Queue|
|Step 7 – |Build Queue Job For Sending Mail|
|Step 8 – |Run Development Server|

# Step 1 – Install Laravel 9 App

Run the following command on command prompt to install or download laravel application:
```php
composer create-project --prefer-dist laravel/laravel blog
```
# Step 2 – Configure SMTP & Database
To configure smtp details in .env file like following:
```php

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=db name
DB_USERNAME=db user name
DB_PASSWORD=db password

MAIL_DRIVER=smtp
MAIL_HOST=smtp.gmail.com 
MAIL_PORT=587 
MAIL_USERNAME=Add  user name here
MAIL_PASSWORD=Add  password here
MAIL_ENCRYPTION=tls 
```
Note that:- If we are sending a mail using Gmail we have to allow non-secure apps to access Gmail we can do this by going to Gmail settings here.

Once less secure apps are enabled; now we can use Gmail for sending the emails.

# Step 3 – Create Mailable Class
Run the following command on command prompt to create NotifyMail mailable class:

```php
php artisan make:mail NotifyMail
<?php
namespace App\Mail;
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Mail\Mailable;
use Illuminate\Queue\SerializesModels;
class NotifyMail extends Mailable
{
    use Queueable, SerializesModels;
    /**
     * Create a new message instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }
    /**
     * Build the message.
     *
     * @return $this
     */
    public function build()
    {
        return $this->view('view.name');
    }
}
```
By whatever name we will create an email template. That want to send. Do not forget to add an email template name in build class of the above created notifymail class.
```php
    return $this->view('view.name');
    to
    return $this->view('emails.demoMail');
    ```
In next step, Create email template named demoMail.blade.php inside resources/views/emails directory. That’s why have added view name email.

# Step 4 – Add Send Email Route
In this step, open /web.php, so navigate to routes directory. And then add the following routes for send email:

```php
Route::get('email-test', function(){

$details['email'] = 'your_email@gmail.com';

dispatch(new App\Jobs\SendEmailJob($details));

dd('done');
});
```
# Step 5 – Create Directory And Mail Blade View
To create directory name emails inside resources/views directory. Then create an demoMail.blade.php blade view file inside resources/views/emails directory. And update the following code into it:
```php
<!DOCTYPE html>
<html>
<head>
 <title>Laravel 9 Send Email Example</title>
</head>
<body>
 <h1>This is test mail from wesley</h1>
 <p>Laravel 9 send email example</p>
</body>
</html> 
```
# Step 6 – Configuration Mail Queue
To configuration on queue driver. So open .env file and define database queue driver on “.env” file like following:
``php
QUEUE_CONNECTION=database
```
Then open the command prompt and run following command for queue database tables:
```php
php artisan queue:table
```
Next, migrate tables into database:

```php
php artisan migrate
```
# Step 7 – Build Queue Job For Sending Mail
Run the following command on command prompt to create queue job:
```php
php artisan make:job SendEmailJob
```
Then open SendEmailJob.php file which is placed on “app/Jobs” directory. And update the following mail queue code into it:
```php
<?php
  
namespace App\Jobs;
  
use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;
use App\Mail\SendEmailTest;
use Mail;
  
class SendEmailJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;
  
    protected $details;
  
    /**
     * Create a new job instance.
     *
     * @return void
     */
    public function __construct($details)
    {
        $this->details = $details;
    }
  
    /**
     * Execute the job.
     *
     * @return void
     */
    public function handle()
    {
        $email = new SendEmailTest();
        Mail::to($this->details['email'])->send($email);
    }
}
```
# Step 8 – Run Development Server
Run the following command on command prompt to start server locally:
```php
php artisan serve
```
Then open browser and fire the following URL on it:


http://127.0.0.1:8000/email-test
# Conclusion
How to send mail using queue in Laravel 9; In this tutorial, we have learned how to create a mailable class in laravel 9. And using this class on how to send emails in laravel 9.
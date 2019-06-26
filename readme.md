=================================================================================================
Introduction
=================================================================================================

Hi Guys ,
My name is Akash Yellgetti,I m being using laravel framework 5.3 for few months,
which require PHP 5.5+  and i thought to make some videos ,
how i use laravel and hope it will help you too.

Today, In this section we will learn how to integrate Jwt-Auth in laravel.

Api Authentication in laravel using Jwt
(RESTfull Api authentication using Jwt)

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/O-hVQG3_W6k/0.jpg)](https://www.youtube.com/watch?v=O-hVQG3_W6k)

Before,Starting the developement of the app create virtual host and exact the laravel
file in that folder
 
I have my own virtual host named example.com and i will install the larave in that folder

and all of this  write up is mentioned in description of the video

lets start 


i m already in to example.com folder so 

i had previously migrated the migration so i deleting migration and will do it again 

i thing  download is completed so.there was some issues i have resolved it now its downloaded 


===================================================================
Steps
===================================================================
Step 1:-
Install Laravel 
composer create-project --prefer-dist laravel/laravel example.com

here example.com is the folder in which entire laravel would be installed,
instead of exampl.dev you can give any name you want .

Also if you are using linux then change your permission to 0777

now go to the folder in which you laravel is installed

mine is example.com
therefore , cd /var/www/example.com

now ,you may be able to view the laravel default homepage 
lets check this 
this could happen bcoz of permission


As we know laravel provide default authenication system ,which we can create using following command
php artisan make:auth

if you want to use  then its ok ..............even not then follow the steps

now go to your .env file to change your database credentials
php artisan migrate
so that for our authenication system we would need user and password_reset tables.

--------------------------------------------------------------

Step 2:-
Once you have installed the framework 

Github :- https://github.com/tymondesigns/jwt-auth/wiki

now its time integrate Jwt-auth Package using composer

i)Install via composer - edit your composer.json to require the package.

	"require": {
	    "tymon/jwt-auth": "0.5.*"
	}

	composer update

ii)composer require tymon/jwt-auth

---------------------------------------------------------------
Step 3:-
Once this has finished, you will need to add the service provider to the 
providers array in your app.php config as follows:

Tymon\JWTAuth\Providers\JWTAuthServiceProvider::class,
---------------------------------------------------------------
Step 4 :-
Next, also in the app.php config file, under the aliases array, you may 
want to add the JWTAuth facade.
'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,

---------------------------------------------------------------
Step 5 :-
Also included is a Facade for the PayloadFactory. This gives you finer control over the payloads you create if you require it
'JWTFactory' => Tymon\JWTAuth\Facades\JWTFactory::class,

---------------------------------------------------------------
Step 6 :-
Now come to you command line and run following commands

my is 5.3 then
Laravel 4:

	--php artisan config:publish tymon/jwt-auth

	--php artisan jwt:generate

Laravel 5:

	--php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"

	--php artisan jwt:generate

----------------------------------------------------------------
Step 7 :-
Now go to your LoginController 

copy paste the the lines

as we are create token system authenication so we dont need any csrf authentication
comment folloing line in kernel



use Illuminate\Http\Request;
use JWTAuth;

class LoginController extends Controller
{
  

    public function authenticate(Request $request)
    {
        // grab credentials from the request
        $credentials = $request->only('email', 'password');
        // dd($credentials);
        try {
            $token = JWTAuth::attempt($credentials);
            dd($token);
            // attempt to verify the credentials and create a token for the user
            if (! $token) {
                return response()->json(['error' => 'invalid_credentials'], 401);
            }
        } catch (JWTException $e) {
            // something went wrong whilst attempting to encode the token
            return response()->json(['error' => 'could_not_create_token'], 500);
        }

        // all good so return the token
        return response()->json(compact('token'));
    }
}

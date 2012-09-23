An Introduction to Zend Framework 2 for the Zend Framework 1 Developer - Part 1
zf2-for-zf1-users-part-1
06092012
true

An Introduction to Zend Framework 2 for the Zend Framework 1 Developer - Part 1
==

Having worked with ZF1 for a lot of years, I've been watching the development of ZF2 with interest. I've tried nearly every version, from the heady days of the pre-Skeleton App, right through the betas and release candidates.

With the imminent release of ZF2 proper, I thought I would share with you some of the most obvious differences between ZF1 and Zf2. Of course, these are only my opinions. While I've contributed (lightly), I've learned most of my ZF2 knowledge from hanging around in IRC and badgering people. If you want to learn quickly and have no remorse for annoying others, you should too!

Getting Started
--
There is currently no command line tool for ZF2. Let me repeat that. There is currently no Zend_Tool cli deployment tool for ZF2. It's not so bad, honest. Instead, there is a GitHub project containing a skeleton application that contains all the basic files and configurations to get you started.

https://github.com/zendframework/ZendSkeletonApplication

Once you've cloned a copy of the application locally, you can use composer to install it's dependencies (currently only ZF2). Composer is brilliant. It's a dependency/package manager that lets you grab any code you need from various locations just by modifying a json text file and running a php command line script. If you are serious about learning to use ZF2, I suggest you have a play with it, it's great. THe documentation for the skeleton app includes instructions on using composer to pull ZF2. Composer also handles all your auto-loading needs, so you don't need to manually configure your PHP autoloading, which is another great reason to be using it.

Namespaces
--

Lots of people will have been using namespaces since the release of PHP 5.3 in 2009. I'm sure that lots of people (like me) will have been happily working away with their chosen framework and *not* used namespaces. For me, jumping from ZF1 to ZF2 also meant jumping from `Long_Unweildy_Underscore_Seperated_Class_Names` to nice `Snappy\Namespaced\Classes`. If there's one thing in PHP I wish I'd started using earlier, it's namespaces.

Namespaces basically allow you to have short, snappy class names without worrying about collisions. The reason the ZF1 uses incredibly verbose underscore separated class names like `Zend_Db_Table_Abstract` is because if it was just called `TableAbsract` then there is no guarentee that some other code library won't be included that also has a class named `TableAbstract`, and errors abound.
In the pre-namespace world, the way to try and avoid collisions was to attempt to be unique through being verbose. While this works, it's hardly any fun for the developer who has to remember very long and tedious class names.

Up steps the namespace. It allows you to set a base namespace across many files, that allows those files to share a namespace. You can then declare any classes you wish to share that namespace without fear of collision.

~~~
<?php
namespace Spabby;
class Client
{
}
~~~

~~~
<?php
namespace Zend;
class Client
{
}
~~~

This is fine, no errors will occur, because I need to use the Fully-Qualified-Namespace (FQN) to reference these classes; 
`$client = new \Spabby\Client();`
"How does this help!" I hear you cry. Because in the namespace section of any PHP file, we can also tell the parser to import other namespaces via the `use` command.

~~~
<?php
namespace Spabby;
use Zend\Http\Client;

class Hello
{
    public function world()
    {
        $client = new Client();
    }
}
~~~

Because of the `use` statement, PHP will instantiate a new Zend\Http\Client. Clever.

Obviously, this is a very lightweight introduction to namespaces, they deserve a blog post all of their own. Because I'm lazy, and because someone inevitably will have done it better, to learn more check out Rob Allen's primer here:

http://akrabat.com/php/a-primer-on-php-namespaces/

Matthew Weier O'Phinney also has a blog post:

http://mwop.net/blog/254-Why-PHP-Namespaces-Matter.html

or php.net's introduction:

http://uk.php.net/manual/en/language.namespaces.php

Modules
--
### ZF1
Modules in Zend Framework 1 aren't really modular at all. ZF1 modules are really a method of grouping descrete areas of code together, without them being inherently reusable.
It's impossible to grab a ZF1 module that someone else has used, and quickly and easily drop it into your project. Set up Zend Framework 2.

### ZF2
Zend Framework 2's modules really are modular. You can drop in someone else's module, and with a simple line added to your application config file,
you can be up and running in seconds. One of the key objectives when writing ZF2 was that it should make modular code-reuse as simple and easy as possible.
I think this has been achieved with distinction. Once the ecosystem is established and mature, you should be able to easily add common functionality to your ZF2 project by
 simply picking and choosing the modules you need from the repository.

To quote a famous telephone manufacturer, need user registration and login functionality? There's a module for that.

The Zend Framework Commons team are a group of Zend selected community contributors who are tasked with trying to "certify" high quality modules with the `Zf-Commons` namespace.
`ZfcUser` is a user registration and login module written and maintained by them. Because it's `Zf-Commons`, it has an implied level of quality and maintenance. So, if you want
to add user login and registration, it's simply a case of adding the ZfcUser dependency to your composer.json file:

~~~
    "require": {
        "zf-commons/zfc-user": "dev-master"
~~~

Now, when you do a `php composer.phar update`, composer will pull in `ZfcUser` (and it's dependency `ZfcBase`) into your "vendor" library directory. Including the module is just a simple
case of adding `ZfcUser` to your list of installed modules in `application.config.php`. The module needs a little configuration (create the database tables it uses and tell it which
database connection to use), but because modules have their own routes, controllers and view scripts, once it's enabled you have a fully working user registration and authentication
system available at the `/user` entry point in minutes.

Bootstrapping
--
###ZF1
Bootstrapping resources in ZF1 has evolved to be a combination of directives in the `application.ini`, and underscore prefixed functions in a module's `Bootstrap.php`. Generally, things
that can be instanciated with minimal or no configuration can be added to the `application.ini`, whereas more complex resources that may need dependencies or similar are handled in the Bootstrap.php.
Each module can (and should) have it's `Bootstrap.php`. As an example, you can bootstrap the `Zend_Translate` object using an _initTranslate method in the application `Bootstrap.php` like this:

~~~
    protected function _initTranslate()
    {
        $translate = new Zend_Translate('array', APPLICATION_PATH.'/lang/en_EN.php', 'en_EN');
        Zend_Registry::set('Zend_Translate', $translate);
    }
~~~

This instanciates a new Zend_Translate object using the array adapter, and adds the English translations from the en_EN.php array file. It then puts it into the registry, having a 'Zend_Translate'
key in the registry automatically enables the translate view helpers.

You could also use the `application.ini` (or `Bootstrap.php`) to include front controller plugins. Front controller plugins allow you to hook into any part of the dispatch process and run custom
code so that you can perform actions on every request. A great example of code that is typically performed on every request is authentication. ZF1 expects the developer to write his own authentication
methods using Zend_Auth, and usually this was handled in a FC plugin:

~~~
class Central_Plugin_AuthPlugin extends Zend_Controller_Plugin_Abstract
{
	 public function dispatchLoopStartup(Zend_Controller_Request_Abstract $request)
	 {
	 	// if not logged in, redirect to the login page
	 	// create an instance of Zend_Auth
	 	$auth = Zend_Auth::getInstance();
	 	// if not logged in
		if (!$auth->hasIdentity())
		{
			// set the new controller name to be the login controller
			$request->setControllerName('login')
			// set dispached to be false so the dispatch loop starts again
			->setDispatched(false);
		}
	 }
}
~~~

Which is loaded in `application.ini` by:

~~~
resources.frontController.plugins[] = "Plugin_AuthPlugin"
~~~

###ZF2
There is no application level bootrapping in Zend Framework 2, each module is responsible for bootstrapping it's own resources in it's `Module.php`.
This is done using a combination of the `onBootstrap` method of the module class, and the Event Manager. We'll go into the Event Manager in more detail
in Part 2. Realistically, most bootstrapping is no longer needed; it's been replaced by entries in the Service Manager (covered in Part 2), and event
hooks (also covered in Part 2!), but as an example, here is how you can attach an event hook to the

~~~

public function onBootstrap($e)
{
    $config => array(
        'locale' => 'en_US',
        'translation_file_patterns' => array(
            array(
                'type'     => 'gettext',
                'base_dir' => __DIR__ . '/../language',
                'pattern'  => '%s.mo',
            ),
        ),
    );

}
~~~

Part 2
---
Part two will be released soon, and will include such wisdom as

+ Routing
+ Event Manager
+ Service Manager
+ Db
+ Views




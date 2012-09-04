An Introduction to Zend Framework 2 for the Zend Framework 1 Developer
==

Having worked with ZF1 for a lot of years, I've been watching the development of ZF2 with interest. I've tried nearly every version, from the heady days of the pre-Skeleton App, right through the betas and release candidates.

With the imminent release of ZF2 proper, I thought I would share with you some of the most obvious differences between ZF1 and Zf2. Of course, these are only my opinions. While I've contributed (lightly), I've learned most of my ZF2 knowledge from hanging around in IRC and badgering people. If you want to learn quickly and have no remorse for annoying others, you should too!

Getting Started
--
There is currently no command line tool for ZF2. Let me repeat that. There is currently no Zend_Tool cli deployment tool for ZF2. It's not so bad, honest. Instead, there is a GitHub project containing a skeleton application that contains all the basic files and configurations to get you started.

https://github.com/zendframework/ZendSkeletonApplication

Once you've cloned a copy of the application locally, you can use composer to install it's dependancies (currently only ZF2). Composer is brilliant. It's a dependancy/package manager that lets you grab any code you need from various locations just by modifying a json text file and running a php command line script. If you are serious about learning to use ZF2, I suggest you have a play with it, it's great. THe documentation for the skeleton app includes instructions on using composer to pull ZF2. Composer also handles all your auto-loading needs, so you don't need to manually configure your PHP autoloading, which is another great reason to be using it.

Namespaces
--

Lots of people will have been using namespaces since the release of PHP 5.3 in 2009. I'm sure that lots of people (like me) will have been happily working away with their chosen framework and *not* used namespaces. For me, jumping from ZF1 to ZF2 also meant jumping from `Long_Unweildy_Unscore_Seperated_Class_Names` to nice `Snappy\Namespaced\Classes`. If there's one thing in PHP I wish I'd started using earlier, it's namespaces.

Namespaces basically allow you to have short, snapp classnames without worrying about colissions. The reason the ZF1 uses incredibly verbose underscore seperated classnames like `Zend_Db_Table_Abstract` is because if it was just called `TableAbsract` then there is no guarentee that some other code library won't be included that also has a class named `TableAbstract`, and errors abound.
In the pre-namespace world, the way to try and avoid collisions was to attempt to be unique through being verbose. While this works, it's hardly any fun for the developer who has to remember very long and tedious class names.

Up steps the namespace. It allows you to set a base namespace across many files, that allows those files to share a namespace. You can then declare any classes you wish to share that namespace without fear of collision.

```<?php
namespace Spabby;
class Client
{
}

<?php
namespace Zend;
class Client
{
}
```
This is fine, no errors will occur, because I need to use the Full-Qualified-Namespace (FQN) to reference these classes; 
`$client = new \Spabby\Client();`
"How does this help!" I hear you cry. Because in the namespace section of any PHP file, we can also tell the parser to import other namespaces via the `use` command.
```
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
````
Because of the `use` statement, PHP will instanciate a new Zend\Http\Client. Clever.

Obviously, this is a very lightweight introduction to namespaces, they deserve a blog post all of their own. Because I'm lazy, and because someone inevitably will have done it better, to learn more check out Rob Allen's primer here:

http://akrabat.com/php/a-primer-on-php-namespaces/

or php.net's introduction:

http://uk.php.net/manual/en/language.namespaces.php

Modules
--

Bootstrapping
--

Routing
--

Service Manager
--

Db
--

ViewModels
--




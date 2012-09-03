An Introduction to Zend Framework 2 for the Zend Framework 1 Developer
==

Having worked with ZF1 for a lot of years, I've been watching the development of ZF2 with interest. I've tried nearly every version, from the heady days of the pre-Skeleton App, right through the betas and release candidates.

With the imminent release of ZF2 proper, I thought I would share with you some of the most obvious differences between ZF1 and Zf2. Of course, these are only my opinions. While I've contributed (lightly), I've learned most of my ZF2 knowledge from hanging around in IRC and badgering people. If you want to learn quickly and have no remorse for annoying others, you should too!

Getting Started
--
There is currently no command line tool for ZF2. Let me repeat that. There is currently no Zend_Tool cli deployment tool for ZF2. It's not so bad, honest. Instead, there is a GitHub project containing a skeleton application that contains all the basic files and configurations to get you started.

https://github.com/zendframework/ZendSkeletonApplication

Once you've cloned a copy of the application locally, you can use composer to install it's dependancies (currently only ZF2). Composer is brilliant. It's a dependancy/package manager that lets you grab any code you need from various locations just by modifying a json text file and running a php command line script. If you are serious about learning to use ZF2, I suggest you have a play with it, it's great. THe documentation for the skeleton app includes instructions on using composer to pull ZF2. Composer also handles all your auto-loading needs, so you don't need to manually configure your PHP autoloading if your using it, which is another great reason to be using it.




Namespaces
--

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




Starting apache server on MacOS
From console command line: sudo apachectl

User will be prompted for MacOs user password in order to run the <sudo> command

Running from a localhost environment using Apache
http://localhost/~kevinbodie/LandMaverick/web/lm_app/





# LandMaverick

Javascript application to read in data from a .csv file and generate a series of Heatmaps. This program is used in conjunction with a robot that provides the data to drive the display.

Reminder - For running on Mac OS and using the Apache Server will require configuration of the Apache Server. Details are below. The text is easier to read via a Web Browser:

https://medium.com/@JohnFoderaro/how-to-set-up-apache-in-macos-sierra-10-12-bca5a5dfffba

How to Set up Apache in macOS Sierra 10.12
John Foderaro
John Foderaro
Follow
Nov 27, 2016 · 14 min read

Originally posted on DigitalShore.io on August 08, 2016
This guide currently reflects macOS Seirra 10.12.1
Note: There have been several responses about 403 errors and access denied issues. Please be 100% certain to swap in your actual user name on your Mac, where ever there are instances of code using the placeholder of “username”. This step is highlighted several times throughout the guide and is critical that it’s adhered to in order for things to work.
Therefore, please read and re-read carefully and as needed.
During the World Wide Developers Conference on June 13, 2015, Apple officially announced that the next version of OS X will be branded as macOS. And with this latest iteration of the Mac desktop operating system — dubbed Sierra — the Californian landmark naming convention continues, too.
New features and enhancements are in store for macOS Sierra, but as web developers, we know that major OS updates can leave things a bit odd depending upon our development environments. For example, a previously working Apache web server in El Capitan will more than likely need to be reconfigured for Sierra. Especially if you preform a clean install when you update your operating system. And with that notion in mind, this guide aims to assist macOS Sierra users in having Apache up and running with minimal fuss.
Note: as of writing, macOS Sierra currently ships with Apache 2.4.23.
Working Through This Guide
This guide is taking a new approach as compared to my previous guides for Apache. I want to provide my readers with a few additional options and flexibility, as everyone’s needs are different.
For example, the initial Apache configuration will set you up with a local Sitesdirectory in your home folder where all of your local sites will live. You’ll then have the ability to navigate to http://localhost/~username/ to view an index of your local sites (where “username” in the URL is swapped out with your actual username). The first optional item is to leverage the customization of Virtual Hosts and local hostnames. The second optional item is to enable the Apache PHP Module.
Areas and items you will be working through:
Using Terminal
Creating Sites and project directories
Configuring Apache
Optional Items
Configuring Virtual Hosts and hostnames
Enabling the Apache PHP Module
1. Using Terminal
Many experienced web developers know how indispensable of a tool Terminal can be for completing their work. However, to users with inexperience using Terminal or even the command line in general, it can, at first glance, look downright daunting and uninviting! Therefore, this guide aims to make your time in Terminal as painless as possible, teaching you some fundamental concepts along the way.
1.1 A Word of Caution
Before you jump in to Terminal and begin executing commands, I want to add a word of caution. Several commands listed throughout this guide will make use of sudo, which allows for users to elevate their security privileges to that of “root” or “superuser”. With this great power comes great responsibility. Be certain that whenever you’re using sudo, you’re not mistyping file paths or changing files that shouldn’t be changed, etc. Simply put, proceed with caution while using sudo!
An alternative to meddling with native macOS files and programs via Terminal is to run a sandboxed Linux virtual machine directly on your Mac. This is actually a great approach for any developer and is a best practice that I highlyrecommend, regardless of your skill set and level. Another viable alternative is Homebrew, but that whole setup can get a bit messy.
Otherwise, if you’re comfortable with Terminal and just want your local version of Apache running on macOS Sierra, without messing around with virtual machines or Homebrew, then read on!
1.2 Terminal and This Guide
To get started with Terminal, you can find the app located at:
Applications > Utilities > Terminal.app
Go ahead and open it. You should have a window with content that looks similar to this — your computer name followed by a colon, a tilde, and your Mac user name ending with a dollar sign:
Johns-Air:~ John$
Commands will be entered after $. This guide will include $ indicating a single line or single command. Please note that $ should not be entered with every command. It’s just there to serve as a guide.
This guide will highlight some of the more common commands you’ll encounter while using Terminal and will not just gloss over these commands like most other guides. Instead, there will be some additional clarification and explanation as to what each command means and why.
Also, this guide will use absolute paths where necessary, as it can sometimes be a bit difficult for newcomers to find their way around the file system in Terminal.
2. Creating Sites and Project Directories
The Sites directory will serve as the new local Document Root for Apache, conveniently located within your user’s Home directory. As it turns out, this directory was actually the home of the original OS X “web sharing” setting that disappeared in OS X 10.7 Lion. Once you create the directory, you can see that Finder still pays homage to this feature of yesteryear, as the folder icon gets a special treatment, receiving an icon that appears strikingly similar to that of Safari’s.
After the Sites directory is created, we’ll add two “test websites” – Foo and Bar.
2.1 Create a Sites Directory in Home
First, we’re going to create a Sites directory within your Mac’s home directory using Terminal. To do this, we’ll use the mkdir command, short for make directory, with the text after the command representing the path and directory name we wish to create. The tilde is shorthand for your current user’s home directory, something that is very handy in Unix-like systems.
Enter the following command into Terminal to create your Sites directory:
$ mkdir ~/Sites
If you’re new to Terminal, you’re probably wondering whether or not the above command actually did something. And since most items in Terminal never really give any feedback, a handy command to list the contents of your current directory is ls. To list the contents of your Home directory, enter the following:
$ ls ~
Sites will now be listed as one of the items within your Home directory.
2.2 Create Two Project Directories within Sites
Next, we’ll be creating two additional directories within Sites. These two directories will serve as the “test websites” for Apache.
To begin, let’s use the mkdir command again. The command below will create a directory named foo within Sites:
$ mkdir ~/Sites/foo
Go ahead and repeat this action again, this time, to create a bar directory:
$ mkdir ~/Sites/bar
To ensure these directories exist, you can check again using:
$ ls ~/Sites
Both foo and bar will be printed to Terminal.
2.3 Create an HTML File
Now that the two test directories are present, we need some files for Apache to serve. Let’s keep things basic and straightforward with a simple HTML file for now. To create this new file, use the touch command. This command allows you to create a new empty file or even change a timestamp on a file or directory:
$ touch ~/Sites/foo/index.html
After executing the above command, an empty index.html file will added to ~/Sites/foo.
2.4 Hello, World!
However, an empty index.html file doesn’t really help us much! Let’s add some HTML to this file, using the nano text editor, which is a text editor designed for Unix-like command line interfaces – such as Terminal:
$ nano ~/Sites/foo/index.html
After running the above command, you’ll notice that the Terminal window now looks more like a text editor — with the file name at the top, a cursor indicating where text can be entered, and several commands listed at the bottom (note: amongst the commands at the bottom of nano, ^ represents the control key).
Add the following to ~/Sites/foo/index.html:
<!doctype html>
<html>
  <head>
    <title>Hello, World! | Foo</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <p>Welcome to <strong>Foo</strong>.</p>
  </body>
</html>
Close and save this file, using control + X, followed by Y, then Enter. These commands tell nano to exit the file and to save to the current file name (or, conversely you can press N when you want to close but do not want to save).
What about Bar ?
Awesome, we have an index.html file in ~/Sites/foo, but what about ~/Sites/bar? Instead of manipulating files via Finder, below is a great example of where Terminal can afford you some greater flexibility. Without needing to click through several folders and windows to copy ~/Sites/foo/index.html, you can use the cp command to copy the file and just swap out instances of the word “foo” for “bar” using nano:
$ cp ~/Sites/foo/index.html ~/Sites/bar/index.html
Next, go ahead and open the file with nano:
$ nano ~/Sites/bar/index.html
Your end result should in ~/Sites/bar/index.html look like this:
<!doctype html>
<html>
  <head>
    <title>Hello, World! | Bar</title>
  </head>
  <body>
    <h1>Hello, World!</h1>
    <p>Welcome to <strong>Bar</strong>.</p>
  </body>
</html>
With two test directories in place, it’s time to shift gears and begin configuring Apache for macOS Sierra.
3. Configuring Apache
With your directories all set, let’s begin editing a few configuration files to have Apache serve content from the ~/Sites directory.
Also, you’ll notice that throughout this section of the guide, sudo will be used rather frequently. That’s because most standard users on a Mac are not privileged enough to edit these files directly.
3.1 Adding a Config File
Our first task is to create a new configuration file that helps tell Apache where to serve content from. This configuration file is actually your username on your Mac and ends with the .conf extension. For example, I would have to create a file titled John.conf.
If you’re not certain what your exact username is (case sensitivity and all), then you’ll want to run the following command to print your username to the Terminal window:
$ whoami
Now that you know your username, navigate to the directory where we need to place this new file. To do this, we’ll need to change directories with the cdcommand. This allows us to navigate into and out of our current working directory to wherever we need to go. For our purposes, we simply need to enter the following command to take use to the /etc/apache2/usersdirectory:
$ cd /etc/apache2/users
Next, lets create the username.conf file (using your Mac username, not the word “username”). This time, we won’t really bother using touch as we had done throughout step 2. There is actually a little shortcut we can use instead, where we simply specify the new file’s name while using nano. If the file exists, nano will open the existing file, otherwise, nano will open a blank text editor and it will allow us to save this file in our current working directory as the specified file name.
Also, for this step we’ll be using the sudo command for the first time in this guide. As pointed out above in step 1, sudo is a special command that if used incorrectly or haphazardly, can damage and destroy system files. Please proceed with caution!
$ sudo nano username.conf
Be sure to replace “username” with your actual username!
With this new file open in nano, go ahead and add the following contents, being sure to replace “username” with your actual username on your Mac:
<Directory "/Users/username/Sites/">
  AllowOverride All
  Options Indexes MultiViews FollowSymLinks
  Require all granted
</Directory>
Once the above is set, close nano with control + X, followed by Y, then Enter.
Next, let’s adjust the permissions a bit so that Apache can read this new file. Enter the following into Terminal, once more replacing “username” with your actual username:
$ sudo chmod 644 username.conf
Without going into too much detail on Linux file permissions, the three numbers places for 644, listed above, represent the following:
Owner Group Other
The “6” in “Owner” means the owner can read/write. The “4” and second “4” restricts “Group” and “Other” to read only.
Now that the user configuration file has been created and it has the proper permissions, let’s get to turning on a few crucial Apache modules.
3.2 Turning on Modules in httpd.conf
Our next step is to move “back” or “up” one level, to /etc/apache2. You should currently be in /etc/apache2/users from the previous step. To change directories one level, you simply need to use this common shorthand coupled with the cd command: two periods instead of an actual directory name:
$ cd ..
This may look familiar to some developers who have seen this used in paths for CSS or JavaScript files. These two dots are shorthand for your current directory’s parent directory. But what if you want to double check the exact file path of where you’re at currently? There’s a command for that too, pwd, which will print the working directory for you, listing exactly where you’re located:
$ pwd
When I run that command, I get: /etc/apache2.
Within /etc/apache2, let’s first make a backup of the existing httpd.conf file. To begin, use the cp command to copy and then add .bak as the file extension of the copied version:
$ sudo cp httpd.conf httpd.conf.bak
I find it useful to keep a copy of these default configuration files handy incase there’s a need to ever revert back to the original, or at the very least, reference the original.
With the backup now stashed, we can focus our efforts on editing httpd.conf. Open that file with nano:
$ sudo nano httpd.conf
In this file, we’ll need to “turn on” several modules. To do this, it’s a matter of removing the # symbol, which acts as a way of commenting the entire line out, effectively “turning it off”.
Nano offers a search capability that makes for finding each line a little easier, as this file is rather lengthy. To use it, press control + W (it behaves similar to command + F in your web browser).
Listed below are the modules to uncomment/enable:
LoadModule authz_host_module libexec/apache2/mod_authz_host.so
LoadModule authz_core_module libexec/apache2/mod_authz_core.so
LoadModule userdir_module libexec/apache2/mod_userdir.so
LoadModule vhost_alias_module libexec/apache2/mod_vhost_alias.so
Include /private/etc/apache2/extra/httpd-userdir.conf
Include /private/etc/apache2/extra/httpd-vhosts.conf
Once you have searched for each line above and removed the prepending #, save and close httpd.conf.
3.3 Edit httpd-userdir.conf
With the httpd.conf file updated, the next file to edit is httpd-userdir.conflocated in the extra directory:
$ cd /etc/apache2/extra
Just like before, it’s good practice to keep a spare copy of the default configuration file around — just to be safe:
$ sudo cp httpd-userdir.conf httpd-userdir.conf.bak
With the backup all set, open and edit httpd-userdir.conf:
$ sudo nano httpd-userdir.conf
Uncomment the following line, removing #:
Include /private/etc/apache2/users/*.conf
Note: if you’re curious about the PHP Module it will be explored in the “optional steps” listed below:
3.4 Restarting Apache
With all of our settings and changes in place, it’s time to restart Apache and test out our setup:
sudo apachectl restart
Viewing Your Local Sites
Navigate to http://localhost/~username, replacing username with your username. If everything is successful, you will see an index of your Sites directory — listing out all sub directories, with both the ~/Sites/foo and ~/Sites/bar folders listed. Upon clicking one of these folders, Apache then servers the contents of that folder, just like a typical web server.
Congratulations, you now have a local Apache web server on your Mac running macOS Sierra 10.12!


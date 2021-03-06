# WP CLI

## Install WordPress with WP-CLI
### Core download install

This will grab all the current WordPress files from the github WordPress repo, extract them and put them inside your site project folder. This is a huge time saver as it can install WordPress in seconds (with a fast internet connection)

```
$ wp core download
```

### Create the wp-config.php file

If you were manually installing WordPress through the browser you would be brought to a page where you put in your database connection information. This would in turn create your `wp_config.php` file. WP-CLI speeds this step up with a little terminal magic.

Before you do this step you need to create a database in MySQL. Using the GUI phpMyAdmin is the way most people do this. You can also use the Terminal, sign into MySQL and create a database this way. As you use the terminal more and more the second option may become your first choice.

So now you are ready to create your `wp-config.php` file

```
$ wp core config --dbuser=root --dbpass=root --dbname=wp_bootstrap
```

This will create the file to connect you to your MySQL databse. The above code is assuming you used MAMP and the default username and password for MAMP is root and root. Change the --dbname value to match the name you used when creating the empty database in phpMyAdmin (or MySQL terminal). I recommend using underscores instead of dashes in db names with more than one word (ie my_db instead of my-db). For security reasons you may want to not have db in name so hackers won't find it so easily.

### Finishing Up Core Install

If you were manually installing WordPress through the browser you would be brought to a page asking you for your username and password, title of the page, email and URL of your WordPress site (local, staging or production depending on the environment you are working in). WP-CLI speeds this step up with the magic of the terminal. 

```
$  wp core install --url=http://localhost/luthier-builder --title=WPLuthierBuilder --admin_user=peh2 --admin_password=password --admin_email=howley.phil@gmail.com
```

* sometimes you may have to rename localhost in wp-config.php to 127.0.0.1 to avoid database connection error

If you get the above error try to change the file permissions:

```
$ chmod 644 wp-config.php
```

* If you get success, wordpress has been installed on command line. 
* Log in with the credentials you just created.

**IMPORTANT:** Manually Check `Discourage search engines from indexing this site`
WP-CLI enables you to update WordPress from the terminal. It is a great improvement in the traditional way to update WordPress.

## Core Update

```
$ wp core update
$ wp core update-db
```

## Deactivate all plugins at one time

```
$ wp plugin deactivate --all
```

## Back up DB

[helpful link](https://blog.sucuri.net/2015/07/wp-cli-guide-secure-wordpress-backup-update.html)

```
$ wp db export
```

### Update all plugins 

```
wp plugin update --all
```

### Update all themes

```
wp theme update --all
```

## Create list of current plugins or themes

```
$ wp plugin list
$ wp theme list
```

## Sample ways to modify plugins

```
$ wp plugin install akismet
$ wp plugin activate akismet
$ wp plugin update akismet
$ wp plugin deactivate zeroday
$ wp plugin delete zeroday
```

## Sample ways to modify themes

```
$ wp theme install twentyfifteen
$ wp theme activate twentyfifteen
$ wp theme update twentyfifteen
$ wp theme disable twentyfifteen
$ wp theme delete twentyfifteen
```

## How to install WP-CLI

```
# cd public_html
# curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
```

Check if WP-CLI works

```
$ php wp-cli.phar --info
```

### WP command
We want to just have to type `wp` as the command instead of `php wp-cli.phar` so in order to accomplish this, do this:

```
$ chmod +x wp-cli.phar
$ sudo mv wp-cli.phar /usr/local/bin/wp
```

Now if you type the following:

```
$ wp --info
```

Output will look something like below:
![output](https://i.imgur.com/1t3tQ45.png)

Make sure you are in the correct folder. The correct folder will be where you want to install WordPress.

If you are using MAMP and set up `Sites` as your Document Root. the path would be

```
$ cd ~/Sites/my-wordpress-project
```

or if you are on a remote box, the path might be...

```
$ cd /var/www/wp-project
```

Where to get name for `theme/plugin`?

If you want to install a new plugin but you don’t know the slug for it, you can find it as a directory under [WordPress.org](http://wordpress.org)

Of course, take care to only install plugins and themes from trusted sources because pirated and nulled themes and plugins are dangerous and often contain some nasty backdoors.

### Problems with MAMP and WP-CLI
* Check out [mamp.md](https://github.com/kingluddite/web_dev_notes/blob/master/workflow/development/mamp.md)

This is a common error. I've run into a bunch of different ones and spend more time then I would have liked to troubleshoot and resolve them. The main error has to do with something like `database connection`. MAMP is usually the problem. I think because I moved from MAMP to virtual box, vagrant, vvv I ran into problems because I first started with MAMP and set my .zshrc to be working with MAMP and then when I moved to virtualbox and went back and forth between MAMP and virtual box, I think I just had to update my .zshrc with the correct info. Just make sure whatever you are using the PHP and MySQL are pointing to the right place.

add this to your `.zshrc` file

**note** I use oh-my-zsh instead of bash. Lots of developers love oh-my-zsh and so I moved from bash to the zsh. I highly recommend Wes Bos 10 free videos on getting up and running with zsh.

`.zshrc`

* make sure your php version is the correct folder

```
##
# WP-CLI / MAMP compat
##
export MAMP_PHP=/Applications/MAMP/bin/php/php5.5.10/bin
export PATH="$MAMP_PHP:$PATH:/Applications/MAMP/Library/bin"
```

[ref](http://laurenpittenger.com/wpcli-error-establishing-database-connection-mamp/)

**IMPORTANT:** Remember to refresh bash or zshr with:
```
$ source ~/.bash_profile
# or
$ source ~/.zshrc
```

## WP-CLI

[View On Kingluddite](http://www.kingluddite.com/cms/install-wp-cli-for-wordpress-on-namecheap-com)

I wanted to get WP-CLI working on GoDaddy so this article helped me set that up.

[how to use WP-CLI on godaddy](https://www.godaddy.com/help/how-to-install-wordpress-cli-on-shared-hosting-12363)

# namecheap

[Namecheap.com](http://namecheap.com) offers shared hosting. They give you a cpanel just like godaddy and others

Once you figure out how to `SSH` (this is not enabled by default and you have to use the online chat, provide usernames and passwords to them and they will enable it). It will take 10 minutes before it `SSH` will work once they enable it.

You then need to add your public key to them so you can `SSH`

Now you need to install `WP-CLI`. Here's how.
SSH into their server, something like:

* you can find the server name by looking at your URL once you are logged into the cpanel
    - don't forget the port number (-p21098) for shared host

```
$ ssh codegod@server333.web-hosting.com -p21098
```

Bam! You're into their box.

## Browse to your home directory

Should be something like

```
/home/codegod
```

## Install WP-CLI
Create a `wp` folder and `cd` into it

```
$ mkdir wp
$ cd wp
```

### Download WP-CLI

```
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
```

Go back to your home directory (where you just were) by using:

```
$ cd ../
```

## Add aliases to .bashrc

If you don't see it look for it using 

```
$ ls -la
```

### Open .bashrc in vim

```
$ vi .bashrc
```

Make `.bashrc` look similar to this (I added the 2 aliases)

```
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

# User specific aliases and functions
alias php='php -d suhosin.executor.include.whitelist=phar'
alias wp='php ~/wp/wp-cli.phar'
```

Close and save `.bashrc` using `:wq!`

* write(`w`), quit(`q`), force(`!`)

Refresh `.bashrc`

```
$ source .bashrc
```

### Test if WP-CLI works with `wp` shortcut

```
$ wp
```

If you get something similar to the following, your `WP-CLI` was successful:

```
prompt$ wp --info
(and you should get something like this:)
PHP binary:     /usr/selector/php-cli
PHP version:    5.4.35
php.ini used:   /usr/local/lib/php.ini
WP-CLI root dir:        phar://wp-cli.phar
WP-CLI global config:
WP-CLI project config:
WP-CLI version: 0.17.1
```

## Add WP-CLI tab completions

Go back into the `wp` folder you created earlier and download this repo from github.com

```
curl -O https://raw.githubusercontent.com/wp-cli/wp-cli/master/utils/wp-completion.bash
```

Then in your home directory add this to your `.bash_profile`

```

# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# Bash completions for wp-cli. (You can not use the ~/ shortcut for $HOME)
source /home/codegod/wp/wp-completion.bash
# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH
```

* I just added the line above beginning with `source`
* save with `:wq!`
* refresh with `$ source .bash_profile`

And that's it. You will have `WP-CLI` and tab completing installed and working.

## Test to make sure

Navigate to a folder you want to install WordPress on your remote namecheap shared hosting server.

Create the folder.
```
$ mkdir some-wordpress-project
```

`cd` into that folder

```
$ cd some-wordpress-project
```

Install core WordPress into that project

```
$ wp core download
```

If it works, you did it. Congrats!

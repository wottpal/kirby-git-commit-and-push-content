# Kirby - Git Commit And Push Content

This is a plugin for [Kirby](http://getkirby.com/) that Commits and Pushes Changes made via the Panel to your Git Repository.

## Usage

Just keep using the Panel as you are used to and watch the commits appear in your git repository!

## Installation

### Create a new git repository for your content

Create a new git repository where you push your content to, name it `your-project_content`.

Init the content repo and push it

Remove the `content/` folder from your current git repository
```
git rm --cached -r content
git add -A
git commit -m "Move Content Folder to separate repository"
``` 

Add the `content/` folder to new git repository

```
cd content
git init
git remote add origin https://github.com/your-project/your-project_content.git
git add -A
git commit -m "Initial Content Commit"
git push origin master
```

### Download and configure the Plugin

#### git submodules
Go into your `site/plugins/` folder and
```
git submodule add --name git-commit-and-push-content https://github.com/blankogmbh/kirby-git-commit-and-push-content.git site/plugins/git-commit-and-push-content
cd site/plugins/
git submodule update --init --recursive
```

#### Composer
If you [installed kirby via composer](https://forum.getkirby.com/t/kirby-2-4-with-composer/5664/3?u=pascalmh) open your projects composer.json and add
`blankogmbh/kirby-git-commit-and-push-content` as a requirement and a custom path so it will be installed into the site/plugins-Folder

Thats what your composer.json will look like afterwards:
```
{
    "name": "your-company/your-project",
    "require": {
        "mnsami/composer-custom-directory-installer": "1.1.*",
        "getkirby/kirby": "^2.4",
        "getkirby/panel": "^2.4",
        "blankogmbh/kirby-git-commit-and-push-content": "1.*"
    },
    "extra": {
        "installer-paths": {
            "./kirby": ["getkirby/kirby"],
            "./kirby/toolkit": ["getkirby/toolkit"],
            "./panel": ["getkirby/panel"],
            "./site/plugins/git-commit-and-push-content": ["blankogmbh/kirby-git-commit-and-push-content"]
        }
    }
}
```



#### copy & paste
Put all the files into your `site/plugins/git-commit-and-push-content/` folder. If the `git-commit-and-push-content/` plugin folder doesn't exist then create it.

### Options

You can use the following [Options](http://getkirby.com/docs/advanced/options) - make use of kirbys [Multi-environment setup](http://getkirby.com/blog/multi-environment-setup).

(In case you need to use multiple git users on your environment - [Multiple SSH Keys settings for different github account](https://gist.github.com/jexchan/2351996))

If you do not want to Pull and/or Push on every change you can also call `yourdomain.com/gcapc/push` or `yourdomain.com/gcapc/pull` manually (or automated with e.g. a cronjob).

#### gcapc-path
Type: `String`
Default value: `kirby()->roots()->content()`

path to the repository to work in

#### gcapc-branch
Type: `String`
Default value: `''`

branch name to be checked out (defauts to currently checked out branch)

#### gcapc-pull
Type: `Boolean`
Default value: `false`

Pull remote changes first?

#### gcapc-commit
Type: `Boolean`
Default value: `false`

Commit your changes?

#### gcapc-push
Type: `Boolean`
Default value: `false`

Push your changes to remote?

#### gcapc-cron-hooks-enabled
Type: `Boolean`
Default value: `true`

Whether `yourdomain.com/gcapc/push` and `yourdomain.com/gcapc/pull` are enabled or not.

#### gcapc-user-hooks-enabled
Type: `Boolean`
Default value: `false`

Whether to user- & avatar-hooks should be listened or not.

**Important:** By default this is disabled because Kirby [stronly discourages](https://github.com/getkirby/starterkit/blob/master/.gitignore) you to add neither `site/accounts` nor `assets/avatars` under version-control. But sometimes it comes in handy even though.

#### gcapc-panel-widget
Type: `Boolean`
Default value: `true`

Show or Hide the Panel widget.

#### gcapc-gitBin
Type: `String`
Default value: `''`

Sets the location where git can be found

[See Git.php](http://kbjr.github.io/Git.php/) `void Git::set_bin ( string $path )`

#### gcapc-windowsMode
Type: `Boolean`
Default value: `false`

[See Git.php](http://kbjr.github.io/Git.php/) `void Git::windows_mode ( void )`

## Git LFS
Your repository might increase over time, by adding Images, Audio, Video, Binaries, etc.
cloning and updating your content repostory can take a lot of time. If you are able to use
[Git LFS](https://git-lfs.github.com/) you probably should. Here is what the .gitattributes-File could look like:

```
*.zip filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.gif filter=lfs diff=lfs merge=lfs -text
```

## Author

Pascal 'Pascalmh' Küsgen <http://pascalmh.de>

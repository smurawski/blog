---
author: steven.murawski@gmail.com
categories: []
comments: true
date: 2015-06-01T00:00:00Z
tags: []
title: Building Your Chef Workstation on Windows

---

**UPDATE**
[2016-08-22 - I've got a shorter, faster method now.]({{< relref "simplified-chef-workstation.md" >}})

## So you want to Chef on Windows?



I've recently been rebuilding my workstations and I figured it was time to automate some of my setup and figured it would be easier to share this way too. Fortunately, another Chef, [Adam Edwards](https://github.com/adamedx) has a good start on this with his [winbox cookbook](https://github.com/adamedx/winbox). (Another coworker - [Joshua Timberman](https://github.com/jtimberman) has a [cookbook called Pantry](https://github.com/opscode-cookbooks/pantry) for a similar purpose, but I haven't taken the time to try that out yet.)


We'll build on Adam's cookbook (well, [my fork](https://github.com/smurawski/winbox/tree/smurawski/updates) of it for the moment) and I'll show you an example of a wrapper cookbook that puts some personalization on top of the basic configuration.


At the end, you'll have a functional workstation for building cookbooks, working on the Chef codebase (if you want), and extending Chef for fun and profit.


## Step 0 - Assumptions



For this process, I'll assume that you've got any SSH keys or Git config pre-staged in your user profile account. I also assume that you are running a modern operating system (at least Windows 7 - which is getting to be pretty ancient) with a recent version of PowerShell (minimum V3).


The last assumption is that you are running in an account with administrative privileges and running with the admin token.


## Step 1 - Get ChefDK



The first thing we have to tackle is installing ChefDK. [I just blogged about how we can do that]({{< relref "want-the-latest-chefdk-on-windows.md" >}}), so I'll wait while you check out that post and get ChefDK installed.


## Step 2 - create a Chef Repo



Since we'll be using Chef Client to configure our workstation, we'll need to work out of a Chef repo. This is a directory structure that Chef Client understands and can just find things. For our initial setup, the main thing is that there is a cookbooks directory. Fortunately, ChefDK comes with the *chef generate* command which will help us create that basic structure.


Now that we have Chef installed, we can create a Chef repo in which we'll build our wrapper cookbook. I'll use C:/workstation-repo as an example, but pick whatever you'd like.


In our shell,

```
cd c:/
chef generate repo workstation-repo
cd workstation-repo
```

## Step 3 - create our wrapper cookbook



The Chef repo we created gives us the starting place to create our cookbook. We'll use *chef generate* again to get us started. I'll call the cookbook "local" for now, but you can use any other name you'd like.


In our shell

```
pushd cookbooks
chef generate cookbook local --copyright 'Steven Murawski' --email 'someone@some.mail.com' --license apache2
popd
```

## Step 4 - Update cookbook metadata



I mentioned that we would be using this as a wrapper for (my fork of) Adam Edward's winbox cookbook. To do this, we'll add a dependency in the cookbook metadata. In cookbooks/local/metadata.rb

```
name 'local'
maintainer 'Steven Murawski'
maintainer_email 'someone@some.mail.com'
license 'apache2'
description 'Installs/Configures local'
long_description 'Installs/Configures local'
version '0.1.0'
depends 'winbox', '= 0.1.50'

```


Cookbook dependencies can be resolved in a number of ways, but for today we are going to use Berkshelf which is a common dependency resolution tool that ships with ChefDK.


Now, since we are using a forked version of this cookbook, we need to tell Berkshelf where to get the version we want.


In cookbooks/local/Berksfile

```
source 'https://supermarket.chef.io'
metadata
cookbook 'winbox', git: 'https://github.com/smurawski/winbox.git', branch: 'smurawski/updates'
```


This will tell Berkshelf to look in my GitHub repository for the winbox cookbook. Any other cookbooks that are required will be pulled from the [Chef Supermarket](https://supermarket.chef.io).


## Step 5 - Create our recipe



Now that we've got our dependency on our upstream cookbook in place, let's write a recipe to use that cookbook.


We'll take our inspiration from the default recipe in the winbox cookbook.


In cookbooks/local/recipes/default.rb

```
include_recipe 'git'
include_recipe 'winbox::chocolatey_install'
include_recipe 'winbox::powershell_dev'
include_recipe 'winbox::readline'
include_recipe 'winbox::editor'
include_recipe 'winbox::console'
include_recipe 'winbox::git'
```


Right off the bat, you may wonder where the git (really git::default) recipe is coming from. Well, the winbox cookbook has the git cookbook as a dependency.


## Step 6 - Adding Attributes



Now, the winbox::powershell_dev recipe includes a default PowerShell profile. I personally keep my WindowsPowerShell directory in a GitHub repo, which I symlink to ~/Documents/WindowsPowerShell. Fortunately, there is an attribute we can set to turn off the creation of that sample profile.


In our shell

```
chef generate attribute cookbooks/local default
```


Then we open up cookbooks/local/attributes/default.rb and add

```

# override the create_profile setting from the winbox cookbook
default['winbox']['create_profile'] = false

```


## Step 7 - Adding some software packages



### Some Chocolatey Goodness



There are a few other software packages I like to install. Since we have Chocolatey installed, let's add a recipe that uses that to grab the software we want.


First, let's add the following to cookbooks/local/attributes/default.rb

```
# packages to install
default['local']['packages'] = %w[7zip winmerge sysinternals]
```


Next, we'll create a recipe to handle the installation of those packages. In our shell,

```

chef generate recipe cookbooks/local packages --copyright 'Steven Murawski' --license apache2
```


Then, we'll add to cookbooks/local/recipes/packages

```
node['local']['packages'].each do |package|
  powershell_script "Install #{package}" do
    code "choco install -y #{package}"
    not_if "(choco list -l) -match '#{package}'"
  end
end

```


### And a few Ruby gems



The next thing we are going to do is install a few gems we'll likely need for testing and debugging.


In our cookbooks/local/attributes/default.rb,

```
# gems to install
default['local']['gems'] = %w[kitchen-pester kitchen-hyperv kitchen-dsc pry]
```


And then in our shell,

```
chef generate recipe cookbooks/local gems --copyright 'Steven Murawski' --license apache2
```


Then, in cookbooks/local/recipes/gems.rb,

```
node['local']['gems'].each do |gem|
  chef_gem gem do
    compile_time false if respond_to?(:compile_time)
  end
end
```


### Update the default recipe


```
include_recipe 'local::packages'
```


Finally, we'll update our default recipe to call this one. In cookbooks/local/recipes/default.rb


## Intermission



At this point, we've got most of what we need to get a workstation up and running. I'm going to continue customizing the setup, but feel free to skip to the last step where we apply the recipes to our system with Chef Client.


Our cookbook will


*   install the git command line tool and fix the path settings to make ssh available,
*   the chocolatey package manager,
*   set your execution policy to unrestricted,
*   install psreadline and conemu for a better command line experience,
*   install psget which is another PowerShell module package manager,
*   install Visual Studio Code as a text editor,
*   install posh-git to make git usable from PowerShell,
*   install 7zip, winmerge, and the sysinternals tools,
*   and a few gems that we'll want to when we start testing and debugging.

There is also one other customization you might want to add. By default, the winbox cookbook will install the Visual Studio Code editor. It can alternatively install vim, emacs, or atom. To adjust the editor choice, you'll need to add an attribute that identifies which editor you'd like to use. ([Find more here](https://github.com/smurawski/winbox/tree/smurawski/updates))


## Step 8 - Beyond the basics



Now that we've got the basics covered, there are a couple of things I like to get a jump start on as well.


I like create a source directory on my d: drive (I've usually got a second drive or I'm using boot to VHD). I then create a symlink to ~/source so it's nice and accessible. I then populate it with some of the git respositories I work with regularly. I also keep my WindowsPowerShell directory in a git repository and then symlink it to ~/Documents/WindowsPowerShell.


To get this in place, we'll create two recipes, one to handle setting up my git repositories and one to handle the symlinks.


### Setting up starting git repositories.



We will need a recipe and some attributes to get this going. We'll add to our cookbooks/local/attributes/default.rb

```
# git repositories to start with
default['local']['git_repos'] = {
  'chef'              => ['chef'],
  'smurawski'         => ['sample-windowspowershell'],
  'opscode-cookbooks' => ['iis',
                          'powershell',
                          'sql_server',
                          'windows'],
  'powershellorg'     => ['cActiveDirectory',
                          'cNetworking',
                          'cWebAdministration',
                          'DSC',
                          'StackExchangeResources']
}

# location of the source directory
default['local']['source_destination'] = "d:/source"
```


In the shell,

```
chef generate recipe cookbooks/local repositories --copyright 'Steven Murawski' --license apache2
```


Then in cookbooks/local/recipes/repositories, we can add

```

node['local']['git_repos'].each do |key, value|
  directory "#{node['local']['source_destination']}/github/#{key}" do
    recursive true
  end

  value.each do |repo|
    git "#{node['local']['source_destination']}/github/#{key}/#{repo}" do
      repository "https://github.com/#{key}/#{repo}.git"
      revision 'master'
      checkout_branch 'current'
    end
  end
end

```


### Creating the symlinks



Next up, we'll make a recipe to describe the symlinks for my source and WindowsPowerShell directories.


*NOTE: If you have a pre-existing PowerShell profile or ~/Documents/WindowsPowerShell directory, you'll want to skip creating the symlink to ~/Documents/WindowPowerShell.*


In our shell,


`chef generate recipe cookbooks/local links --copyright 'Steven Murawski' --license apache2`


Then, we can edit cookboks/local/recipes/links.rb to contain

```

directory "#{node['local']['source_destination']}"

link "#{ENV['USERPROFILE']}/source" do
  to "#{node['local']['source_destination']}"
end

link "#{ENV['USERPROFILE']}/Documents/WindowsPowerShell" do
  to "#{node['local']['source_destination']}/github/smurawski/sample-windowspowershell"
end
```


### Adding those steps



Now we need to include these as part of our default recipe. We can insert these steps right after calling the git recipe. Then our cookbooks/local/recipes/default should look like (after the comments at the top)

```
include_recipe 'git'
include_recipe 'local::repositories'
include_recipe 'local::links'
include_recipe 'winbox::chocolatey_install'
include_recipe 'winbox::powershell_dev'
include_recipe 'winbox::readline'
include_recipe 'winbox::editor'
include_recipe 'winbox::console'
include_recipe 'winbox::git'
include_recipe 'local::packages'
```


## Step 9 - Run it!



*If you want compare your cookbook to what I put together, you can find the finished cookbook on GitHub at https://github.com/smurawski/local *


Now that we've got our wrapper cookbook in place, along with some customizations, we can get ready to run our recipe.


First, we need to retrieve all our dependencies. To do this, we'll use Berkshelf. We'll point Berkshelf to the Berksfile in our local cookbook and tell it that we want the cookbooks to be placed in our cookbook directory (so that the Chef Client can find them when we run it). In our shell,


`berks vendor cookbooks -b cookbooks/local/Berksfile`


This will build a dependency tree for all the dependencies specified by our cookbook (and the dependencies of the dependencies). You'll see a Berksfile.lock created that will actually list the complete group of cookbooks identified and there versions.


*NOTE: Normally, I wouldn't vendor cookbooks right over the top of the cookbook I just developed. Rather than building a Chef repo, I normally build cookbooks in my source directory, create a Chef repo in another part of my file system, and then poing to the Berksfile in my source directory and use that to populate the cookbooks directory of my source repo. Since this process is being used to build out my system structure in the first place, it's ok to deviate from standard use.*


Now, we are ready to run! We'll start the Chef Client in local mode (which spins up an in-memory Chef Server called Chef Zero) and tell it to apply our default recipe from the local cookbook.


In our shell,


`chef-client --local-mode --override-runlist 'local'`


For those of you who like to type as little as possible, there is


`chef-client -z -o 'local'`


And that's it! You should now be set up with a workstation with all the basic tools to get started using Chef, building cookbooks, and going deeper into Chef itself. Enjoy!


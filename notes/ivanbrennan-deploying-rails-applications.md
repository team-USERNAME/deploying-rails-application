#Deploying Rails Applications

##1.0 Introduction

###Terminology
- VPN: virtual private server
- Redis: key-value store

###Prerequisites
- use SSH to connect to remote servers
- navigate the shell
- how to setup DNS

####Sample code available on Github

##Part 1 (Chef)
- Ch 2, 3: chef, servers, provisioning
- Ch 4: minimal command set
- Ch 5: step by step chef
- Ch 6-12: rails server template configuration
	- https://github.com/TalkingQuickly/rails-server-template
- Ch 13: Vagrant

##Part 2 (Capistrano)
- deployment
- Capistrano v Chef
- Avoiding falling foul of $PATH
- Virtualhost files
- SSL Certificates
- Unicorn
- log rotation
- cron jobs, Whenever
- background jobs, Sidekiq

##2.0 The Stack

- Ubuntu 12.04 LTS
- Nginx: reverse proxy
- Unicorn: web server for Rack apps
	- Unicorn v Passenger:
		- https://blog.engineyard.com/2012/passenger-vs-unicorn
	- https://github.com/blog/517-unicorn
- Postgresql / MongoDB / MySQL
- Ruby (rbenv)
- Redis
- Memcached

##3.0 Chef Definitions

####the 'simple approach' (slow, error-prone, unscalable)

- create new VPS on Digital Ocean, login via SSH, apt-get packages, tweak config files, add custom package sources where newer versions needed
- build server up in layers over time, keeping track in a text file or wiki

####Automation

- Chef: provisioning
- Capistrano: deployment

###Chef

- use ruby DSL to represent provisioning commands
- "role" (eg. rails app server, database server)
- "hub and spoke" servers arrangement (10's or 100's of servers)
- "solo" 1-10 servers, apply configurations manually as needed

###Knife

- command line tool: interface between local chef repository and remote server
- "knife solo"
	- https://github.com/matschaffer/knife-solo

###Berkshelf

- "recipe" bundles commands to install an individual component on a system
- Cookbook bundles related recipes
- Berkshelf is like Bundler for recipes

##4.0 Quickstart - 5 Minute Server

###Stack

- Ruby 1.9.3+
- Postgres
- Redis

###Steps

- install tools
- define the server
- "Also copy your SSH public key (cat ∼/.ssh/id_rsa.pub) into the public keys array."
	- as string?
- cookbooks directory in root of project?
- monit host_name, user_name, password?
- Bershelf warning:

		WARNING: Berkshelf could not be loaded
		WARNING: Please add the berkshelf gem to your Gemfile or install it manually with `gem install berkshelf`
    
##5.0 Anatomy of a Chef solo project

###Create a project

- `mkdir my_chef_projects`
- `cd my_chef_projects`
- `rvm gemset create chef`
- `rvm --rvmrc --create ruby-version@chef` replace `ruby-version` with desired version
- `gem install knife-solo berkshelf`

###Terminology

- recipe
- cookbook
- node
- role
- data bag
- chef repository

###Create a repository

- `knife solo init my_first_chef_repo` should see warning, creating, setup
- `cd my_first_chef_repo`
	- rvmrc warning?

		my_first_chef_repo/
			.chef/
				knife.rb
			cookboks/
			data_bags/
			nodes/
			roles/
			site_cookbooks/

- knife.rb: configuration for knife
	- cookbook_path, node_path, role_path, data_bag_path
	- probably leave these alone
- cookbooks/ for other people's cookbooks we install
- site_cookbooks/ for our own custom cookbooks

##5.1 A Simple Chef Cookbook

###Recipe structure

- ####metadata.rb
	- metadata, os, dependencies
- ####default.rb
	- cookbook's default recipe
	- recipe commands
		- bash, package, template
	- templates/default/ refers to distribution, *not* default recipe
- attributes: defined in node definitions, role definitions, cookbook defaults
	- can override defaults specified by cookbook

###Cookbook attribute defaults

- attributes/default.rb

##5.2 A Simple Node Definition

####node: single server

####a redis node
- run_list: roles and recipes; `role[rolename]`, `recipe[recipename]`
	- attributes defined here override those in attributes/default.rb

####Customising Recipes with Attributes
- unclear about this, revisit

##5.3 A simple chef role

####role: like a mixin (reusable chunk of functionality applicable to many roles)

####role definition

- name: for accessing
- description: human readable
- default attributes: provide default values if node doesn't provide any
- json_class and chef_type: disambiguate that this is a role
- run_list: included recipes and roles
	- recipe[recipename] -> recipe[cookbookname::defaultrecipe]
	- recipe[cookbookname::recipename]

##5.4 - Applying a node definition to a VPS

- set up a VPS (Digital Ocean or other)
- `ssh-copy-id root@yourserverip` to copy public key from local machine to VPS
- go to root of cookbook directory
- `knife solo prepare root@yourserverip` to install chef on VPS
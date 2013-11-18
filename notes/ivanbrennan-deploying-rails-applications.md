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

####the 'simple approach'

- create new VPS on Digital Ocean, login via SSH, apt-get packages, tweak config files, add custom package sources where newer versions needed
- build server up in layers over time, keeping track in a text file or wiki
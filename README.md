##Questions

- "copy your SSH public key (cat ∼/.ssh/id_rsa.pub)into the public keys array"	- as string?- monit host_name, user_name, password?
- Bershelf warning:

		WARNING: Berkshelf could not be loaded
		WARNING: Please add the berkshelf gem to your Gemfile or install it manually with `gem install berkshelf`
- rvmrc warning?

		You are using '.rvmrc', it requires trusting, it is slower and it is not compatible with other ruby managers,
		you can switch to '.ruby-version' using 'rvm rvmrc to [.]ruby-version'
		or ignore this warning with 'rvm rvmrc warning ignore /Users/ivan/Development/Flatiron/code/deployment-sandbox/my_chef_projects/.rvmrc',
		'.rvmrc' will continue to be the default project file in RVM 1 and RVM 2,
		to ignore the warning for all files run 'rvm rvmrc warning ignore all.rvmrcs'.

		******************************
		 NOTICE                                                                       
		******************************
		* RVM has encountered a new or modified .rvmrc file in the current directory,  *
		* this is a shell script and therefore may contain any shell commands.         *
		*                                                                              *
		* Examine the contents of this file carefully to be sure the contents are      *
		* safe before trusting it!                                                     *
		* Do you wish to trust                                                         *
		* '/Users/ivan/Development/Flatiron/code/deployment-sandbox/my_chef_projects/. *
		* rvmrc'?                                                                      *
		* Choose v[iew] below to view the contents                                     *
		******************************
		y[es], n[o], v[iew], c[ancel]>


##Anwers

- does `cookbooks/` belong in project root directory?
	- Yes
   
##Links

###Spike's lecture
- [lecture](https://github.com/spikegrobstein/flatironschool-deployment_lecture/blob/master/lecture.md)

###unicorn
- [Github's config](https://github.com/blog/517-unicorn)
- [Passenger v Unicorn](https://blog.engineyard.com/2012/passenger-vs-unicorn)

###rails server template
- [rails server template](https://github.com/TalkingQuickly/rails-server-template)

###chef solo
- [knife solo](https://github.com/matschaffer/knife-solo)
- [knife options (may not need)](http://docs.opscode.com/config_rb_knife.html)
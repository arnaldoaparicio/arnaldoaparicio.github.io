---
layout: page
permalink: /github-actions-p1/
---

 Introduction

<p>Instructions</p>

### Step 1: 'rails new'

<p>To start from scratch, generate a new rails app on your local machine. This is the configuration I am using for this walkthrough, making sure to set `postgres` as the database, to make the github actions more real-world and realistic.</p>

<p>

{% highlight console %}
$ rails _5.2.8.1_ new github_rails_ci -T -d="postgresql" --skip-spring --skip-turbolinks
# there will be lots of terminal output after running this command, but when it finishes:

$ cd github_actions_walkthrough
$ git add .
$ git commit -m "initial commit"
{% endhighlight %}
</p>

### Step 2: Ensure local and remote repositories are connected


<p>If you already have an existing repository on github, skip this step. Otherwise we will create a new repo.</p>


- Create new repository attached to your github account at https://github.com/new
- Add repository name. Ensure that it matches the name of your rails app (for example, I will use `github_actions_walkthrough`)
- Run github's prescribed terminal commands. 
- Refresh browser, make sure you can see your initial commit

<p>
{% highlight console %}
# after creating repository on https://github.com/new:

$ git remote add origin git@github.com:arnaldoaparicio/github_actions_walkthrough.git
$ git branch -M main
$ git status
$ git push -u origin main

# refresh browser to verify results
{% endhighlight %}
</p>

### Step 3.0 : Install `rspec-rails`


<p>For the purposes of this guide, you can auto-generate tests, but if you're trying to set up github actions on an existing rails project, you _likely_ already have tests, so skip this step.</p>

<p>Add the `rspec-rails` gem to your `:development, :test` group in `Gemfile`</p>

{% highlight console %}
$ bundle install
$ rails generate rspec:install
{% endhighlight %}

### Step 3.1 : Use 'rails generate scaffold' to generate tests.


{% highlight console %}
rails generate scaffold Widget name:string
{% endhighlight %}

<p>It could be helpful to add and commit work so far, but that's up to you.</p>


### Step 4: Create .yml file that will be read by github actions

<p>To trigger github actions, github will expect a yml file inside of `.github/workflows/`</p>

{% highlight console %}
code .github/workflows/run_tests.yml
{% endhighlight %}


### Step 5: Fill new yml file with instructions for github actions:

{% highlight yaml %}
# .github/workflows/run_spec.yml
name: CI 
on: [push, pull_request] 
jobs:
  test:
    runs-on: ubuntu-latest
    services: 
      db:
        image: postgres:14
        ports: ['5432:5432']
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    steps:
    - uses: actions/checkout@v3
    - name: Setup Ruby
      uses: ruby/setup-ruby@v1
      with:
        ruby-version: '2.7.6'
        bundler-cache: true
    - uses: borales/actions-yarn@v2.0.0
      with:
        cmd: install
    - name: Build and run tests
      env:
        PG_DATABASE: postgres
        PG_HOST: localhost
        PG_USER: postgres
        PG_PASSWORD: postgres
        RAILS_ENV: test
        
      run: |
        sudo apt-get -yqq install libpq-dev
        gem install bundler
        bundle install --jobs 4 --retry 3
        bundle exec rails db:setup
        bundle exec rspec spec
{% endhighlight %}

### Step 6: Make sure your tests work locally

{% highlight console %}
$ rails db:setup
$ rails db:migrate
$ rspec
{% endhighlight %}

### Step 7: Commit it and see if your tests work

<p>Push your new `yml` file to Github, and check the `actions` tab on the repository to watch the action get run. For me, I'd head to http://github.com/arnaldoaparicio/github_actions_walkthrough/actions/</p>

### Step 8: Realize your tests don't work because `config/database.yml` needs to be updated

<p>The values in the `test` development block in `config/database.yml` need to include postgres user information to be used during the github Actions run.</p>

<p>The values for `host`, `username`, and `password` should match what you have in `run_spec.yml`</p>

{% highlight yaml %}
# .github/workflows/run_spec.yml:28
 env:
    PG_DATABASE: postgres
    PG_HOST: localhost
    PG_USER: postgres
    PG_PASSWORD: postgres
    RAILS_ENV: test
{% endhighlight %}

{% highlight diff %}
diff --git a/config/database.yml b/config/database.yml
index 7555321..908e314 100644
--- a/config/database.yml
+++ b/config/database.yml
@@ -58,6 +58,10 @@ development:
 test:
   <<: *default
   database: github_actions_walkthrough_test
+  host: localhost
+  username: postgres
+  password: postgres
{% endhighlight %}

<p>Push this all to Github, check the `actions` tab output, and you should see a green check, and if you drill into the `build and run tests` dropdown/collapsible menu item, you'll see some output like:</p>

{% highlight console %}
  13) /widgets DELETE /destroy destroys the requested widget
     # Add a hash of attributes valid for your model
     # ./spec/requests/widgets_spec.rb:118

  14) /widgets DELETE /destroy redirects to the widgets list
     # Add a hash of attributes valid for your model
     # ./spec/requests/widgets_spec.rb:125

Finished in 0.58442 seconds (files took 0.72949 seconds to load)
27 examples, 0 failures, 14 pending
{% endhighlight %}

<p>Congrats!</p>

## Conclusion

<p>Now, having read this guide, you know how to go from `rails new` in your terminal, to a successful github actions "run" than checks that all of your tests are passing.</p>

<p>Sally forth and prosper.</p>

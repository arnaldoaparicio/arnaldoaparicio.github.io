---
layout: post
title: "GitHub Actions Walkthrough: Part 1"
permalink: /blog/github-actions-p1/
---

### Introduction

A few weeks ago, for a [different project](https://github.com/arnaldoaparicio/mugen_db_fe) which used a separate front-end and back-end, I wanted to use Github Actions to auto-run my test suite, whenever I pushed a commit.

I was having issues getting the databases to talk to each other during the test run, but since I'd not used Github Actions much before, or any sort of ci/cd tooling, I wanted to get a 'minimally working' version of a Rails app on Github actions. 

The steps below are _very_ detailed, but represents a fair bit of the digging that I had to do to figure it all out. I've written the notes out here for myself and others, if it's helpful, and I'll do a part two soon, to explain how I got Github Actions working on an app that is 'stateless', and detatched from the backend.


### Step 1: 'rails new'

To start from scratch, generate a new rails app on your local machine. This is the configuration I am using for this walkthrough, making sure to set `postgres` as the database, to make the GitHub Actions more real-world and realistic.

{% highlight console %}
$ rails _5.2.8.1_ new github_rails_ci -T -d="postgresql" --skip-spring --skip-turbolinks
# there will be lots of terminal output after running this command, but when it finishes:

$ cd github_actions_walkthrough
$ git add .
$ git commit -m "initial commit"
{% endhighlight %}

### Step 2: Ensure local and remote repositories are connected

If you already have an existing repository on github, skip this step. Otherwise we will create a new repo.

- Create new repository attached to your github account at [https://github.com/new](https://github.com/new)
- Add repository name. Ensure that it matches the name of your rails app (for example, I will use `github_actions_walkthrough`)
- Run github's prescribed terminal commands. 
- Refresh browser, make sure you can see your initial commit

{% highlight console %}
# after creating repository on https://github.com/new:

$ git remote add origin git@github.com:arnaldoaparicio/github_actions_walkthrough.git
$ git branch -M main
$ git status
$ git push -u origin main

# refresh browser to verify results
{% endhighlight %}

### Step 3.0 : Install `rspec-rails`


For the purposes of this guide, you can auto-generate tests, but if you're trying to set up github actions on an existing rails project, you _likely_ already have tests, so skip this step.

Add the `rspec-rails` gem to your `:development, :test` group in `Gemfile`

{% highlight console %}
$ bundle install
$ rails generate rspec:install
{% endhighlight %}

### Step 3.1 : Use 'rails generate scaffold' to generate tests.

{% highlight console %}
rails generate scaffold Widget name:string
{% endhighlight %}

It could be helpful to add and commit work so far, but that's up to you.

### Step 4: Create .yml file that will be read by github actions

To trigger github actions, github will expect a yml file inside of `.github/workflows/`

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

Push your new `yml` file to GitHub, and check the `actions` tab on the repository to watch the action get run. For me, I'd head to [http://github.com/arnaldoaparicio/github_actions_walkthrough/actions/](http://github.com/arnaldoaparicio/github_actions_walkthrough/actions/)

### Step 8: Realize your tests don't work because `config/database.yml` needs to be updated

The values in the `test` development block in `config/database.yml` need to include postgres user information to be used during the GitHub Actions run.

The values for `host`, `username`, and `password` should match what you have in `run_spec.yml`

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

Push this all to GitHub, check the `actions` tab output, and you should see a green check, and if you drill into the `build and run tests` dropdown/collapsible menu item, you'll see some output like:

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

Congrats!

### Conclusion

Now, having read this guide, you know how to go from `rails new` in your terminal, to a successful github actions "run" than checks that all of your tests are passing.

Sally forth and prosper.

### Additional links

Here's some of the resources and articles I was consulting and checking with as I worked through this GitHub Actions learning:

- [Building a Rails CI Pipline with GitHub Actions (boringrails.com)](https://boringrails.com/articles/building-a-rails-ci-pipeline-with-github-actions/)
- [How to set up a Ruby on Rails CI workflow using GitHub Actions (dev.to)](https://dev.to/buildwithallan/how-to-set-up-a-ci-workflow-using-github-actions-4818)
- [How to run Ruby on Rails tests on GitHub Actions using RSpec (docs.knapsackpro.com)](https://docs.knapsackpro.com/2021/how-to-run-ruby-on-rails-tests-on-github-actions-using-rspec)

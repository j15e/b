---
layout: post
title:  "What is a Gemfile versus a Gemspec"
date:   2013-06-28 22:00:03
categories: ruby
---

*Foreword : when I started working with ruby a few years ago, it took me quite some time to
figure out the whole stack and every subtil detail of it. I will write a few
articles about the ruby toolbox and my current understanding of it, stuff I would
have love to have been told long ago.*

In plain english, here is what they do and why you should use them. I think the
best way to start explaining this, is to know where they come from and what
is their respective purpose.

**[Gemspec][gemspec-spec]** was created in 2006 by Rubygems and is the way to
define a ruby package (a gem), publish it on rubygems.org and install them with
the `gem` command.

When you define a gem, you set a name, a version, an author, etc. and you also specify
which gems your gem depends on, and this is where things might get confusing at first.

**[Gemfile][gemfile-spec]** was created in 2009 by Bundler (Engine Yard, Andre Arko) as a
way to define a project gems dependency, deal with dependencies version and lock versions
to use the same environment across developpers & deployments.

When using a Gemfile, bundler is resolving each gem's gemspec and gather them all to
find out what conflicts might occurs and what which most up to date version of a gem
is compatible among all your dependencies.

**When you are doing a ruby project** which isn't a gem, you should only use a `Gemfile`.
It deals with dependencies versions, it locks versions in a file because you need to
share same gems versions among developers & deployments.

**When you are building a gem**, your sure need a gemspec. And, optionally, if you would like
to either keep a track of dependencies that aren't part of the real gem dependencies
(ex. tests framework, debugging tools) you may create a Gemfile. In either case, you can
tell bundler in the `Gemfile` to read the gemspec :

{% highlight ruby %}
# Gemfile
source 'https://rubygems.org'

# Gems for gem development
group :debug do
  gem 'debugger', :platform => 'mri'
  gem 'awesome_print'
  gem 'gem-release'
end

group :test do
  gem 'contest'
  gem 'rake'
  gem 'mocha'
end

# Bundler will get other dependencies from the gemspec.
gemspec
{% endhighlight %}

Some pure rubyist might told your is wrong and that you should never include a `Gemfile`
in a ruby gem since bundler is a system for deployments and has nothing to do with a gem.

I agree to disagree.

[gemspec-spec]: http://docs.rubygems.org/read/chapter/20
[gemspec-exemple]: http://guides.rubygems.org/specification-reference/
[gemfile-spec]: http://gembundler.com/v1.3/man/gemfile.5.html
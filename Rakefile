require 'bundler/setup'
require 'shellwords'
require 'yaml'
require 'tmpdir'

Bundler.require

desc 'Generate blog files'
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
    'source'      => '.',
    'destination' => '_site'
  })).process
end


desc 'Generate and publish blog to gh-pages'
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    cp_r '_site/.', tmp
    Dir.chdir tmp
    system 'git init'
    system 'git add .'
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m #{message.shellescape}"
    system 'git remote add origin git@github.com:j15e/j15e.github.io.git'
    system 'git push origin gh-pages --force'
  end
end
require 'rake/clean'

CLEAN << '_site'
CLEAN << '.sass_cache'

desc 'build jekyll site'
task :build do |t|
	sh 'jekyll build'
end

desc 'serve up jekyll site'
task :serve do |t|
	sh 'jekyll serve'
end

desc 'serve up jekyll site and watch for source changes'
task :watch do |t|
	sh 'jekyll serve --drafts --watch'
end

task :default => :build


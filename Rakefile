require 'rake/clean'

CLEAN << '_site'
CLEAN << '.sass_cache'


task :build do |t|
	sh 'jekyll build'
end

task :serve do |t|
	sh 'jekyll serve'
end

task :watch do |t|
	sh 'jekyll serve --drafts --watch'
end

task :default => :build


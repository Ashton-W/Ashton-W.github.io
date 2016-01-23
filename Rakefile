require 'yaml'
require 'rake/clean'

CLEAN << '_site'
CLEAN << '.sass_cache'


# Set "rake watch" as default task
task :default => :watch

# Load the configuration file
CONFIG = YAML.load_file("_config.yml")

# Get and parse the date
DATE = Time.now.strftime("%Y-%m-%d")
TIME = Time.now.strftime("%H:%M:%S")
POST_TIME = DATE + ' ' + TIME

# Directories
POSTS = "_posts"
DRAFTS = "_drafts"

def check_title(title)
  if title.nil? or title.empty?
    raise "Please add a title to your file."
  end
end

def transform_to_slug(title, extension)
  characters = /("|'|!|\?|:|\s\z)/
  whitespace = /\s/
  "#{title.gsub(characters,"").gsub(whitespace,"-").downcase}.#{extension}"
end

def read_file(template)
  File.read(template)
end

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

desc 'Create a post in _drafts'
task :draft, :title do |t, args|
  title = args[:title]
  template = CONFIG["post"]["template"]
  extension = CONFIG["post"]["extension"]
  editor = CONFIG["editor"]
  check_title(title)
  filename = transform_to_slug(title, extension)
  content = read_file(template)
  create_file(DRAFTS, filename, content, title, editor)
end

desc 'Move a post from _drafts to _posts'
task :publish do |t|
  extension = CONFIG["post"]["extension"]
  files = Dir["#{DRAFTS}/*.#{extension}"]
  files.each_with_index do |file, index|
    puts "#{index + 1}: #{file}".sub("#{DRAFTS}/", "")
  end
  print "> "
  number = $stdin.gets
  if number =~ /\D/
    filename = files[number.to_i - 1].sub("#{DRAFTS}/", "")
    FileUtils.mv("#{DRAFTS}/#{filename}", "#{POSTS}/#{DATE}-#{filename}")
    puts "#{filename} was moved to '#{POSTS}'."
  else
    puts "Please choose a draft by the assigned number."
  end
end

task :default => :watch




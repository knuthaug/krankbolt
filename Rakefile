require "rubygems"
require 'rake'
require 'yaml'
require 'time'
require 'rake/minify'

SOURCE = "."
CONFIG = {
  'version' => "0.2.13",
  'themes' => File.join(SOURCE, "_includes"),
  'layouts' => File.join(SOURCE, "_layouts"),
  'posts' => File.join(SOURCE, "_posts"),
  'post_ext' => "markdown",
  'theme_package_version' => "0.1.0"
}


# Usage: rake post title="A Title" [date="2012-02-09"]
desc "Begin a new post in #{CONFIG['posts']}"
task :post do
  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
  title = ENV["title"] || "new-post"
  slug = title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
  rescue Exception => e
    puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
    exit -1
  end
  filename = File.join(CONFIG['posts'], "#{date}-#{slug}.#{CONFIG['post_ext']}")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end

  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title.gsub(/-/,' ')}\""
    post.puts "date: #{date}"
    post.puts "published: false"
    post.puts "tags: "
    post.puts "categories: "
    post.puts "---"
  end
end # task :post

task :deploy do
  exec("rsync -e ssh -a _site/ knuthaugen@login.domeneshop.no:www/krankbolt/")
end

#Load custom rake scripts
Dir['_rake/*.rake'].each { |r| load r }

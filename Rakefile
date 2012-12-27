# Requirements
require 'yaml'       # For reading the configuration file
require 'fileutils'  # For creating recursive directories

# Load the configuration file
config = YAML.load_file("_config.yml")

# Set build as default
task :default => :build

# rake build
# rake build[number]
desc "Generate the site (with an optional post limit)"
task :build, :number do |t, args|
  number = args[:number]
  if number.nil?
    system "jekyll --auto --server"
  else
    system "jekyll --auto --server --limit_posts=#{number}"
  end
end

# rake post["Post title"]
desc "Create a post in the _posts directory"
task :post, :title do |t, args|
  title = args[:title]
  template = config["post"]["template"]
  extension = config["post"]["extension"]
  date = Time.now.strftime("%Y-%m-%d")
  if title.nil?
    raise "Please add a title to your post."
  end
  filename = "#{date}-#{title.gsub(/[^[:alnum:]]+/, "-").downcase}.#{extension}"
  content = File.read(template)
  File.open("_posts/#{filename}","w") { |file|
    file.puts("#{content.gsub("title:", "title: #{title}")}") }
  puts "#{filename} was created."
end

# rake page["Page title"]
# rake page["Page title","Path/to/folder"]
desc "Create a page in a specific directory."
task :page, :title, :path do |t, args|
  title = args[:title]
  path = args[:path]
  template = config["page"]["template"]
  extension = config["page"]["extension"]
  if title.empty?
    raise "Please add a title to your page."
  end
  if path.nil?
    path = "./"
  else
    FileUtils.mkdir_p("#{path}") unless File.exists?("#{path}")
  end
  filename = "#{title.gsub(/[^[:alnum:]]+/, "-").downcase}.#{extension}"
  content = File.read(template)
  File.open("#{path}/#{filename}","w") { |file|
    file.puts("#{content.gsub("title:", "title: #{title}")}") }
end

# rake git["Commit message"]
desc "Deploy the site to it's remote git repository"
task :git, :message do |t, args|
  message = args[:message]
  if message.nil?
    raise "Please add a commit message."
  end
  system "git add ."
  system "git commit -m \"#{message}\""
  system "git push origin master"
  puts "Your site was deployed."
end

# rake remote
desc "Deploy the site to a remote server"
task :remote do
  command = config["remote"]["command"]
  source = config["remote"]["source"]
  destination = config["remote"]["destination"]
  settings =config["remote"]["settings"]
  if command.nil?
    raise "Please choose a file transfer command."
  elsif command == "robocopy"
    system "robocopy #{source} #{destination} #{settings}"
    puts "Your site was deployed."
  elsif command == "rsync"
    system "rsync #{settings} #{source} #{destination}"
    puts "Your site was deployed."
  else
    raise "#{command} isn't a valid file transfer command."
  end
end
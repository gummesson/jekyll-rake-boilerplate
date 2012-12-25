# For the configuration
require 'yaml'

# Load the configuration file
config = YAML.load_file("_config.yml")

# Set build as default
task :default => :build

# rake build or rake build[number]
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
  else
    filename = "#{date}-#{title.gsub(/\s/, "-").downcase}.#{extension}"
    content = File.read(template)
    File.open("_posts/#{filename}","w") { |file|
      file.puts("#{content.gsub("title:", "title: #{title}")}") }
    puts "#{filename} was created."
  end
end

# rake git["Commit message"]
desc "Deploy the site to it's remote git repository"
task :git, :message do |t, args|
  message = args[:message]
  if message.nil?
    raise "Please add a commit message."
  else
    system "git add ."
    system "git commit -m \"#{message}\""
    system "git push origin master"
    puts "Your site was deployed."
  end
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
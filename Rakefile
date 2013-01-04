# Requirements 
require 'rake'       # For the rake tasks
require 'yaml'       # For reading the configuration file
require 'fileutils'  # For creating recursive directories

# Load the configuration file
config = YAML.load_file("_config.yml")

# Set "rake watch" as default task
task :default => :watch

# rake post["Post title"]
desc "Create a post in the _posts directory"
task :post, :title do |t, args|
  title = args[:title]
  template = config["post"]["template"]
  extension = config["post"]["extension"]
  editor = config["editor"]

  if title.nil? or title.empty?
    raise "Please add a title to your post."
  end

  date = Time.now.strftime("%Y-%m-%d")
  filename = "#{date}-#{title.gsub(/[^[:alnum:]]+/, "-").downcase}.#{extension}"
  content = File.read(template)
 
  if File.exists?("_posts/#{filename}")
    raise "The post already exists."
  else
    File.open("_posts/#{filename}","w") { |file|
      file.puts("#{content.gsub("title:", "title: \"#{title}\"")}") }
    puts "#{filename} was created."
    
    if editor && !editor.nil?
      sleep 2 # seconds
      system "#{editor} _posts/#{filename}"
    end
  end
end

# rake page["Page title"]
# rake page["Page title","Path/to/folder"]
desc "Create a page (with an optional filepath)"
task :page, :title, :path do |t, args|
  title = args[:title]
  filepath = args[:path]
  template = config["page"]["template"]
  extension = config["page"]["extension"]
  editor = config["editor"]
  
  if title.nil? or title.empty?
    raise "Please add a title to your page."
  end
  
  if filepath.nil? or filepath.empty?
    filepath = "./"
  else
    FileUtils.mkdir_p("#{filepath}")
  end

  filename = "#{title.gsub(/[^[:alnum:]]+/, "-").downcase}.#{extension}"
  content = File.read(template)
 
  if File.exists?("#{filepath}/#{filename}")
    raise "The page aldready exists."
  else
    File.open("#{filepath}/#{filename}","w") { |file|
      file.puts("#{content.gsub("title:", "title: \"#{title}\"")}") }
    puts "#{filename} was created in #{filepath}."
    
    if editor && !editor.nil?
      sleep 2 # seconds
      system "#{editor} #{filepath}/#{filename}"
    end
  end
end

# rake build
desc "Generate the site (no server)"
task :build do
  system "jekyll --no-server"
end

# rake watch
# rake watch[number]
desc "Generate and watch the site (with an optional post limit)"
task :watch, :number do |t, args|
  number = args[:number]
  
  if number.nil? or number.empty?
    system "jekyll --auto --server"
  else
    system "jekyll --auto --server --limit_posts=#{number}"
  end
end

# rake preview
desc "Launch a preview of the site in the browser"
task :preview do
  require 'Launchy'  # For launching the browser
  
  puts "Launching browser for preview..."
  sleep 2 #seconds

  Launchy.open("http://localhost:4000")
  Rake::Task[:watch].invoke
end

# rake deploy["Commit message"]
desc "Deploy the site to it's remote git repository"
task :deploy, :message do |t, args|
  message = args[:message]

  if message.nil? or message.empty?
    raise "Please add a commit message."
  end

  Rake::Task[:build].invoke 
  system "git add ."
  system "git commit -m \"#{message}\""
  system "git push origin master"
end

# rake transfer
desc "Transfer the site to a remote server or a local git repository"
task :transfer do
  command = config["transfer"]["command"]
  source = config["transfer"]["source"]
  destination = config["transfer"]["destination"]
  settings =config["transfer"]["settings"]

  if command.nil? or command.empty?
    raise "Please choose a file transfer command."

  elsif command == "robocopy"
    Rake::Task[:build].invoke
    system "robocopy #{source} #{destination} #{settings}"
    puts "Your site was transfered."

  elsif command == "rsync"
    Rake::Task[:build].invoke
    system "rsync #{settings} #{source} #{destination}"
    puts "Your site was transfered."

  else
    raise "#{command} isn't a valid file transfer command."
  end
end

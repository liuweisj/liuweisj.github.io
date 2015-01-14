desc "Begin a new post in #{CONFIG['posts']}"
task :post do
  abort("rake aborted: '#{CONFIG['posts']}' directory not found.") unless FileTest.directory?(CONFIG['posts'])
  title = ENV["title"] || "new－post"
  # 新增用来接收category和description参数
  category = ENV["category"] || "default"
  description = ENV["description"] || ""
  tags = ENV["tags"] || "[]"
  ＃ 新增用来将汉字转换成拼音，因为url好像不支持中文。当然在文件顶部require了Hz2py
  slug = Hz2py.do(title, :join_with => '-', :to_simplified => true)
  slug = slug.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '')
  begin
    date = (ENV['date'] ? Time.parse(ENV['date']) : Time.now).strftime('%Y-%m-%d')
  rescue Exception => e
    puts "Error - date format must be YYYY-MM-DD, please check you typed it correctly!"
    exit -1
  end
  # 新增，首先判断分类目录是否存在，不存在则创建
  filename = File.join(CONFIG['posts'], category)
  if !File.directory?(filename)
    mkdir_p filename
  end
  filename = File.join(filename, "#{date}-#{slug}.#{CONFIG['post_ext']}")
  if File.exist?(filename)
    abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y', 'n']) == 'n'
  end
  # 新增用户提示，在创建博客之前最后再检查一次是否按照自己的需求正确创建
  # User confirm 
  abort("rake aborted!") if ask("The post #{filename} will be created in category #{category}, are you sure?", ['y', 'n']) == 'n'
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts "---"
    post.puts "layout: post"
    post.puts "title: \"#{title}\""
    post.puts "description: \"#{description}\""
    post.puts "category: \"#{category}\""
    post.puts "tags: \"#{tags}\""
    post.puts "---"
    post.puts "Included file 'JB/setup' not found in _includes directory"
  end
end # task :post

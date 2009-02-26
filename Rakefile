require 'rake'

desc "install symlinks to these files into the xcode extension folder"
task :install do
  replace_all = false
  
  destination_dir = File.expand_path("~/Library/Application Support/Developer/Shared/Xcode")
  
  Dir['**/*'].each do |source|
    next if %w[Rakefile README].include? source
    next if File.directory?(source)

    dest_path = File.join(destination_dir, source)
    FileUtils::mkdir_p File.dirname(dest_path)
    
    if File.exist?(dest_path) || File.symlink?(dest_path)
      if replace_all
        replace_file(source, dest_path)
      else
        print "overwrite #{dest_path}? [ynaq] "
        case $stdin.gets.chomp
        when 'a'
          replace_all = true
          replace_file(source, dest_path)
        when 'y'
          replace_file(source, dest_path)
        when 'q'
          exit
        else
          puts "skipping #{dest_path}"
        end
      end
    else
      link_file(source, dest_path)
    end
  end
end
 
def replace_file(source, destination)
  system %Q{rm "#{destination}"}
  link_file(source, destination)
end
 
def link_file(source, destination)
  puts "linking #{source} to #{destination}"
  system %Q{ln -s "$PWD/#{source}" "#{destination}"}
end
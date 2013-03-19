require 'rubygems'
require 'dotenv'
require 'dotenv/tasks'

Dotenv.load

task :default => :usage

desc 'Show usage'
task :usage do
  puts "Run 'rake -T' to see all tasks"
end

desc 'Show credentials'
task :creds => :dotenv do
  puts ENV['server'], ENV['user'], ENV['password']
end

desc "Remove the black border around warped raster images\n
usage: rake remove_border[/path/to/image(s)]"
task :remove_border, :arg1 do |task, args|
  glob_pattern = args[:arg1]

  Dir[glob_pattern].each do |file|
    base = File.basename(file, '.tif')
    f = File.expand_path(file)
    directory = File.dirname(f)

    base_file = "#{directory}/#{base}"
    warp_file = "#{base_file}_warped.tif"
    translate_file =  "#{base_file}_translated.tif"
    final_file = "#{base_file}_transformed.tif"
    
    command = "gdalwarp -srcnodata 0 -dstalpha #{file} #{warp_file}\n"
    command += "gdal_translate -of GTiff -a_srs EPSG:4326 #{warp_file} #{translate_file}\n"
    #command += "gdalwarp -rc -s_srs 'EPSG:4326' -t_srs 'EPSG:900913'  #{translate_file} #{final_file}\n"
    puts command
  end
end

desc "Uploads the files to the server"
task :web_services, :arg1, :arg2 do |task, args|
  glob_pattern = args[:arg1]
  workspace = args[:arg2]

  Dir[glob_pattern].each do |file|
    coverage = File.basename(file, 'tif.zip')
    server_path = "#{ENV['server']}/workspaces/#{workspace}/coveragestores/#{coverage}/file.geotiff"
    command = "curl -u #{ENV['user']}:#{ENV['password']} -XPUT -H 'Content-type: application/zip --data-binary @#{file} #{server_path}"
    puts command
  end
end

def make_dir(path)
  FileUtils.mkdir_p path
end

def zip_files(pattern)
  Dir[pattern].each do |file|
    #base = File.basename(file, '.tif')
    directory = File.dirname(File.expand_path(file))
    command = "zip #{directory}/#{file}.zip #{file}"
    puts command
  end
end

def cleanup(files)
  
end


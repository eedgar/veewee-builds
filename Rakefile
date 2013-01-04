@bb = "vagrant basebox"
@baseboxes = Dir.glob("definitions/*").map { |a| File.basename(a) }

desc "list known baseboxes"
task :baseboxes do 
    puts @baseboxes
end

namespace :box do
    desc "add all boxes to vagrant"
    task :add => :build do
        @baseboxes.each do |box|
            sh %{vagrant box add #{box} #{box}.box} do | ok, res |
                if ! ok
                    puts "already installed (status = #{res.exitstatus})"
                end
            end
        end
    end

    desc "remove all boxes from vagrant"
    task :rm do
        @baseboxes.each do |box|
            sh "vagrant box remove #{box}"
        end
    end
    
    namespace :add do
        @baseboxes.each do |box|
            desc "add basebox '#{box}' to vagrant"
            task box.to_s => "#{box}.box" do
                sh %{vagrant box add #{box} #{box}.box} do | ok, res |
                    if ! ok
                        puts "#{box} is already installed (status = #{res.exitstatus})"
                    end
                end
            end
        end  
    end  

    namespace :rm do
        @baseboxes.each do |box|
            desc "rm basebox '#{box}' from vagrant"
            task box.to_s => "#{box}.box" do
                sh %{vagrant box remove #{box}} do | ok, res |
                end
            end
        end  
    end  

end

namespace :build do
    @baseboxes.each do |box|
        file "#{box}.box" do
            sh "#{@bb} build #{box} --force"
            sh "#{@bb} validate #{box}"
            sh "#{@bb} export #{box}"
        end

        desc "build basebox '#{box}'"
        task box.to_s => "#{box}.box"
    end
end

desc "build all baseboxes"
task :build => @baseboxes.map{|box| "#{box}.box"}

desc "clean up build environment"
task :clean do
    @baseboxes.each do |box|
        sh "#{@bb} destroy #{box}"
    end
end

namespace :clean do
    desc 'clean up *.box files'
    task :all => :clean do
        @baseboxes.each do |box|
            File.exists?("#{box}.box") && File.delete("#{box}.box")
        end
    end

    @baseboxes.each do |box|
        desc "clean basebox '#{box}'"
        task box.to_s => :clean do
           File.exists?("#{box}.box") && File.delete("#{box}.box")
        end
    end
end

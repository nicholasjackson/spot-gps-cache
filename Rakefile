require 'docker'
require_relative 'rake-modules/docker'

GOPATH = Dir.pwd + "/go"
GOCMD = "/usr/local/go/bin/go"
CONFIG = Dir.pwd + "/config.json"
ROOTFOLDER = Dir.pwd

task :test  do
	p "Test Application"
	container = get_container

	begin
		# Get go packages
		ret = container.exec(['go','get','-t','./...']) { |stream, chunk| puts "#{stream}: #{chunk}" }
		raise Exception, 'Error running command' unless ret[2] == 0

		# Test application
		ret = container.exec(['go','test','./...']) { |stream, chunk| puts "#{stream}: #{chunk}" }
		raise Exception, 'Error running command' unless ret[2] == 0
	ensure
		container.delete(:force => true)
	end
end

task :build_go_build_server do
	p "Building Docker Image:- Dev Server"

	Docker.options = {:read_timeout => 3600}
	image = Docker::Image.build_from_dir './dockerfile/gobuildserver', {:t => 'gobuildserver'}
end

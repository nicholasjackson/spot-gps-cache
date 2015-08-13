require 'docker'
require_relative 'rake-modules/docker'

GOPATH = Dir.pwd + "/go"
GOCMD = "/usr/local/go/bin/go"
CONFIG = Dir.pwd + "/config.json"
ROOTFOLDER = Dir.pwd

task :build => [:test] do
	p "Build for Linux"
	container = get_container

	begin
		# Build go server
		ret = container.exec(['go','build','-o','server']) { |stream, chunk| puts "#{stream}: #{chunk}" }
		raise Exception, 'Error running command' unless ret[2] == 0
	ensure
		container.delete(:force => true)
	end
end

task :test  do
	p "Test Application"
	container = get_container

	begin
		# Get go packages
		ret = container.exec(['go','get','-t','-v','./...']) { |stream, chunk| puts "#{stream}: #{chunk}" }
		raise Exception, 'Error running command' unless ret[2] == 0

		# Test application
		ret = container.exec(['go','test','./...']) { |stream, chunk| puts "#{stream}: #{chunk}" }
		raise Exception, 'Error running command' unless ret[2] == 0
	ensure
		container.delete(:force => true)
	end
end

task :build_server => [:build] do
	p "Building Docker Image:- API Users Server"

	FileUtils.cp "./go/src/github.com/nicholasjackson/spot-gps-cache/server", "./dockerfile/spot-gps-cache/server"

	Docker.options = {:read_timeout => 6200}
	image = Docker::Image.build_from_dir './dockerfile/spot-gps-cache', {:t => 'spot-gps-cache'}
end

task :run => [:build_server] do
	begin
		sh 'docker-compose -f ./dockercompose/api-users/docker-compose.yml up'
	rescue SystemExit, Interrupt
		sh 'docker-compose -f ./dockercompose/api-users/docker-compose.yml stop'
		# remove stopped containers
		sh 'docker-compose -f ./dockercompose/api-users/docker-compose.yml rm'
	end
end

task :build_go_build_server do
	p "Building Docker Image:- Dev Server"

	Docker.options = {:read_timeout => 6200}
	image = Docker::Image.build_from_dir './dockerfile/gobuildserver', {:t => 'gobuildserver'}
end

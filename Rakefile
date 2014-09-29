require 'bundler'
require 'rspec/core/rake_task'


#### FIXME UGLY ####

# Serve a directory over HTTP with Ruby
def ruby_web_server(dir, port)
  "ruby -run -e httpd #{dir} -p #{port} 2>/dev/null"
end

DOCKER_IMAGE = "evolvingweb/sitediff"
task :docker_test do |t|
  cmd = [
    ruby_web_server('spec/fixtures/before', 8881),
    ruby_web_server('spec/fixtures/after', 8882),
    'sitediff diff --before-url=http://localhost:8881 --after-url=http://localhost:8882 spec/fixtures/config.yaml',
  ].join(' & ')
  sh 'docker', 'run', '-v', "#{Dir.pwd}:/sitediff", DOCKER_IMAGE,
    'sh', '-c', cmd
end
task :docker_build do |t|
  sh 'docker', 'build', '-t', DOCKER_IMAGE, '.'
end



#### FIXME OLD ####

# Bundler::GemHelper.install_tasks

RSpec::Core::RakeTask.new(:spec)

desc "Run unit tests"
RSpec::Core::RakeTask.new('spec:unit') { |t| t.pattern = "./spec/unit/**/*_spec.rb" }

desc 'Default task which runs all specs'
task :default => 'spec:unit'

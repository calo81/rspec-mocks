require 'bundler'
Bundler.setup
Bundler::GemHelper.install_tasks

require 'rake'
require 'rspec/core/rake_task'
require 'rspec/mocks/version'
require 'cucumber/rake/task'

desc "Run all examples"
RSpec::Core::RakeTask.new(:spec) do |t|
  t.ruby_opts = %w[-w]
  t.rspec_opts = %w[--color]
end

Cucumber::Rake::Task.new(:cucumber)

task :clobber do
  rm_rf 'pkg'
  rm_rf 'tmp'
  rm_rf 'coverage'
  rm_rf '.yardoc'
  rm_rf 'doc'
end

namespace :clobber do
  desc "remove generated rbc files"
  task :rbc do
    Dir['**/*.rbc'].each {|f| File.delete(f)}
  end
end

desc "Push docs/cukes to relishapp using the relish-client-gem"
task :relish, :version do |t, args|
  raise "rake relish[VERSION]" unless args[:version]
  sh "cp Changelog.md features/"
  if `relish versions rspec/rspec-mocks`.split.map(&:strip).include? args[:version]
    puts "Version #{args[:version]} already exists"
  else
    sh "relish versions:add rspec/rspec-mocks:#{args[:version]}"
  end
  sh "relish push rspec/rspec-mocks:#{args[:version]}"
  sh "rm features/Changelog.md"
end

task :default => [:spec, :cucumber]

task :verify_private_key_present do
  private_key = File.expand_path('~/.gem/rspec-gem-private_key.pem')
  unless File.exists?(private_key)
    raise "Your private key is not present. This gem should not be built without that."
  end
end

task :build => :verify_private_key_present


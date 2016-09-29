require 'racatt'
require 'coveralls/rake/task'


namespace 'cuke_cataloger' do

  task :clear_coverage do
    # Remove previous coverage results so that they don't get merged into the new results
    code_coverage_directory = File.join(File.dirname(__FILE__), 'coverage')
    FileUtils.remove_dir(code_coverage_directory, true) if File.exists?(code_coverage_directory)
  end


  Racatt.create_tasks

  # Redefining the task from 'racatt' in order to clear the code coverage results
  task :test_everything => :clear_coverage


  desc 'Test the project'
  task :smart_test do |t, args|
    rspec_args = '--tag ~@wip --pattern testing/rspec/spec/**/*_spec.rb'
    cucumber_args = 'testing/cucumber/features -r testing/cucumber/support -r testing/cucumber/step_definitions -f progress -t ~@wip'

    Rake::Task['cuke_cataloger:test_everything'].invoke(rspec_args, cucumber_args)
  end


  # The task that CI will use
  Coveralls::RakeTask.new
  task :ci_build => [:smart_test, 'coveralls:push']
end


task :default => 'cuke_cataloger:smart_test'

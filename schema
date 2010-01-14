#!/usr/bin/env ruby

require 'fileutils'

def usage
  puts "Usage: schema -l [partial_model_name] OR schema <model_name>"
end

if ARGV.first == '-l'
  def camelize(lower_case_and_underscored_word, first_letter_in_uppercase = true)
    if first_letter_in_uppercase
      lower_case_and_underscored_word.to_s.gsub(/\/(.?)/) { "::#{$1.upcase}" }.gsub(/(?:^|_)(.)/) { $1.upcase }
    else
      lower_case_and_underscored_word.first.downcase + camelize(lower_case_and_underscored_word)[1..-1]
    end
  end

  model_names = []
  FileUtils.cd(FileUtils.pwd + '/app/models') do
    model_names = Dir['**/*.rb'].collect {|file| camelize(file.sub!(/\.rb$/, '')) }
  end
  if ARGV.last != ARGV.first
    model_names = model_names.select {|model| model =~ /^#{ARGV.last}/ }
  end
  puts model_names.join("\n")
elsif ARGV.first
  require FileUtils.pwd + '/config/environment'

  model_name = ARGV.first
  begin
    model = model_name.constantize
    table = model.table_name
    puts model.columns.collect {|column| "#{column.name} #{column.sql_type}" }.join("\n")
  rescue
    puts "Unknown model: #{model_name}"
    usage
  end
else
  usage
end

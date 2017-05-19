# To use this build script, first install the bundler gem:
#
# $ gem install bundler
#
# Then use the `bundle` command to install the dependencies into the project:
#
# $ rm -rf Gemfile.lock .bundle
#   bundle config --local git.allow_insecure true
#   bundle config --local build.nokogiri --use-system-libraries
#   bundle --path=.bundle/gems
#
# NOTE The gems are installed under the path .bundle/gems.
#
# Finally, run a named task using the `bundle exec rake` command:
#
# $ bundle exec rake pdf
#
# You can find a list of tasks using the following:
#
# $ bundle exec rake -T
#
MASTER_FILENAME='learn-rails-51-part-II.adoc'
BUILD_DIR='output'
autoload :FileUtils, 'fileutils'

desc 'Build the HTML5 format'
task :html do
  require 'asciidoctor'
  Asciidoctor.convert_file MASTER_FILENAME,
    safe: :unsafe,
    # FIXME task should copy images instead of embedding them
    attributes: 'data-uri',
    to_dir: BUILD_DIR,
    mkdirs: true
end

desc 'Build the PDF format'
task :pdf do
  require 'asciidoctor-pdf'
  Asciidoctor.convert_file MASTER_FILENAME,
    safe: :unsafe,
    backend: 'pdf',
    to_dir: BUILD_DIR,
    mkdirs: true
end

# TIP invoke using epub[ebook-validate] to validate
desc 'Build the EPUB3 format'
task :epub, [:attrs] do |_, args|
  require 'asciidoctor-epub3'
  Asciidoctor.convert_file MASTER_FILENAME,
    safe: :unsafe,
    backend: 'epub3',
    attributes: ['epub3-stylesdir=styles/epub3', args[:attrs]].compact * ' ',
    to_dir: BUILD_DIR,
    mkdirs: true
end

desc 'Build the MOBI format'
task :mobi do
  require 'asciidoctor-epub3'
  Asciidoctor.convert_file MASTER_FILENAME,
    safe: :unsafe,
    backend: 'epub3',
    attributes: 'epub3-stylesdir=styles/epub3 ebook-format=kf8 ebook-compress=huffdic',
    to_dir: BUILD_DIR,
    mkdirs: true
  File.unlink %(#{BUILD_DIR}/#{File.basename MASTER_FILENAME, '.*'}-kf8.epub)
end
task kf8: :mobi

desc 'Build the EPUB3 and MOBI formats'
task ebook: [:epub, :mobi]

desc 'Build all formats'
task default: [:html, :ebook, :pdf]

desc 'Clean the build directory'
task :clean do
  FileUtils.remove_entry_secure BUILD_DIR
end

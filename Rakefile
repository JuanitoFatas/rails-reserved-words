require 'bundler/setup'
require 'erb'
require 'redcarpet'
require 'pry'
require 'yaml'

task :build do
  pages = build_pages
  result = ERB.new(File.read("src/index.html.erb")).result(binding)
  File.open("index.html", "w+") do |f|
    f.write(result)
  end
end

def build_pages
  pages = []
  Dir["src/*.md"].each do |f|
    page = Page.new(f)
    page.process_file
    pages << page
  end
  pages
end

class Page
  attr_accessor :lines, :file, :content, :path, :title, :metadata

  def initialize(file)
    @file = file
    @lines = File.readlines(file)
  end

  def process_file
    process_metadata
    write
  end

  def [](key)
    @metadata[key]
  end

  private

  def write
    content = @lines[@yaml_ends_at+1..-1].join
    @path = File.basename(@file, ".md") + ".html"
    File.open(path, "w+") do |output|
      output.write process_markdown
    end
  end

  def process_metadata
    @metadata ||= begin
      metadata = []
      lines.each_with_index do |line, i|
        metadata << line
        if /\A-{3,}\n\Z/.match(line)
          @yaml_ends_at = i
          break
        end
      end
      YAML.load(metadata.join)
    end
  end

  def process_markdown
    markdown = Redcarpet::Markdown.new(Redcarpet::Render::HTML)
    content = lines[@yaml_ends_at+1..-1].join
    markdown.render(content)
  end


end
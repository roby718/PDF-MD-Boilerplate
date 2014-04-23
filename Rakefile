require 'rubygems'
require 'maruku'
require 'fileutils'

HTML_OUTPUT     = "#{Rake.original_dir}/out/xhtml"
PDF_OUTPUT      = "#{Rake.original_dir}/out/pdf"
ASSETS_OUTPUT   = "#{HTML_OUTPUT}/assets"
SCSS_OUTPUT     = "#{ASSETS_OUTPUT}/scss"

PDF_NAME        = "product.pdf"



SOURCE_DIR  = "#{Rake.original_dir}/src/main"

MDOWN_DIR   = "#{SOURCE_DIR}/markdown"

HEADER_FILE = "#{MDOWN_DIR}/header.md.part"
FOOTER_FILE = "#{MDOWN_DIR}/footer.md.part"

SCSS_DIR    = "#{SOURCE_DIR}/scss"
SCSS_FILES  = ["pdf.scss"]

ASSET_DIR   = "#{SOURCE_DIR}/resources"

task :default => :compileMarkdownToHTML

task :copyAssets => [:cleanAssets] do
    FileUtils.mkdir_p ASSETS_OUTPUT
    
    puts "Running SASS preprocessor"
    FileUtils.mkdir_p SCSS_OUTPUT
    
    SCSS_FILES.collect{|f| %{#{SCSS_DIR}/#{f}}}.each {|f| 
        baseName    = File.basename(f, ".scss")
        command     = %{sass --scss "#{f}" "#{SCSS_OUTPUT}/#{baseName}.css"}
        puts command
        `#{command}` 
    }
    
    puts "Copying Assets"
    FileUtils.cp_r(ASSET_DIR, ASSETS_OUTPUT)
end

task :html => [:copyAssets] do
    header      = File.open(HEADER_FILE, 'r').read + "\n"
    footer      = "\n" + File.open(FOOTER_FILE, 'r').read

    files       = Dir["#{MDOWN_DIR}/**.md"]
    files.each { |f|
        puts "Compiling #{f}"
        contents    = File.open(f, 'r').read
        output      = Maruku.new(header + contents + footer).to_html_document

        outputFile  = File.open("#{HTML_OUTPUT}/#{File.basename(f, '.md')}.html", 'w')
        outputFile.write(output)
        outputFile.close
    }

end

# This bit is linux/mac only
# Sorry, windows. But I can't be bothered to make this work for you.
task :pdf => [:cleanPDF, :html] do
    FileUtils.mkdir_p PDF_OUTPUT

    wkhtmltopdf = `which wkhtmltopdf`.strip
    
    command = %{#{wkhtmltopdf} "#{HTML_OUTPUT}/"*.html "#{PDF_OUTPUT}/#{PDF_NAME}"}
    puts command
    `#{command}`
end

task :cleanHtml do
    puts "Cleaning HTML output"
    FileUtils.rm_r HTML_OUTPUT if File.exist? HTML_OUTPUT
end

task :cleanPDF do
    puts "Cleaning PDF output"
    FileUtils.rm_r PDF_OUTPUT if File.exist? PDF_OUTPUT
end

task :cleanAssets do
    puts "Cleaning Assets"
    FileUtils.rm_r ASSETS_OUTPUT if File.exist? ASSETS_OUTPUT
end

task :clean => [:cleanPDF, :cleanHtml, :cleanAssets]

require 'colorize'

def colorizedsh(cmd)
  puts cmd.yellow
  sh cmd
end

namespace :blog do
  
  desc "Compile the site"
  task :render do
    puts ("== Compiling blog with jekyll").green
    sh 'Rscript -e "servr::jekyll(serve = FALSE)"'
  end

  desc "Clean the site build"
  task :clean do
    puts ("== Cleaning site").green
    sh 'rm -rf _site/'
  end

  desc "view the site browser"
  task :view do
    puts ("== View site").green
    # build book
    sh 'gnome-open _site/index.html'
  end

  desc "Serve the site browser"
  task :serve do
    puts ("== View site").green
    # build book
    sh 'Rscript -e "servr::jekyll(serve = TRUE)"'
  end


end

require 'rake'
require 'date'

GIT_NAME = "pin-cong"
GIT_EMAIL = "pin-cong@github.com"

def check_destination
  unless Dir.exist? "/home/travis/data/.git"
    sh "git clone --depth=1 git@github.com:PincongBot/pin-cong-data.git /home/travis/data"
  end
  Dir.chdir("/home/travis/data") { sh "git pull" }
end

def push
  date = DateTime.now.strftime("%F")
  sh "git add --all ."
  
  files_changed = `git status --short | wc -l`.match(/\d+/)[0].to_i
  
  if files_changed > 0
    sh "git commit -m '#{date}'"
  end

  sh "git push --quiet origin master"
  puts "Pushed updated branch master"
end

task :main do

    # Detect pull request
    if ENV['TRAVIS_PULL_REQUEST'].to_s.to_i > 0
      puts 'Pull request detected.'
      exit
    end

    # Configure git if this is run in Travis CI
    if ENV["TRAVIS"]
      sh "git config --global user.name '#{GIT_NAME}'"
      sh "git config --global user.email '#{GIT_EMAIL}'"
      sh "git config --global push.default simple"
    end

    check_destination

    Dir.chdir("/home/travis/data") {
      sh "wget -O pink.sql https://gitlab.com/pin-cong/data/raw/master/pink.sql"
      puts "file downloaded"
    
      push
    }

end

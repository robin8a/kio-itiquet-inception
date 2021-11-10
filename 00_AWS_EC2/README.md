
# How to Deploy Ruby on Rails Apps on AWS EC2

1. [works! RVM on ubuntu](https://github.com/rvm/ubuntu_rvm)
   - Reboot  
- https://itnext.io/how-to-deploy-rails-5-2-on-aws-ec2-ubuntu-nginx-passenger-cd842c1c35ee

https://www.phusionpassenger.com/library/install/nginx/install/oss/bionic/

```sh
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 68980A0EA10B4DE8

# sudo apt-key adv — keyserver hkp://keyserver.ubuntu.com:80 — recv-keys 561F9B9CAC40B2F7

sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger bionic main > /etc/apt/sources.list.d/passenger.list'

 vim config/initializers/devise.rb 

 bundle exec rake assets:precompile RAILS_ENV=production
rake aborted!
Devise.secret_key was not set. Please add the following to your Devise initializer:

  config.secret_key = '<replace secret>'

cp -a /kio-itiquet/. /var/www/kio-itiquet



```

```sh
 passenger-config about ruby-command
passenger-config was invoked through the following Ruby interpreter:
  Command: /home/ubuntu/.rvm/gems/ruby-2.4.0/wrappers/ruby
  Version: ruby 2.4.0p0 (2016-12-24 revision 57164) [x86_64-linux]
  To use in Apache: PassengerRuby /home/ubuntu/.rvm/gems/ruby-2.4.0/wrappers/ruby
  To use in Nginx : passenger_ruby /home/ubuntu/.rvm/gems/ruby-2.4.0/wrappers/ruby
  To use with Standalone: /home/ubuntu/.rvm/gems/ruby-2.4.0/wrappers/ruby /usr/bin/passenger start
```

```sh
sudo vim /etc/nginx/sites-enabled/kio-itiquet.conf
```

```conf
# /etc/nginx/sites-enabled/[APP_REPOSITORY].conf
server {
  listen 80;
  server_name kio-itiquet.com;
  # Tell Nginx and Passenger where your app’s ‘public’ directory is
  root /var/www/kio-itiquet/public;
  # Turn on Passenger
  passenger_enabled on;
  passenger_ruby /home/ubuntu/.rvm/gems/ruby-2.4.0/wrappers/ruby;
}
```

# Security group port 80
https://www.codewithjason.com/install-nginx-passenger-ec2-instance-rails-hosting/

- https://itnext.io/how-to-deploy-rails-5-2-on-aws-ec2-ubuntu-nginx-passenger-cd842c1c35ee
- https://medium.com/@manishyadavv/how-to-deploy-ruby-on-rails-apps-on-aws-ec2-7ce55bb955fa
- https://www.digitalocean.com/community/tutorials/how-to-install-ruby-on-rails-with-rvm-on-ubuntu-18-04
- https://medium.com/@wintermeyer/rails-5-2-and-ruby-2-5-install-how-to-bc287f3dacef


sudo apt-get -y install curl gawk g++ \
make libreadline6-dev zlib1g-dev libssl-dev \
libyaml-dev libsqlite3-dev sqlite3 autoconf \
libgdbm-dev libncurses5-dev libtool bison nodejs \
pkg-config libffi-dev libgmp-dev libgmp-dev git \
dirmngr


curl -k -sSL https://get.rvm.io | bash


source /home/ubuntu/.rvm/scripts/rvm


sudo -u postgres createuser -s itiquet


RAILS_ENV=development bundle exec rails console

```rb

User.create({us_name: "Consultor", us_lastname: "dummy", email: "consultordummy@yopmail.com", password: "1t1qu3tc0nsult$", us_phone: "NA", us_address: "NA", us_rol: 3, us_status: 1 })

User.create({us_name: "Admin", us_lastname: "Admin", email: "admon_itiquet@yopmail.com", password: "sbiQm2nvqiNMKJnf", us_phone: "NA", us_address: "NA", us_rol: 4, us_status: 1 })
```


sudo vi /etc/init.d/unicorn_itiquet

```sh
#!/bin/sh
### BEGIN INIT INFO
# Provides:          unicorn
# Required-Start:    $all
# Required-Stop:     $all
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts the unicorn app server
# Description:       starts unicorn using start-stop-daemon
### END INIT INFO
set -e
USAGE="Usage: $0 <start|stop|restart|upgrade|rotate|force-stop>"
# app settings
USER="ubuntu"
APP_NAME="kio-itiquet"
APP_ROOT="/home/$USER/$APP_NAME"
ENV="production"
# environment settings
PATH="/home/$USER/.rbenv/shims:/home/$USER/.rbenv/bin:$PATH"
CMD="cd $APP_ROOT && bundle exec unicorn -c config/unicorn.rb -E $ENV -D"
PID="$APP_ROOT/shared/pids/unicorn.pid"
OLD_PID="$PID.oldbin"
# make sure the app exists
cd $APP_ROOT || exit 1
sig () {
  test -s "$PID" && kill -$1 `cat $PID`
}
oldsig () {
  test -s $OLD_PID && kill -$1 `cat $OLD_PID`
}
case $1 in
  start)
    sig 0 && echo >&2 "Already running" && exit 0
    echo "Starting $APP_NAME"
    su - $USER -c "$CMD"
    ;;
  stop)
    echo "Stopping $APP_NAME"
    sig QUIT && exit 0
    echo >&2 "Not running"
    ;;
  force-stop)
    echo "Force stopping $APP_NAME"
    sig TERM && exit 0
    echo >&2 "Not running"
    ;;
  restart|reload|upgrade)
    sig USR2 && echo "reloaded $APP_NAME" && exit 0
    echo >&2 "Couldn't reload, starting '$CMD' instead"
    $CMD
    ;;
  rotate)
    sig USR1 && echo rotated logs OK && exit 0
    echo >&2 "Couldn't rotate logs" && exit 1
    ;;
  *)
    echo >&2 $USAGE
    exit 1
    ;;
esac

```



sudo chmod 755 /etc/init.d/unicorn_itiquet
sudo update-rc.d unicorn_itiquet defaults


sudo service unicorn_itiquet start


scp -i kio-itiquet-kp.pem your_username@remotehost.edu:/remote/dir/foobar.txt /local/dir

scp -i "kio-itiquet-kp.pem" ubuntu@ec2-18-116-36-90.us-east-2.compute.amazonaws.com:/home/ubuntu/kio-itiquet.zip ~/Downloads/


```sh
1  sudo apt-get update
    2  sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev python-software-properties libffi-dev
    3  git clone git://github.com/sstephenson/rbenv.git .rbenv
    4  pwd
    5  git config --global user.name "robin8a"
    6  git config --global user.email "robin8a@gmail.com"
    7  git clone git://github.com/sstephenson/rbenv.git .rbenv
    8  echo ‘export PATH=”$HOME/.rbenv/bin:$PATH”’ >> ~/.bashrc
    9  echo ‘eval “$(rbenv init -)”’ >> ~/.bashrc
   10  sudo apt install rbenv
   11  echo ‘eval “$(rbenv init -)”’ >> ~/.bashrc
   12  git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
   13  echo ‘export PATH=”$HOME/.rbenv/plugins/ruby-build/bin:$PATH”’ >> ~/.bashrc
   14  source ~/.bashrc
   15  echo ‘export PATH=”$HOME/.rbenv/plugins/ruby-build/bin:$PATH”’ >> ~/.bashrc
   16  source ~/.bashrc
   17  rbenv install -v 2.2.3
   18  sudo apt install gnupg2
   19  gpg2 --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
   20  clear
   21  gpg2 --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
   22  cd /tmp
   23  curl -sSL https://get.rvm.io -o rvm.sh
   24  rbenv global 2.2.3
   25  echo “gem: --no-document” > ~/.gemrc
   26  cd ~
   27  pt-get -y install curl gawk g++ make libreadline6-dev zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgdbm-dev libncurses5-dev libtool bison nodejs pkg-config libffi-dev libgmp-dev libgmp-dev git dirmngr
   28  apt-get -y install curl gawk g++ make libreadline6-dev zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgdbm-dev libncurses5-dev libtool bison nodejs pkg-config libffi-dev libgmp-dev libgmp-dev git dirmngr
   29  sudo apt-get -y install curl gawk g++ make libreadline6-dev zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 autoconf libgdbm-dev libncurses5-dev libtool bison nodejs pkg-config libffi-dev libgmp-dev libgmp-dev git dirmngr
   30  clear
   31  gpg — keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
   32  curl -sSL https://get.rvm.io | bash
   33  curl -k -sSL https://get.rvm.io | bash
   34  source /home/ubuntu/.rvm/scripts/rvm
   35  rvm install 2.5
   36  gem install rails -v 5.2.0
   37  ruby -v
   38  rails -v
   39  git -l
   40  git clone https://github.com/robin8a/kio-itiquet.git
   41  ls
   42  cd kio-itiquet/
   43  ruby -v
   44  rvm install "ruby-2.4.0"
   45  RAILS_ENV=development bundle install
   46  sudo apt-get update
   47  sudo apt-get install postgresql postgresql-contrib libpq-dev
   48  sudo -u postgres createuser -s pguser
   49  sudo -u postgres psql
   50  pwd
   51  sudo -u postgres createuser -s itiquet
   52  sudo -u postgres psql
   https://medium.com/coding-blocks/creating-user-database-and-adding-access-on-postgresql-8bfcd2f4a91e

   ```sql
    sudo -u postgres createuser itiquet
    sudo -u postgres psql
    alter user itiquet with encrypted password 'itiqu3t$';
   ```
   53  clear
   54  RAILS_ENV=development bundle install
   RAILS_ENV=production bundle install
   55  clear
   56  RAILS_ENV=development bundle exec rake db:create
   57  RAILS_ENV=development bundle exec rake db:migrate
   58  RAILS_ENV=development bundle exec rake db:seed
   59  git pull
   60  RAILS_ENV=development bundle exec rake db:seed
   61  git pull
   62  RAILS_ENV=development bundle exec rake db:seed
   63  cleawr
   64  clear
   65  RAILS_ENV=development bundle exec rails console
   66  RAILS_ENV=development bundle exec rake db:seed
   67  rails s
   68  clear
   69  rails -v
   70  ruby -v
   71  ls
   72  cd kio-itiquet/
   73  git pull
   74  history
   75  RAILS_ENV=development bundle install
   76  vi config/unicorn.rb
   77  clear
   78  git pull
   79  sudo vi /etc/init.d/unicorn_REPO
   80  sudo chmod 755 /etc/init.d/unicorn_REPO
   81  sudo update-rc.d unicorn_REPO defaults
   82  sudo service unicorn_REPO start
   83  systemctl status unicorn_REPO.service
   84  sudo vi /etc/init.d/unicorn_itiquet
   85  sudo chmod 755 /etc/init.d/unicorn_itiquet
   86  sudo update-rc.d unicorn_itiquet defaults
   87  cd kio-itiquet/
   88  sudo service unicorn_itiquet start
   89  systemctl status unicorn_itiquet.service
   90  journalctl -xe
   91  sudo vi /etc/init.d/unicorn_itiquet
   92  sudo chmod 755 /etc/init.d/unicorn_itiquet
   93  sudo update-rc.d unicorn_itiquet defaults
   94  sudo service unicorn_REPO start
   95  systemctl status unicorn_REPO.service
   96  journalctl -xe
   97  exit
   98  pwd
   99  cd ..
  100  zip -r kio-itiquet.zip kio-itiquet/
  101  ls
  102  pwd
  103  git status
  104  cd kio-itiquet/
  105  git status
  106  history

```
```sh
RAILS_ENV=development bundle install
RAILS_ENV=development gem install bundler:2.2.30
gem install bundler:2.2.30


rvm install "ruby-2.4.0"
rvm install "ruby-2.5.9"

RAILS_ENV=development bundle install
```

```sh
scp -i "kio-itiquet-kp.pem" ubuntu@ec2-18-116-36-90.us-east-2.compute.amazonaws.com:/home/ubuntu/kio-itiquet.zip ~/Downloads/

```

```sh
cd REPO
sudo apt-get install libpq-dev
bundle install
RAILS_ENV=production bundle exec rake db:create 
rake db:migrate RAILS_ENV=production


RAILS_ENV=production bundle install
RAILS_ENV=production bundle exec rake db:create 
RAILS_ENV=production rake db:migrate 


config.secret_key = '94b4b8661b5fce39c55ccda7b9e8241afa70dc6e7d48a181feb9d5526f278b6f36e079eb7bdd0ee3ea134e29c4f04e10f395c43e88148cbaccfc25071287925c'

```


- https://stackoverflow.com/questions/30089977/devise-token-auth-error-devise-secret-key-was-not-set


```sh
cd $APP_ROOT; 
bundle exec unicorn -D -c /home/ubuntu/kio-itiquet/config/unicorn.rb -E production



/home/ubuntu/kio-itiquet

```



gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

sudo gpg2 — keyserver hkp://pool.sks-keyservers.net — recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

keyserver.ubuntu.com



/usr/share/rvm
/usr/share/rvm/scripts/rvm
/usr/share/rvm/src/rvm
/usr/share/rvm/src/rvm/scripts/rvm
/usr/share/rvm/src/rvm/lib/rvm
/usr/share/rvm/src/rvm/bin/rvm
/usr/share/rvm/lib/rvm
/usr/share/rvm/bin/rvm
/usr/share/doc/rvm

export PATH=$PATH:/opt/rvm/bin:/opt/rvm/sbin


```sh
Installing RVM to /usr/share/rvm/
Installation of RVM in /usr/share/rvm/ is almost complete:

  * First you need to add all users that will be using rvm to 'rvm' group,
    and logout - login again, anyone using rvm will be operating with `umask u=rwx,g=rwx,o=rx`.

  * To start using RVM you need to run `source /etc/profile.d/rvm.sh`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
  * Please do NOT forget to add your users to the rvm group.
     The installer no longer auto-adds root or users to the rvm group. Admins must do this.
     Also, please note that group memberships are ONLY evaluated at login time.
     This means that users must log out then back in before group membership takes effect!
Thanks for installing RVM 🙏
Please consider donating to our open collective to help us maintain RVM.


export PATH=$PATH:/opt/rvm/bin:/opt/rvm/sbin
export PATH=$PATH:/usr/share/rvm/bin:/usr/share/rvm/bin/rvmsudo

which rvm/bin

```
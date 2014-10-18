# Padrino tutorial.

Getting started.

## What's this all about?

[padrino-tutorial](https://github.com/padrino-tutorial) is an effort to make a live Padrino tutorial from the ground up going from 0 to a web app led by [Ankita](https://github.com/ankitashukla) and [Divya](https://github.com/divyaupadhyay) and guided by [Darío](https://github.com/dariocravero).

## So you do it live, right?

Well, that's the idea. We'll be doing hangouts every now and then as we build our Chat application.
We chose a Chat app because the concept simple enough to grasp but it's still complex enough for us to showcase all the building blocks of the app at the end of the day.

## The "knowledge milestones"

*`TODO` cleanup notes.*

The "knowledge milestones", or the topics we'll cover, will highly likely be the following (the order isn't strict):

- layout the idea / problem being solved;

- setup the environment:

    I'd love to showcase postmodern's concepts -we've been using chruby and its friends in production for a long while now and they truly shine.

- build a minimal prototype that works:

    highly likely in Sinatra and evolve it (thoughts?);

- implement a backend API:

    we'll touch base with generators, helpers, models, plugins;

- implement the client side of it:

    since we would be building an API approach I'd suggest evolve to some JS rendering at some point - fairly simple;

    implement asset management;

    implement stylesheets in scss;

- introduce/implement versioned APIs:
    multiple apps, etc.;

- implement websockets;

- introduce deployments:

    deploy to heroku for quick startup;

    introduce minimal server configuration (web server, application server, database, etc.) and probably deploy to something
    like Digital Ocean.

- introduce/implement an admin:

    we'll probably use the admin :);

- introduce caching concepts:

    at least in-app with Moneta;

- introduce events/background processing:

    maybe using a task that may delay stuff like sending an email plus we get to showcase the mailer

    we've been using Ost and besides it being extremely simple and small (it passes messages through Redis) it's very robust.

- gemify everything for a simple distribution.

- how to use jruby (would this be of interest?)

## Base setup

We'll use [postmodern](https://github.com/postmodern) tools to setup our Ruby installation.
Ruby comes in many flavours, we'll be sticking to the basics and using [MRI](ruby-lang.org) (the official one) on its latest  version for now.

Only Mac and Linux are covered for now; feel free to fork and add any Windows guides.

### Installing Ruby

Your OS may bring some version of MRI. Unfortunately most times it's outdated :(.
We'll use [ruby-install](https://github.com/postmodern/ruby-install) to sort that out.

```
cd ~
mkdir src
wget -O ruby-install-master.tar.gz https://github.com/postmodern/ruby-install/archive/master.tar.gz
tar -xzvf ruby-install-master.tar.gz
cd ruby-install-master/
sudo make install
ruby-install ruby
```

### Making that Ruby our default

[chruby](https://github.com/postmodern/chruby) will help us cleanly switch between installed rubies. Let's install it:

```
cd ~/src
wget -O chruby-master.tar.gz https://github.com/postmodern/chruby/archive/master.tar.gz
tar -xzvf chruby-master.tar.gz
cd chruby-master/
sudo make install
```

Then make `bash` load it by default. Run:

```
echo "source /usr/local/share/chruby/chruby.sh" >> ~/.bash_profile
echo "chruby ruby" >> ~/.bash_profile
source /usr/local/share/chruby/chruby.sh
chruby ruby
```

### Making sure we isolate our Ruby apps' dependencies

*`TODO` Eventually we want to move into a completely isolated dev environment. Somebody feels like explaining that? :)*

For those that are new to it, Ruby has the concept of `gems` to encapsulate and share functionality. A `gem` can depend on others.

To make our lives easier for now, we'll be using `bundler` to manage those dependencies automatically for us. `bundler` introduces the concept of a `Gemfile` that allows us to tell which `gems` our app needs to run and it takes care of the rest.

All `gems` get installed in `$GEM_HOME`, an environment variable that the Ruby uses to tell where to locate them. That common space can be cluttered very easily as we install and develop Ruby applications.
A common solution to that issue was to run every local command with `bundle exec` prefixing it. That's unnatural though as Ruby-installed binaries are just that and they should just work -give you're on the right path :).
That's why it's generally a good practice to isolate your app's `gems` into their own silos. It will make it easier to manage your apps and have less surprises when going to production this way.

[gem_home](https://github.com/postmodern/gem_home) is here to sort just that as it quickly let's you change `$GEM_HOME` thus creating new silos everywhere. Let's install it:

```
cd ~/src
wget -O gem_home-master.tar.gz https://github.com/postmodern/gem_home/archive/master.tar.gz
tar -xzvf gem_home-master.tar.gz
cd gem_home-master
sudo make install
```

Then make `bash` load it by default. Run:

```
echo "source /usr/local/share/gem_home/gem_home.sh" >> ~/.bash_profile
source /usr/local/share/gem_home/gem_home.sh
gem_home $HOME
gem instal bundler
```

User-wide `gems` will live in `$HOME`. Since `gem_home` works by stacking up paths and not to repeat it, that's where we want `bundler` and that's why we've installed it just now.

Please note that when we go into a specific app we should always run `gem_home .` before typing in any other Ruby command related to that app.

## Tips and tricks while running your Padrino applications

- Set `$GEM_HOME` (run this before typing in any other Ruby command related to your app when entering its directory):

```
gem_home .
```

- Install your app dependencies through `bundler`:

```
bundle
# or
bundle install
```

- Update your app dependencies through `bundler`:

```
bundle update
```

- Start Padrino's web server:

```
padrino s
# or
padrino start
```

- Start Padrino's console:

```
padrino c
# or
padrino console
```

- See what tasks you have available to run:

```
rake -T
```


- Run your tests:

```
rake test   
```

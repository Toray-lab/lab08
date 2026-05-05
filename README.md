[![Build Status](https://travis-ci.org/Toray-lab/lab04.svg?branch=master)](https://travis-ci.org/Toray-lab/lab04)
[![Build Status](https://travis-ci.org/Toray-lab/lab04.svg?branch=master)](https://travis-ci.org/Toray-lab/lab04)
# Отчёт к лабораторной работе №4
Устанавливаем переменные окрудения
```bash
$ export GITHUB_USERNAME=Toray-lab
$ export GITHUB_TOKEN=<токен>
```
Переходим в рабочую директорию и активируем окружение:
```bash
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

Устанавливаем RVM и Ruby
```bash
$ \curl -sSL https://get.rvm.io | bash -s -- --ignore-dotfiles
Turning on ignore dotfiles mode.
Downloading https://github.com/rvm/rvm/archive/master.tar.gz
Installing RVM to /home/vlad/.rvm/
Installation of RVM in /home/vlad/.rvm/ is almost complete:

  * To start using RVM you need to run `source /home/vlad/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
Thanks for installing RVM 🙏
Please consider donating to our open collective to help us maintain RVM.

👉  Donate: https://opencollective.com/rvm/donate

$ echo "source $HOME/.rvm/scripts/rvm" >> scripts/activate
$ . scripts/activate
$ rvm autolibs disable
$ sudo apt install ruby-full ruby-dev build-essential zlib1g-dev
ruby-dev is already the newest version (1:3.3+b1).
zlib1g-dev is already the newest version (1:1.3.dfsg+really1.3.1-1+b1).
Upgrading:
  dpkg

Installing:
  build-essential  ruby-full

Installing dependencies:
  dpkg-dev                   libdpkg-perl
  fakeroot                   libfakeroot
  libalgorithm-diff-perl     libfile-fcntllock-perl
  libalgorithm-diff-xs-perl  ri
  libalgorithm-merge-perl

Suggested packages:
  debian-keyring  debian-tag2upload-keyring  bzr

Summary:
  Upgrading: 1, Installing: 11, Removing: 0, Not Upgrading: 202
  Download size: 3,747 kB
  Space needed: 4,699 kB / 13.7 GB available

Continue? [Y/n] Y
Get:1 http://deb.debian.org/debian trixie/main amd64 dpkg amd64 1.22.22 [1,537 kB]
Get:2 http://deb.debian.org/debian trixie/main amd64 libdpkg-perl all 1.22.22 [651 kB]
Get:3 http://deb.debian.org/debian trixie/main amd64 dpkg-dev all 1.22.22 [1,337 kB]
Get:4 http://deb.debian.org/debian trixie/main amd64 build-essential amd64 12.12 [4,624 B]
Get:5 http://deb.debian.org/debian trixie/main amd64 libfakeroot amd64 1.37.1.1-1 [29.6 kB]
Get:6 http://deb.debian.org/debian trixie/main amd64 fakeroot amd64 1.37.1.1-1 [76.0 kB]
Get:7 http://deb.debian.org/debian trixie/main amd64 libalgorithm-diff-perl all 1.201-1 [43.3 kB]
Get:8 http://deb.debian.org/debian trixie/main amd64 libalgorithm-diff-xs-perl amd64 0.04-9 [11.1 kB]
Get:9 http://deb.debian.org/debian trixie/main amd64 libalgorithm-merge-perl all 0.08-5 [11.8 kB]
Get:10 http://deb.debian.org/debian trixie/main amd64 libfile-fcntllock-perl amd64 0.22-4+b4 [34.6 kB]
Get:11 http://deb.debian.org/debian trixie/main amd64 ri all 1:3.3 [5,192 B]
Get:12 http://deb.debian.org/debian trixie/main amd64 ruby-full all 1:3.3 [5,132 B]
Fetched 3,747 kB in 2s (2,274 kB/s)
Reading changelogs... Done
(Reading database ... 142892 files and directories currently installed.)
Preparing to unpack .../dpkg_1.22.22_amd64.deb ...
Unpacking dpkg (1.22.22) over (1.22.21) ...
Setting up dpkg (1.22.22) ...
Selecting previously unselected package libdpkg-perl.
(Reading database ... 142892 files and directories currently installed.)
Preparing to unpack .../00-libdpkg-perl_1.22.22_all.deb ...
Unpacking libdpkg-perl (1.22.22) ...
Selecting previously unselected package dpkg-dev.
Preparing to unpack .../01-dpkg-dev_1.22.22_all.deb ...
Unpacking dpkg-dev (1.22.22) ...
Selecting previously unselected package build-essential.
Preparing to unpack .../02-build-essential_12.12_amd64.deb ...
Unpacking build-essential (12.12) ...
Selecting previously unselected package libfakeroot:amd64.
Preparing to unpack .../03-libfakeroot_1.37.1.1-1_amd64.deb ...
Unpacking libfakeroot:amd64 (1.37.1.1-1) ...
Selecting previously unselected package fakeroot.
Preparing to unpack .../04-fakeroot_1.37.1.1-1_amd64.deb ...
Unpacking fakeroot (1.37.1.1-1) ...
Selecting previously unselected package libalgorithm-diff-perl.
Preparing to unpack .../05-libalgorithm-diff-perl_1.201-1_all.deb ...
Unpacking libalgorithm-diff-perl (1.201-1) ...
Selecting previously unselected package libalgorithm-diff-xs-perl.
Preparing to unpack .../06-libalgorithm-diff-xs-perl_0.04-9_amd64.deb ...
Unpacking libalgorithm-diff-xs-perl (0.04-9) ...
Selecting previously unselected package libalgorithm-merge-perl.
Preparing to unpack .../07-libalgorithm-merge-perl_0.08-5_all.deb ...
Unpacking libalgorithm-merge-perl (0.08-5) ...
Selecting previously unselected package libfile-fcntllock-perl.
Preparing to unpack .../08-libfile-fcntllock-perl_0.22-4+b4_amd64.deb ...
Unpacking libfile-fcntllock-perl (0.22-4+b4) ...
Selecting previously unselected package ri.
Preparing to unpack .../09-ri_1%3a3.3_all.deb ...
Unpacking ri (1:3.3) ...
Selecting previously unselected package ruby-full.
Preparing to unpack .../10-ruby-full_1%3a3.3_all.deb ...
Unpacking ruby-full (1:3.3) ...
Setting up libfile-fcntllock-perl (0.22-4+b4) ...
Setting up libalgorithm-diff-perl (1.201-1) ...
Setting up ri (1:3.3) ...
Setting up libfakeroot:amd64 (1.37.1.1-1) ...
Setting up fakeroot (1.37.1.1-1) ...
update-alternatives: using /usr/bin/fakeroot-sysv to provide /usr/bin/fakeroot (fakeroot) in auto mode
Setting up libdpkg-perl (1.22.22) ...
Setting up ruby-full (1:3.3) ...
Setting up libalgorithm-diff-xs-perl (0.04-9) ...
Setting up libalgorithm-merge-perl (0.08-5) ...
Setting up dpkg-dev (1.22.22) ...
Setting up build-essential (12.12) ...
Processing triggers for man-db (2.13.1-1) ...
Processing triggers for libc-bin (2.41-12+deb13u2) ...

$ gem install travis --user-install
Fetching net-http-pipeline-1.0.1.gem
Fetching connection_pool-3.0.2.gem
Fetching net-http-persistent-4.0.8.gem
Fetching multi_json-1.21.1.gem
Fetching ffi-1.17.4-x86_64-linux-gnu.gem
Fetching ethon-0.18.0.gem
Fetching typhoeus-1.6.0.gem
Fetching faraday-net_http-3.0.2.gem
Fetching faraday-2.7.12.gem
Fetching faraday-typhoeus-2.0.0.gem
Fetching faraday-retry-2.4.0.gem
Fetching public_suffix-7.0.5.gem
Fetching addressable-2.9.0.gem
Fetching concurrent-ruby-1.3.6.gem
Fetching tzinfo-2.0.6.gem
Fetching prism-1.9.0.gem
Fetching minitest-6.0.6.gem
Fetching i18n-1.14.8.gem
Fetching activesupport-7.0.10.gem
Fetching travis-gh-0.21.0.gem
Fetching rack-3.2.6.gem
Fetching rack-test-2.1.0.gem
Fetching websocket-1.2.11.gem
Fetching pusher-client-0.6.2.gem
Fetching travis-1.14.0.gem
Fetching launchy-2.5.2.gem
Fetching json_pure-2.6.3.gem
Fetching highline-2.1.0.gem
Fetching faraday-rack-2.1.3.gem
Successfully installed net-http-pipeline-1.0.1
Successfully installed connection_pool-3.0.2
Successfully installed net-http-persistent-4.0.8
Successfully installed multi_json-1.21.1
Successfully installed ffi-1.17.4-x86_64-linux-gnu
Successfully installed ethon-0.18.0
Successfully installed typhoeus-1.6.0
Successfully installed faraday-net_http-3.0.2
Successfully installed faraday-2.7.12
Successfully installed faraday-typhoeus-2.0.0
Successfully installed faraday-retry-2.4.0
Successfully installed public_suffix-7.0.5
Successfully installed addressable-2.9.0
Successfully installed concurrent-ruby-1.3.6
Successfully installed tzinfo-2.0.6
Building native extensions. This could take a while...
Successfully installed prism-1.9.0
WARNING:  You don't have /home/vlad/.local/share/gem/ruby/3.3.0/bin in your PATH,
          gem executables (minitest) will not run.
Successfully installed minitest-6.0.6
Successfully installed i18n-1.14.8
Successfully installed activesupport-7.0.10
Successfully installed travis-gh-0.21.0
Successfully installed rack-3.2.6
Successfully installed rack-test-2.1.0
Successfully installed websocket-1.2.11
Successfully installed pusher-client-0.6.2
WARNING:  You don't have /home/vlad/.local/share/gem/ruby/3.3.0/bin in your PATH,
          gem executables (launchy) will not run.
Successfully installed launchy-2.5.2
Successfully installed json_pure-2.6.3
Successfully installed highline-2.1.0
Successfully installed faraday-rack-2.1.3
WARNING:  You don't have /home/vlad/.local/share/gem/ruby/3.3.0/bin in your PATH,
          gem executables (travis) will not run.
Successfully installed travis-1.14.0
Parsing documentation for net-http-pipeline-1.0.1
Installing ri documentation for net-http-pipeline-1.0.1
Parsing documentation for connection_pool-3.0.2
Installing ri documentation for connection_pool-3.0.2
Parsing documentation for net-http-persistent-4.0.8
Installing ri documentation for net-http-persistent-4.0.8
Parsing documentation for multi_json-1.21.1
Installing ri documentation for multi_json-1.21.1
Parsing documentation for ffi-1.17.4-x86_64-linux-gnu
Installing ri documentation for ffi-1.17.4-x86_64-linux-gnu
Parsing documentation for ethon-0.18.0
Installing ri documentation for ethon-0.18.0
Parsing documentation for typhoeus-1.6.0
Installing ri documentation for typhoeus-1.6.0
Parsing documentation for faraday-net_http-3.0.2
Installing ri documentation for faraday-net_http-3.0.2
Parsing documentation for faraday-2.7.12
Installing ri documentation for faraday-2.7.12
Parsing documentation for faraday-typhoeus-2.0.0
Installing ri documentation for faraday-typhoeus-2.0.0
Parsing documentation for faraday-retry-2.4.0
Installing ri documentation for faraday-retry-2.4.0
Parsing documentation for public_suffix-7.0.5
Installing ri documentation for public_suffix-7.0.5
Parsing documentation for addressable-2.9.0
Installing ri documentation for addressable-2.9.0
Parsing documentation for concurrent-ruby-1.3.6
Installing ri documentation for concurrent-ruby-1.3.6
Parsing documentation for tzinfo-2.0.6
Installing ri documentation for tzinfo-2.0.6
Parsing documentation for prism-1.9.0
Installing ri documentation for prism-1.9.0
Parsing documentation for minitest-6.0.6
Installing ri documentation for minitest-6.0.6
Parsing documentation for i18n-1.14.8
Installing ri documentation for i18n-1.14.8
Parsing documentation for activesupport-7.0.10
Installing ri documentation for activesupport-7.0.10
Parsing documentation for travis-gh-0.21.0
Installing ri documentation for travis-gh-0.21.0
Parsing documentation for rack-3.2.6
Installing ri documentation for rack-3.2.6
Parsing documentation for rack-test-2.1.0
Installing ri documentation for rack-test-2.1.0
Parsing documentation for websocket-1.2.11
Installing ri documentation for websocket-1.2.11
Parsing documentation for pusher-client-0.6.2
Installing ri documentation for pusher-client-0.6.2
Parsing documentation for launchy-2.5.2
Installing ri documentation for launchy-2.5.2
Parsing documentation for json_pure-2.6.3
Installing ri documentation for json_pure-2.6.3
Parsing documentation for highline-2.1.0
Installing ri documentation for highline-2.1.0
Parsing documentation for faraday-rack-2.1.3
Installing ri documentation for faraday-rack-2.1.3
Parsing documentation for travis-1.14.0
Installing ri documentation for travis-1.14.0
Done installing documentation for net-http-pipeline, connection_pool, net-http-persistent, multi_json, ffi, ethon, typhoeus, faraday-net_http, faraday, faraday-typhoeus, faraday-retry, public_suffix, addressable, concurrent-ruby, tzinfo, prism, minitest, i18n, activesupport, travis-gh, rack, rack-test, websocket, pusher-client, launchy, json_pure, highline, faraday-rack, travis after 66 seconds
29 gems installed
```

Клонируем предыдущую работу (lab03) как основу для lab04:
```bash
$ git clone https://github.com/${GITHU
B_USERNAME}/lab03- projects/lab04
Cloning into 'projects/lab04'...
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (6/6), 4.44 KiB | 758.00 KiB/s, done.

$ cd projects/lab04
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04
```

Создаём файл .travis.yml:
```bash
$ cat > .travis.yml <<EOF
language: cpp
EOF
$ cat >> .travis.yml <<EOF

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install
EOF
$ cat >> .travis.yml <<EOF

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
EOF
```
Авторизуемся в Travis CLI и проверяем конфигурацию:
```bash
$ travis login --github-token ${GITHUB_TOKEN}
Successfully logged in as Toray-lab!
$ travis lint
Hooray, .travis.yml looks valid :)
```
Добавляем значок Travis CI в README.md (используем ex для вставки в первую строку):
```bash
$ ex -sc '1i|[![Build Status](https://travis-ci.org/'${GITHUB_USERNAME}'/lab04.svg?branch=master)](https://travis-ci.org/'${GITHUB_USERNAME}'/lab04)' -cx README.md
```

Коммитим изменения и пушим:
```bash
$ git add .travis.yml README.md
$ git commit -m "added CI"
[main ce86d18] added CI
 2 files changed, 8 insertions(+)
 create mode 100644 .travis.yml
$ git push origin main
```

Проверяем Travis CI:
```bash
$ travis lint
Hooray, .travis.yml looks valid :)
```

Просматриваем аккаунты, синхронизируем и включаем репозиторий:
```bash
$ travis accounts
Toray-lab (Toray-lab): not subscribed, 20 repositories
To set up a subscription, please visit app.travis-ci.com.
$ travis sync
synchronizing: . done
$ travis repos
Toray-lab/DZ2 /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/command.rb:334:in `format': wrong number of arguments (given 5, expected 1..3) (ArgumentError)
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/command.rb:315:in `store_error'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/command.rb:235:in `rescue in execute'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/command.rb:200:in `execute'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli.rb:66:in `run'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/bin/travis:21:in `<top (required)>'
        from /home/vlad/.local/share/gem/ruby/3.3.0/bin/travis:25:in `load'
        from /home/vlad/.local/share/gem/ruby/3.3.0/bin/travis:25:in `<main>'
/home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/repos.rb:23:in `block in run': can't modify frozen String: "(" (FrozenError)
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/repos.rb:18:in `each'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/repos.rb:18:in `run'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli/command.rb:208:in `execute'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/lib/travis/cli.rb:66:in `run'
        from /home/vlad/.rvm/gems/ruby-2.4.2/gems/travis-1.14.0/bin/travis:21:in `<top (required)>'
        from /home/vlad/.local/share/gem/ruby/3.3.0/bin/travis:25:in `load'
        from /home/vlad/.local/share/gem/ruby/3.3.0/bin/travis:25:in `<main>'
$ travis enable
Toray-lab/lab04: enabled :)
```

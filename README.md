# Buggy Project

An exercise for debugging cloned from Made Tech's learning guide https://learn.madetech.com/guides/03-Debugging/ . 

Original GitHub repo https://github.com/madetech/learn/tree/master/guides/03-Debugging/BuggyProject was a part in a whole repo. It was not very handy to clone only that folder in a local machine, so I take out this folder and store it in this repo.

## Known issue
There seems to be an issue of installing Ruby gem `ffi` on some versions of macOS / Ruby.


```
Installing ffi 1.9.25 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.
```

I was able to solve this issue on my machine, with below config:
macOS version: 12.1 (Monterey)
Chip: Apple M1 Pro
Ruby version: 2.6.9 (installed by rbenv)


Here is what I did to solve this error.

Step 1. Copy the whole BuggyProject folder from a local clone of https://github.com/madetech/learn/tree/master/guides/03-Debugging/BuggyProject. Run `git init` and stored a copy on github. (which is the 1st commit of this repo)

Step 2. Change `.ruby-version` from `2.5.0` to `2.6.9`.
(As I failed to install 2.5.0 on my machine)

Step 3. Run `bundle install --path vendor/bundle`. got the below errors

```
Fetching ffi 1.9.25
Installing ffi 1.9.25 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

current directory:
/Users/joe.fong/academy/lectures/BuggyProject/vendor/bundle/ruby/2.6.0/gems/ffi-1.9.25/ext/ffi_c
/Users/joe.fong/.rbenv/versions/2.6.9/bin/ruby -I /Users/joe.fong/.rbenv/versions/2.6.9/lib/ruby/2.6.0
-r ./siteconf20220209-52625-kuqxsg.rb extconf.rb
```

For detail error logs, see file `bundle_install_err.txt`

Step 4. Rename `Gemfile.lock` to `Gemfile.lock.bak`. This should disable the lock file.

Step 5. Run `bundle install --path vendor/bundle` again. 
At this point, all gems, including FFI, could be successfully installed.

Step 6. Run `make serve`. 
Sinatra server runs fine and can serve an error page when visited from browser.

Step 7. Run `make test` and press enter (to force guard re-run the tests)
The tests in `spec` folder were run and the desired result of failing tests was shown.


Note:
Before renaming Gemfile.lock, bundler was trying to install `ffi` with ver 1.9.25 and failed. 
After renaming, it installs `ffi` ver 1.15.5 and succeeded.




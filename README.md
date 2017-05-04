sample_launch_app_at_login
===

This is a sample git repo to walk through the process of launching a website at login using a LaunchAgent on macOS.

# Usage Steps

1. Download `munkipkg`

        curl -L https://raw.githubusercontent.com/munki/munki-pkg/master/munkipkg -o /usr/local/bin/munkipkg

1. Set executable permissions on the `munkipkg` script

        chmod +x /usr/local/bin/munkipkg

1. (Potentially optional) Install command line tools. This will allow you to build a package.

        xcode-select --install

1. Download this git repo

        git clone https://github.com/clburlison/sample_launch_app_at_login.git

1. Change directory into the downloaded folder

        cd sample_launch_app_at_login

1. Make customizations to launchWebsite/build-info.plist:
  * The `identifier` & `name` keys

1. Make customizations to launchWebsite/payload/Library/LaunchAgent/com.github.launchWebsite.plist:
  * The `Label`, & `ProgramArguments` keys

Please us a real text editor: Atom, TextMate, Sublime, nano, vi, etc. Basically anything but TextEdit (it's bad...real bad!)

1. Now you can create the a native macOS package that will allow you to easily deploy this setting to multiple computers in a repeatable manner.

Assuming you're still in the `sample_launch_app_at_login` directory

        munkipkg launchWebsite

This will output a .pkg file to the build folder.


# Why and how this works

## Package bits
* Using munkipkg to create a native package makes this process repeatable
* munkipkg is a wrapper for native Apple packaging tools `pkgbuild`, `productbuild`, etc. but it's much easier to use
* The `build-info.plist` `ownership` key is set to `recommended`. This will automatically set the correct file permissions for the LaunchAgent to root:wheel with 644.

## LaunchAgent bits
* We are using a LaunchAgent so this process runs under the context of the logged in user
* We are using the open command line tool
* Note we are using the full path /bin/usr/open
    1. We provide it a url to open
    1. Then the `-a` flag to open with the specific application
    1. Then the specific app, Safari in this example 

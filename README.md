SSSH: Get to places faster with less typing
===========================================

This is an in-house utility that eases the process of SSHing into one of our servers, becoming root, and then CDing to a `public_html` directory. Use of this utility will save you time, key-presses, and psychological stamina!

What it does
------------

This utility does all this...

    local $ ssh -p 1234 user@server
    remote $ sudo -i
    remote $ cd /path/to/some/predetermined/directory
    
with just this:

    sssh server bookmark

How it works
------------

The utility comes in three pieces:

* The SSSH client script `sssh`, which belongs on every machine you want to SSSH from. This is the script you directly invoke, and it's responsible for connecting you and elevating you to `root`. It uses your SSH `config` file's `Host` entries.
* The SSSH router script `ssshrouter`, which belongs on every machine you want to SSSH to. This script is invoked by the client script, and takes care of changing you to your desired directory. It uses the `ssshindex` file, described below.
* The SSSH router index `ssshindex`, which also belongs on every machine you want to SSSH to. This file maps paths with short, easy-to-remember names.

When you invoke `sssh` it uses your input to find a Host in your SSH `config` file, connect to it, and become `root` on it. Lastly, it passes your input to the `ssshrouter` script, which then looks up a named directory in the `ssshindex` file to CD you to your destination.

If you need more information, you should look inside the scripts. They're currently not all that complex, and with Google by your side you should have no problems figuring out the nuts and bolts.

How to install it
-----------------

1. Place `sssh` somewhere that's in your shell's `PATH`. `/usr/bin` is recommended.
2. Setup all the machines you're going to be SSSSHing to as hosts in your SSH `config` file. Check out [Nerdarati's lovely article](http://nerderati.com/2011/03/17/simplify-your-life-with-an-ssh-config-file/) on this if you need guidance.
3. Place `ssshrouter` at `/usr/bin/ssshrouter` on every server you're going to be SSSHing to. This location is non-negotiable for now.
4. Write an `ssshindex` file and place it at `/etc/ssshindex`. This file consists of space-separated key-value records, one on each line. They are bookmarks of directories you want to be able to automatically CD to. The key is a short, memorable name which you will use as an argument for `sssh` (and thus, `ssshrouter`). The value is just the absolute path your want CD to. You can use the sample file included in this repository as a guide.  

How to use it
-------------

Just run `sssh server bookmark`. The `server` argument must match a `Host` in your SSH `config` file. The `bookmark` argument must match a bookmark in the remote server's `ssshindex` file.

Todos
-----

* Add more error checking! Check all the things!
* Make the path of the `ssshrouter` script configurable. Somehow.
* Let the user manually specify an absolute path, skipping bookmarks.

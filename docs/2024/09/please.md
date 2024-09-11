![Package Management](//cacilhas.info/img/package.png)

Something I consider really amazing is the package management idea, used by open source projects and cannibalised by big techs.

I’m not talking about programming languages and frameworks, I’m talking about operating system package management. It’s an incredibly good idea.

You can take any [GNU](https://www.gnu.org/)/[Linux](https://linux.org/), [BSD](https://www.wikiwand.com/en/articles/Berkeley_Software_Distribution), or other open source operating system, and there you’ll find a package manager.

The idea is so good that proprietary systems started to imitate, by will or not: [Scoop](https://scoop.sh/), [Homebrew](https://brew.sh/), and so on.

It has only a _tiny-tiny_ issue: every package manager has its own user interface.

It’s actually quite annoying to remember which are the command line parameters for each system. Even through the most similar operating systems, such as GNU/Linux distros, the differences between managers are big.

Take [Pacman](https://wiki.archlinux.org/title/Pacman) and [APT](https://www.wikiwand.com/en/articles/APT_(Package_Manager)), by instance.

How to install a package in an Arch Linux based system?

    pacman -S $package_name

And in a Debian GNU/Linux based system?

    apt install $package_name

I took the two most different tools, but the slightly different ones are even worse, ’cause you have to keep remember tiny variations to avoid big mistakes.

### Enter the universal package managers

Some people had the idea of creating universal package managers, in order to put some order to that chaos.

So you may trip over [Flatpak](https://www.flatpak.org/), [AppImage](https://appimage.org/), or, my favourite, [UPT](https://crates.io/crates/upt).

UPT is a real platform-agnostic package manager. I started using it some time ago, and never stopped (till now).

It’s enough reliable and stable. If you’re using virtually any Linux distro, a BSD distro, macOS with Homebrew, or Windows with a package manager, I highly recommend it.

UPT **doesn’t replace** the system package manager, it works on top of it.

### It works until it doesn’t

Just like anything else, UPT is no bed of roses.

I was using UPT in association to [Rua](https://crates.io/crates/rua), then [Yay](https://github.com/Jguer/yay).

When I was still using Rua, I’d never requested UPT to supported it, after the Math, Rua isn’t that popular tool.

However, when I migrated to Yay, I started reckoning it makes no sense to use two different package manages since Yay is a well known and popular tool.

I took a look at UPT’s code, and I find it easy to extend it to support Yay, so I did it, and I uploaded a pull request.

After a copple of weeks, my pull request kept being ignored. That upset me.

Thereat I took a decision of creating my own demi-universal package manager.

### UPT Please had born (to die premature)

I started using the UPT fork I’ve created for my pull request, and performed some changes in order to differentiate it from UPT, but I didn’t like it.

UPT code is quite good, but it uses some approaches which I don’t like (they aren’t wrong, it’s a personal taste), and if I kept it that way, I would be in trouble supporting it.

### Please Installer at last

So I [rewrote it from scratch](https://github.com/cacilhas/please). My first goal was to reproduce as much as possible UPT features I had used, so I did it.

Now I have a beta version working, and I’m using it in my systems.

I need more people using though. The more people uses it, the more stable and reliable I’ll be able to make it.

### Installation

Please Installer requires the nightly version of Rust – and it won’t probable change. While I’m writing this, the 2024-09-01 version works fine.

Therefore use the package manager of your system to install `rustup`, then run:

    rustup toolchain install nightly-2024-09-01

Install Please Install:

    cargo +nightly-2024-09-01 install please-install

If you dare, you may wanna install the development version:

    cargo +nightly-2024-09-01 install please-install --git=https://github.com/cacilhas/please.git

Please installer supports a [configuration file](https://github.com/cacilhas/please?tab=readme-ov-file#settings), `$HOME/.config/please.toml`. It’s not required, but recommended. I suggest the following initial content:

    su = true
    
    [list]
    pager = "bat --file-name='installed packages'"
    
    [search]
    pager = "bat --file-name='search $args'"

Note that it requires `bat`.

Since I use [Arch Linux](https://archlinux.org/), _btw_, with [Yay](https://github.com/Jguer/yay), my personal settings are:

    assume-yes = true
    vendor = "yay"
    
    [remove]
    su = true
    
    [list]
    pager = "bat --file-name='installed packages'"
    
    [search]
    pager = "bat --file-name='search $args'"

My son uses Flatpak, and the his settings are:

    vendor = "flatpak"
    
    [list]
    pager = "bat --file-name='installed packages'"
    
    [search]
    pager = "bat --file-name='search $args'"

So you can update your system:

    please update

### Using Please Installer

Let’s find and install Brave Browser:

    please search brave-browser

Using Yay, for me it returned the package `chaotic-aur/brave-bin` – dependenting on your system, the package name may be completely different.

So install it:

    please install chaotic-aur/brave-bin

And that’s it.

In order to upgrade it:

    please upgrade brave-bin

And to remove it:

    please remove brave-bin

List of subcommands:

    please help

### Next goals

I still brought the overall approach from UPT code to Please Installer , and now I realised it won’t stand. I need to rewrite the code again 🙁 to make it mantainable.

I have plans to use more traits and put system-bought features into dedicated modules that can be easily disabled in compiling time.

I wanna create a subcommand to help users to set their installation as well, and also implement an built-in pager.

There’s a lot to do, it’s gonna take some time to get to the point I want, but I expect to count on community to help testing and using it.

Let’s see what comes next…

* * *

Also in [DEV.to](https://dev.to/cacilhas/please-install-it-for-me-3ea9).
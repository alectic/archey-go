Archey-go
=========

[![Build Status](https://travis-ci.org/alexdreptu/archey-go.svg?branch=master)](https://travis-ci.org/alexdreptu/archey-go)
[![GoDoc](https://godoc.org/github.com/alexdreptu/archey-go?status.svg)](https://godoc.org/github.com/alexdreptu/archey-go)
[![Platform](https://img.shields.io/badge/platform-Linux-5272b4.svg)](https://www.linuxfoundation.org/)
[![License](https://img.shields.io/badge/license-MIT-5272b4.svg)](https://github.com/alexdreptu/archey-go/blob/master/LICENSE)

#### Build is currently broken due to recent refactoring of the [sysinfo](https://github.com/alexdreptu/sysinfo) module upon which this depends. I'll refactor this as well soon.

## About

_**Archey**_ is traditionally a _**Linux**_ tool for displaying system info in a pretty way on _**Arch Linux**_. It was originally written in _**Python 2**_ and then rewriten in _**Python 3**_. _**Archey-go**_ is written from scratch in _**[Go](https://golang.org)**_ and it compiles to a single statically linked binary. It's fast, it has no external dependencies and doesn't call any system utilities to gather the information. It also allows a decent amount of customization to satisfy your _**[unixporn](https://www.reddit.com/r/unixporn)**_ addiction.

![](screenshots/ss1.png?raw=true)

![](screenshots/ss2.png?raw=true)

## Installing

To install _**Archey-go**_ you must have _**Go**_ [installed](https://golang.org/doc/install) and set up to be able to compile it first.

On Arch Linux you can install _**Go**_ with ```pacman -S go go-tools```.

Clone the repository and build it.
```
git clone git@github.com:alexdreptu/archey-go.git
cd archey-go
go build
```
The result is a binary called _**archey-go**_ that you can move wherever you want afterwards.

Alternatively you can install _**Archey-go**_ via go tooling.

You need to export ```$GOPATH``` and ```$GOBIN``` then add ```$GOBIN``` to ```$PATH```

E.g. if you're using bash add to ~/.bashrc
```
export GOPATH=$HOME/.gocode
export GOBIN=$GOPATH/bin
export PATH+=:$GOBIN
```
where _**.gocode**_ can be replaced with any directory name. Then ```source ~/.bashrc```.

Assuming you have that set up, you install _**Archey-go**_ by executing
```
go get github.com/alexdreptu/archey-go
```

**NOTE:** From [Go 1.8](https://beta.golang.org/doc/go1.8) onwards you don't need to manually set ```$GOPATH``` and ```$GOBIN```. ```$GOPATH``` automatically [defaults](https://beta.golang.org/doc/go1.8#gopath) to ```$HOME/go```.

The **_archey-go_** binary will be located in ```$GOBIN```
```
alectic@particular $ echo $GOBIN
/home/alectic/.gocode/bin
alectic@particular $ which archey-go
/home/alectic/.gocode/bin/archey-go
alectic@particular $
```

## Usage

_**Archey-go**_ can be used with or without a config file. The configuration file format is _**[toml](https://github.com/toml-lang/toml)**_. Each flag responsible for configuration has an equivalent variable in its configuration file. See _**[sample_config.toml](https://github.com/alexdreptu/archey-go/blob/master/sample_config.toml)**_.

**NOTE:** _**Archey-go**_ doesn't include an option to take a screenshot, as I think this shouldn't be handled by _**Archey-go**_. Instead you can use _**scrot**_. If you just want to take a screenshot of the terminal window, you ```scrot -s -q 100 screenshot1.png``` after which you click on the terminal window. The next window you click your mouse pointer on after you run the command, will be screenshotted. For convenience run it from a command runner, usually set to ```alt+F2``` or directly from the terminal window in which _**Archey-go**_ was executed. Otherwise without ```-s``` or with a counter ```-cd 5``` to screenshot the whole display after 5 seconds.

To install _**scrot**_ you ```pacman -S scrot```.

### Flags

```
--no-os
```

Don't show OS name as read from ```/etc/os-release```.

```
--no-arch
```
Don't show architecture e.g. _**x86_64**_ which is displayed next to OS name on the same line.

```
--no-kernel
```
Don't show kernel version.

```
--no-hostname
```
Don't show hostname.

```
--no-uptime
```
Don't show system uptime.

```
--no-up-since
```
Don't show "**Up since**" - the time and date when the system booted.

```
--no-wm
```
Don't show Window Manager name.

```
--no-de
```
Don't show Desktop Environment name.

```
--no-gtk2-theme
```
Don't show GTK2 theme name. The theme name is read from ```$HOME/.gtkrc-2.0```, if it doesn't exist it's read from ```/etc/gtk-2.0/gtkrc```. If neither one of them exists, _**None**_ is displayed.

```
--no-gtk2-icon-theme
```
Don't show GTK2 icon theme name. The icon theme name is read the same way GTK2 theme name is.

```
--no-gtk2-font
```
Don't show GTK2 font name. The font name is read the same way GTK2 theme name is.

```
--no-gtk2-cursor-theme
```
Don't show GTK2 cursor theme name. The cursor theme name is read the same way GTK2 theme name is.


```
--no-gtk3-theme
```
Don't show GTK3 theme name. The GTK3 theme name is read from ```$HOME/.config/gtk-3.0/settings.ini```, if it doesn't exist it's read from ```/etc/gtk-3.0/settings.ini```. If neither one of them exists, _**None**_ is displayed.

```
--no-gtk3-icon-theme
```
Don't show GTK3 icon theme name. The icon thene name is read the same way GTK3 theme name is.

```
--no-gtk3-font
```
Don't show GTK3 font name. The font name is read the same way GTK3 theme name is.

```
--no-gtk3-cursor-theme
```
Don't show GTK3 cursor theme name. The cursor theme name is read the same way GTK3 theme name is.

```
--no-terminal
```
Don't show terminal name.

```
--no-editor
```
Don't show the default editor's name.

```
--no-packages
```
Don't show package count on Arch Linux and Arch Linux based distributions. The package count is set by reading the ```/var/lib/pacman/local``` directory. If the directory doesn't exist (on other distributions), the package count is set to zero.

```
--no-memory
```
Don't show memory usage.

```
--no-swap
```
Don't show swap usage.

```
--no-cpu
```
Don't show CPU model.

```
--no-root
```
Don't show root partition disk usage.

```
--no-home
```
Don't show home partition disk usage.

```
--sep
```
Set the string that separates the variable name from the value (default is ":"). E.g. _**OS: Arch Linux**_.

```
--memory-unit
```
Set the unit (MB or GB) to show memory usage in (default is GB). Case is insensitive.

```
--swap-unit
```
Same as ```--memory-unit```.

```
--disk-unit
```
Same as ```--memory-unit```.

```
--paths
```
Set additional paths to be showed to disk usage info. Paths are separated by ",".

E.g. ```--paths /some/path1,/some/path2,/some/path3```.

```
--path-full
```
Show full paths for root, home and the additionally added paths instead of just their basename.

```
--shell-full
```
Show full shell path instead of just its basename.

```
--up-since-format
```
Set the time and date format to be used for "**Up since**". The format used is _**[strftime](http://strftime.org/)**_.


**Supported strftime formats**

| Code | Example  | Description |
| ------- | ------------- | ----------------- |
| `%A` | `Sunday` | Full weekday name |
| `%a` | `Sun` | Abbreviated weekday name |
| `%B` | `September` | Full month name |
| `%b` | `Sep` | Abbreviated month name |
| `%C` | `20` | (`year / 100`) as number. Single digits are preceded by zero |
| `%D` | `09/21/14` | Equivalent to `%m/%d/%y` |
| `%d` | `21` | Day of month as number. Single digits are preceded by zero |
| `%e` | `21` | Day of month as number. Single digits are preceded by a blank |
| `%f` | `001234` | Microsecond as a six digit decimal number, zero-padded on the left |
| `%F` | `2014-09-21` | Equivalent to` %Y-%m-%d` |
| `%H` | `15` | The hour (24 hour clock) as a number. Single digits are preceded by zero |
| `%h` | `Sep` | Same as `%b` |
| `%I` | `03` | The hour (12 hour clock) as a number. Single digits are preceded by zero |
| `%j` | `264` | The day of the year as a decimal number. Single digits are preced by zeros |
| `%k` | `15` | The hour (24 hour clock) as a number. Single digits are preceded by a blank |
| `%L` | `001` | Millisecond as a three digit decimal number, zero-padded on the left |
| `%l` | `11` | Replaced by the hour (12 hour clock) as a number. Single digits are preceded by blank |
| `%M` | `32` | Replaced by the minute as a decimal number. Single digits are preceded by a zero |
| `%m` | `09` | Replaced by the month as a decimal number. Single digits are preceded by a zero |
| `%N` | `001234567` | Nanosecond as a 9 digit decimal number, zero-padded on the left |
| `%n` | `\n` | A newline |
| `%P` | `am` | am or pm as appropriate |
| `%p` | `AM` | AM or PM as appropriate |
| `%R` | `15:32` | Equivalent to `%H:%M` |
| `%r` | `03:32:05 AM` | Equivalent to `%I:%M:%S %p` |
| `%S` | `05` | The second as a number. Single digits are preceded by a zero |
| `%s` | `1461497457`  | The number of seconds since the Epoch, UTC |
| `%T` | `15:32:05` | Equivalant to `%H:%M:%S` |
| `%t` | `\t` | A tab |
| `%v` | `21-Sep-2014` | Equivalent to `%e-%b-%Y` |
| `%w` | `0` | The weekday (Sunday as first day of the week) as a number  |
| `%Y` | `2014` | Replaced by the year with century as a number  |
| `%y` | `14` | Year without century as a number. Single digits are preceded by zero  |
| `%Z` | `UTC` | Time zone name  |
| `%z` | `-0700` | The time zone offset from UTC |

```
--name-color
```
Set the color for the variable name. The format is _**foregroundColor+attributes:backgroundColor+attributes**_.

```
--text-color
```
Set the color for the text value. Same as ```--name-color```.

```
--sep-color
```
Set the color for the separator string. Same as ```--name-color```.

```
--body-color
```
Set the color for the logo body.

The color format is:
_**foregroundColor+attributes:backgroundColor+attributes,foregroundColor+attributes:backgroundColor+attributes**_.

There are two sections of the body, _**upper**_ and _**lower**_ separated by ",".

E.g. ```--body-color red+h:red,green+h:green```

Parts of the format can be omitted. If only colors for the upper body are specified, they will be used to color the whole body.

Set only foreground color: ```--body-color red```

Set only background color: ```--body-color :green```

Set upper and lower body colors without background colors: ```--body-color red,green```

Set only upper foreground and background colors which will colorize the whole body with the same colors: ```--body-color red+h:red```

Set upper foreground and background colors with only lower foreground color: ```--body-color red+h:red,green```

**Colors**

* black
* red
* green
* yellow
* blue
* magenta
* cyan
* white
* 0...255 (256 colors)

**Attributes**

* b = bold foreground
* B = Blink foreground
* u = underline foreground
* i = inverse
* h = high intensity (bright) foreground, background

	does not work with 256 colors

```
--list-colors
```
Show colors and styles

```
--no-color
```
Disable colors

```
--config
```
Specify config file. The configuration file is optional, _**archey-go**_ can be configured via flags. By default _**archey-go**_ looks first in the current directory for _**config.toml**_, if it's not there then it looks for ```$HOME/.archey-go/config.toml``` or ```$HOME/.config/archey-go/config.toml``` in this order. If neither one of them is there, it looks for ```/etc/archey-go/config.toml```.

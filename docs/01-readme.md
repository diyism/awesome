    #after install awesome windows manager:
    $ sudo mv /usr/bin/awesome /usr/bin/awesome.bin
    
    $ sudo micro /usr/bin/awesome
    #!/bin/bash
    killall Xephyr 2>/dev/null
    Xephyr :1 -ac -screen 1920x1080 &
    sleep 2
    DISPLAY=:1 /usr/bin/awesome.bin

    $ micro ~/.config/awesome/rc.lua
    # 所有窗口默认初始状态为不浮动:
    # awful.rules.rules { { properties = { ..., floating = false
    # 不需要float, sticky, ontop 这三个按钮:
    # awful.titlebar(c) : setup { ..., { -- Right   --awful.titlebar.widget.floatingbutton (c), ..., --awful.titlebar.widget.stickybutton   (c), --awful.titlebar.widget.ontopbutton    (c), 

    # 注释掉floating 和 max模式, 把tile.left模式放最前:
    # awful.layout.layouts = {   awful.layout.suit.tile.left, --awful.layout.suit.floating, --awful.layout.suit.max, --awful.layout.suit.max.fullscreen,
    # 在 awful.tag({ "1", "2", "3", ... 之后加:
    for _, t in ipairs(s.tags) do
        t.master_count=  1  -- awful.layout.suit.tile.left里 1个主窗口在右, 放firefox浏览器
        t.column_count = 2  -- awful.layout.suit.tile.left里 2个副窗口列在左
    end
    # 分到awesome的多个workspace的firefox window之间互相写冲突, 可能搞坏多标签页记录, 这样恢复:
    killall firefox-bin
    cp ~/.mozilla/firefox/e0bwwjak.default/sessionstore-backups/recovery.jsonlz4 ~/.mozilla/firefox/e0bwwjak.default/sessionstore-backups/recovery.jsonlz4.bak
    cp ~/.mozilla/firefox/e0bwwjak.default/sessionstore-backups/previous.jsonlz4 ~/.mozilla/firefox/e0bwwjak.default/sessionstore-backups/recovery.jsonlz4
    nohup firefox >/dev/null 2>&1 &

    # 注释掉s.mytasklist, 因为平铺管理器 根本不需要 系统任务栏
    # s.mywibox:setup { ...,  --s.mytasklist, -- Middle widget
    # 所有0,1,...,9 这些workspace切换按钮加宽:
    s.mytaglist = awful.widget.taglist {..., widget_template = { { { ..., forced_width = 120,

    #需要加语音输入法 细长状态栏的话 还要留出位置:
    -- Create an invisible wibar below mywibox to reserve space for voxkb (18px)
    s.voxkb_spacer = awful.wibar({
        position = "top",
        screen = s,
        height = 18,
        y = 20,  -- Position below mywibox
    })
    #还要将输入法状态栏设置为floating, 添加输入法标题栏窗口名voxkb:
    -- Floating clients. { rule_any = { ... name = {"Event Tester",  -- xev.   "voxkb"}, ... }, properties = { floating = true }},

![](./2025-12-18-040039_1920x1080_scrot.png)

=================================================================

# Readme

## About Awesome

Awesome is a highly configurable, next generation framework window manager for X.

## Building and installation

After extracting the dist tarball or cloning the repository, run:

```sh
make
sudo make install
```

This will

1. create a build directory at `./build`,
2. run `cmake`,
3. build Awesome and
4. install it to the default prefix path `/usr/local`.

Alternatively to the above, you can generate a `.deb` or `.rpm` package, for easy installation management:

```sh
make package

sudo dpkg -i awesome-x.y.z.deb
# or
sudo rpm -Uvh awesome-x.y.z.rpm
```

### Advanced options and testing

A full list of dependencies, more advanced build options, as well as instructions on how to use the test suite can be found [here](https://awesomewm.org/apidoc/documentation/10-building-and-testing.md.html).

### Installing current git master as a package receipts

#### Arch Linux AUR

```sh
sudo pacman -S --needed base-devel git
git clone https://aur.archlinux.org/awesome-git.git
cd awesome-git
makepkg -fsri
```

#### Debian-based

```sh
sudo apt build-dep awesome
sudo apt install libxcb-xfixes0-dev 
git clone https://github.com/awesomewm/awesome
cd awesome
make package
cd build
sudo apt install ./*.deb
```

## Running Awesome

You can directly select Awesome from your display manager. If not, you can
add the following line to your `.xinitrc` to start Awesome using `startx`
or to `.xsession` to start Awesome using your display manager:

```sh
exec awesome
```

In order to connect Awesome to a specific display, make sure that
the `DISPLAY` environment variable is set correctly, e.g.:

```sh
DISPLAY=foo.bar:1 exec awesome
```

(This will start Awesome on display `:1` of the host foo.bar.)

## Configuration

The configuration of Awesome is done by creating a
`$XDG_CONFIG_HOME/awesome/rc.lua` file, typically `~/.config/awesome/rc.lua`.

An example configuration named `awesomerc.lua` is provided in the source.

## Troubleshooting

On most systems any message printed by Awesome (including warnings and errors)
is written to `~/.xsession-errors`.

If Awesome does not start or the configuration file is not producing the
desired results the user should examine this file to gain insight into the
problem.

### Debugging tips

You can call `awesome` with `gdb` like this:

```sh
DISPLAY=:2 gdb awesome
```

Then in `gdb` set any arguments and run it:

```
(gdb) set args --replace
(gdb) run
```

## Asking questions

#### IRC

You can join us in the `#awesome` channel on the [OFTC](http://www.oftc.net/) IRC network.

[IRC Webchat](https://webchat.oftc.net/?channels=awesome)

#### Stack Overflow
You can ask questions on [Stack Overflow](http://stackoverflow.com/questions/tagged/awesome-wm).

#### Reddit
We also have an [awesome subreddit](https://www.reddit.com/r/awesomewm/) where you can share your work and ask questions.

## Reporting issues

Please report any issues you may find on [our bugtracker](https://github.com/awesomeWM/awesome/issues).

## Contributing code

You can submit pull requests on the [GitHub repository](https://github.com/awesomeWM/awesome).
Please read the [contributing guide](https://github.com/awesomeWM/awesome/blob/master/docs/02-contributing.md) for any coding, documentation or patch guidelines.

## Status
[![Build Status](https://travis-ci.com/awesomeWM/awesome.svg?branch=master)](https://travis-ci.com/awesomeWM/awesome)

## Documentation

Online documentation is available [here](https://awesomewm.org/apidoc/).

## License

The project is licensed under GNU General Public License v2 or later.
You can read it online at ([v2](http://www.gnu.org/licenses/gpl-2.0.html)
or [v3](http://www.gnu.org/licenses/gpl.html)).

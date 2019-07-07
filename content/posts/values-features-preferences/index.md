+++
title = "Values, Features, and Preferences"
date = 2019-07-07T10:22:32+10:00
+++

The goal of this post is to set the foundation for future posts. I hope that
this will serve as a reference as more investigation is done and choices and
decisions are made. First up I want to talk about values, then describe some
concrete desired features, and finally some personal preferences.

<!-- more -->

### Values

I recently watched two conference talks that that cover trade-offs and values:

* [Platform as a Reflection of Values: Joyent, node.js, and Beyond][values] &mdash; Bryan Cantrill
* [How Rust Views Tradeoffs][tradeoffs] &mdash; Steve Klabnik

I think these talks are a fascinating and useful prelude to this whole
experiment. They highlight that a given project will have core values:
traits that it values above all others. Different projects have different
values, which leads them to make different decisions. Similarly, people will
have certain core values that they hold.

When you find a project that aligns with your values that project will feel more "correct"
and projects that value different traits will feel less "correct". This is why we see
constant disagreement on the Internet when people debate programming languages, operating
systems, and yes, desktop environments.

The goal is to find the project that best aligns with your values. That's what
I'm aiming to do here. I will outline the traits I value in software as these
will consciously, or not, influence my decisions.

Bryan Cantrill presents a list of core values that a platform, project,
organisation, or person may hold:

<div class="three-columns">
  <ul>
    <li>Approachability</li>
    <li>Availability</li>
    <li>Compatibility</li>
    <li>Composability</li>
    <li>Debuggability</li>
    <li>Expressiveness</li>
    <li>Extensibility</li>
    <li>Interoperability</li>
    <li>Integrity</li>
    <li>Maintainability</li>
    <li>Measureability</li>
    <li>Operability</li>
    <li>Performance</li>
    <li>Portability</li>
    <li>Resiliency</li>
    <li>Rigor</li>
    <li>Robustness</li>
    <li>Safety</li>
    <li>Security</li>
    <li>Simplicity</li>
    <li>Stability</li>
    <li>Thoroughness</li>
    <li>Transparency</li>
    <li>Velocity</li>
  </ul>
</div>

With these in mind I consider the following my ordered set of core values for a
desktop environment:

* Completeness
* Resiliency
* Performance
* Consistency

Note that these are core values. The absence of a value from this list doesn't
mean it's undesired, just that it's not held as deeply.

#### Completeness

I want a desktop environment that provides a complete user experience, not a
build-your-own-desktop experience.

#### Resiliency

A desktop environment should ideally never crash or cause the user to lose work.

#### Performance

The desktop environment is part of the baseline CPU and memory space that a user
session will occupy. For this reason it should not use excessive memory, or CPU
when idle. Memory use should be deterministic and not balloon for no reason
over time.

#### Consistency

The desktop environment should present a consistent interface. All the parts
should feel like they belong, no part should look out of place or require
special treatment.

### Features

I'm not looking to reinvent the graphical desktop from scratch. My goal is to
find or build something that meets as many of the following features as
possible. Ideally this means combining existing paradigms, toolkits, and
software into a cohesive, just works environment.

Currently, I feel that my ideal environment would combine aspects of [GNOME]
and [Awesome]. Many of the following features are already possible with manual
installation and configuration. I'm looking for an environment with little to
no configuration required to meet these features. Although, this does not
preclude customisation.

#### Installation

Installation should only be a couple of commands. For example, installing GNOME
on [Arch Linux] is something like this:

```sh
pacman -S gnome
systemctl enable gdm
```

On [FreeBSD] it is:

```sh
pkg install xorg-minimal gnome3
sysrc dbus_enable=YES
sysrc gnome_enable=YES
```

I would like a similar installation experience.

#### Core services

- Logging into the shell takes care of:
  - Unlocking a system keychain that acts as secure credential storage, ssh agent, gpg agent
  - Screen locking on idle and laptop close
  - Provisioning a PolKit agent so that tools like `virt-manager` work.
- Window compositing that composites windows but does not impact full screen
  performance, for example in games
- Volume status and control with keyboard keys
- Screen brightness control with keyboard keys
- Network/WiFi status and control
- Screenshot tools, key bindings
- Media controls (play, pause, etc.)
- Notifications with support for images, actions, focussing the application when clicked
- HiDPI support, at least as good as GNOME
  - Correct cursor sizing
- Auto-mounting of external drives
- Automatic multi-monitor support
  - Desktop gracefully adapts to monitors being added and removed
  - Ideally mixing HiDPI + non-HiDPI monitors is supported
- Power management
  - Battery status
  - Low power warnings
- Clipboard preservation
  - I.e. clipboard source can exit and you can still paste

#### Window Management

- Tiling window management
- Multiple window layouts named as follows in Awesome and [Lain]:
    - `tile` and top, bottom, and right variants
    - `floating`
    - `centerfair`
    - `cascadetile`
    - `centerwork`
    - `termfair`
    - `fairv`, `fairh`
    - `max`
    - `fullscreen`
- Keyboard switching of a workspace layout
- Toggle floating on a window
- Focus follows mouse
- Fast switching between workspaces with keyboard
- Keyboard control of window focus, size, maximise, full-screen

#### Visuals

- Consistent, uncluttered visual language
- Limited use of animation
- Maximise vertical space
  - No title bars on tiled windows

#### Configuration

Configuration of core features can be done in a GUI:

- Mouse speed
- Scroll direction (natural/reversed)
- Date, time, and time zone
- Networking
- Monitor layout, scaling
- Trackpad click behaviour (E.g. tap to click vs. [click fingers])

### Preferences and Biases

#### Toolkit and Dependencies

Pretty much any personal computer these days will have a browser installed.
Hopefully Firefox.
[Firefox](https://www.archlinux.org/packages/extra/x86_64/firefox/) and
[Chromium](https://www.archlinux.org/packages/extra/x86_64/chromium/) both have
dependencies like [GTK], [GLib], and [D-Bus] either directly or transitively. If we
assume that in the common case these will be installed then it makes little
sense to me to jump through hoops to avoid dependencies like these. Instead, I
think it makes sense to consider these part of the base platform and use the
higher level abstractions they afford.

I generally prefer the GNOME aesthetic to the KDE one.

#### Wayland

Ideally Wayland and X.org would both be supported (for maximum portability) but
a Wayland only solution would also be acceptable.

#### Avoid Scripting Languages

It's my opinion that long-running, "base" level, software should aim to
maximise correctness, reliability and memory use. I feel that most scripting
run-times do not meet this requirement. Plus installation is often more complex
and error prone due to the absence of a compile-time phase that can reduce the
source into a final binary. Therefore, I have a strong preference for ahead of
time, compiled native code.

[Lua] \(used by Awesome) skirts some of this by being embedded and compact. It
does not avoid the problem of run-time errors though. For example, under
certain circumstances when I add or remove an external display I start getting
errors like those shown in the screenshot. They appear constantly until I
restart Awesome, losing my workspace layout assignments and sizing in the
process.

{{ figure(src="awesome-run-time-errors.png" width="434" class="screenshot" alt="Screenshot of red error notifications from Awesome" caption="Run time errors in Awesome") }}

#### No Restarts

It should not typically be necessary to restart the desktop environment to pick
up configuration changes. These should apply automatically, as most settings
do in GNOME. Newly installed desktop files should be automatically noticed without
manual intervention.

#### More Than Linux

Linux is the dominant the open source desktop. However, there is more than just
Linux out there and diversity helps. FreeBSD and [OpenBSD] both have GNOME
available. I would like a desktop that runs on more than Linux.

[values]: https://vimeo.com/230142234
[tradeoffs]: https://www.infoq.com/presentations/rust-tradeoffs/
[Lain]: https://github.com/lcpz/lain/
[click fingers]: https://wayland.freedesktop.org/libinput/doc/1.11.3/clickpad_softbuttons.html#clickfinger
[Awesome]: https://awesomewm.org/
[GNOME]: https://www.gnome.org/
[Lua]: https://www.lua.org/
[OpenBSD]: https://www.openbsd.org/
[Arch Linux]: https://www.archlinux.org/
[FreeBSD]: https://www.freebsd.org/
[GTK]: https://www.gtk.org/
[GLib]: https://gitlab.gnome.org/GNOME/glib/blob/master/README.md
[D-Bus]: https://www.freedesktop.org/wiki/Software/dbus/

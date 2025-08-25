---
layout: single
title: Install Kitty with Quake-style Terminal
tags:
  - vscode
  - ai
  - vibe
  - coding
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: Installing/Upgrading AI Tools for Coding
---

# 🚀 How to Set Up Kitty as a Quake-Style Drop-Down Terminal on KDE Plasma

A **quake-style terminal** is one you can summon instantly with a hotkey — just like the terminal in the game *Quake*. Popular tools like Yakuake or Guake have done this for years, but now you can achieve the same result natively with **Kitty**, the fast GPU-accelerated terminal emulator.

In this tutorial, we’ll set up Kitty’s **Quick Access Terminal** feature on KDE Plasma, bind it to <kbd>Meta</kbd> + <kbd>O</kbd>, and configure it to behave like a slick drop-down terminal.

---

## 1. Install or Upgrade Kitty

The **Quick Access Terminal** kitten was introduced in **Kitty 0.42**. Most Linux distributions ship an older version, so let’s install the latest official build.

### Remove the distro version (if installed with apt)

```bash
sudo apt remove -y kitty
```

This is important because Quake-like terminal is in version 0.42+

### Install the latest Kitty

```bash
curl -L https://sw.kovidgoyal.net/kitty/installer.sh | sh /dev/stdin
```

This installs Kitty to `~/.local/kitty.app/bin/kitty`.

### Add Kitty to PATH

```bash
echo 'export PATH="$HOME/.local/kitty.app/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Verify the version

```bash
kitty --version
```

![✅ You should see `0.42.2` or newer.](/assets/2025/08/kitty.png)


## 2. Configure the Quick Access Terminal

Create Kitty’s config file for the quick-access terminal:

```bash
mkdir -p ~/.config/kitty
$EDITOR ~/.config/kitty/quick-access-terminal.conf
```

Paste this example:

```conf
# ~/.config/kitty/quick-access-terminal.conf

lines 20
columns 120
edge top
background_opacity 0.85
hide_on_focus_loss yes
app_id kitty-quick
start_as_hidden yes
kitty_override font_size=13
```

This will:

* Open at the **top edge** of your screen
* Use **20 lines** of height, **120 columns** of width
* Be **semi-transparent (85%)**
* Hide automatically if you click elsewhere
* Start hidden until you toggle it

![kitty dropping down at the top of Plasma desktop.](/assets/2025/08/kitten-running.png)

Test it by running:

```bash
kitten quick-access-terminal
```

## 3. Bind It to <kbd>Meta</kbd> + <kbd>O</kbd> in KDE Plasma

Now let’s make <kbd>Meta</kbd> + <kbd>O</kbd> toggle Kitty’s quake terminal. Kitty uses `kitten` for using kitty with plugin. So you need to create application shortcut.

1. Open **System Settings → Shortcuts → Custom Shortcuts**.
2. Click **Edit → New → Global Shortcut → Command/URL**.
![Access New Script](/assets/2025/08/new-command-or-script.png)
3. Fill in:

   * **Name**: Quake Kitty
   * **Command/URL**:

     ```bash
     kitten quick-access-terminal
     ```



![KDE Plasma “New Script” dialog](/assets/2025/08/new-command-or-script-2.png)

4. Search for `kitty` or `quake kitty` and then click the edit button.
5. In the right panel, click New and press <kbd>Meta</kbd> + <kbd>O</kbd>.
![Kitty shortcut](/assets/2025/08/new-command-or-script-3.png)

Apply changes — now <kbd>Meta</kbd> + <kbd>O</kbd> will toggle your Kitty terminal.

## 4. Refine with KWin Window Rules

To make the experience smoother, you can tweak the window behavior with **KWin rules**:

1. Launch Kitty quick access terminal once.
2. Right-click its title bar → **More Actions → Special Window Settings**.
3. Add rules:

   * No window borders
   * Keep above others
   * Skip taskbar and pager
   * Placement: Top

![Insert screenshot: KWin Window Rules dialog with “Keep above others” checked](/assets/2025/08/kitten-window-settings.png)

NOTE: Press <kbd>ALT</kbd> + <kbd>F3</kbd>


## ✅ Done!

You now have a Kitty-powered quake terminal on KDE Plasma:

* Press <kbd>Meta</kbd> + <kbd>O</kbd> → terminal drops down
* Press <kbd>Meta</kbd> + <kbd>O</kbd> again → it hides
* It auto-hides when losing focus (optional)

All with the speed and GPU acceleration of Kitty.

👉 Pro tip: you can run **multiple profiles** (different colors, shells, or fonts) by creating extra config files like `quick-access-terminal-work.conf` and binding them to other hotkeys.

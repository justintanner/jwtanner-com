---
title: Emacs keybindings to rule them all
description: How I used Hammerspoon and AutoHotkey to script a set universal keybindings.
author: Justin Tanner
date: "2019-04-06"
---

When Microsoft released [Ubuntu for Windows 10][1] I took the opportunity to try web development on Windows after 8 years on a Mac. I was pleasantly surprised with Ubuntu on Windows 10. Having Ubuntu in your terminal is handy because it more closely resembles the servers I deploy my code to. But there was one frustrating aspect of switching back to Windows, keyboard shortcuts.

Some differences are easy to workaround such as `Ctrl+c` and `Cmd+c` (copy). Copy and paste can be dealt with by changing settings in the OS, but other shortcuts are quite different such as `Cmd+q` and `Alt+F4` (quit an app). Instead of memorizing these differences I wondered if there was a better solution.

My text editor of choice is Emacs. Once inside Emacs you enter another world of keyboard shortcuts. Goodbye `Ctrl+c` and hello `Ctrl+y`. Navigating text starts by holding down a modifier key such as `Ctrl` or `Alt` and your hands never leave the home row. Would it be possible to use Emacs shortcuts to control both operating systems in the same way?

## Are there any apps that already do this?

I started by looking for something I could download or buy to solve the problem. For Windows I found [XKeymacs][2] which described it self as "a keyboard utility to realize Emacs like-usability on all windows applications".

On Mac I couldn't find any app to solve the problem, but OSX supports **most** Emacs shortcuts out of the box. For example `Ctrl+a` is mapped to the start of the line character just like Emacs.

Both XKeymacs and the default shortcuts for Mac failed to support two important Emacs features:

1. Selecting text with `Ctrl+Space` (start mark), move down a few lines to expand the text selection and `Ctrl+Space` (end mark)
2. Prefix keys or chords shortcuts involving a modifier key and **two** other keystrokes such as `Ctrl+xs` (save)

## Are there any scripts that do this?

Next I searched Github for solutions and I found some [interesting][5], [scripts][6]. These scripts didn't solve the problems of selecting text or chord keys, but they were written in two apps / frameworks that were new to me.

* [Hammerspoon][4] A Lua based scripting language to intercept keystroke and mouse events (Mac)
* [AutoHotkey][3] A scripting language that intercepts keystrokes and mouse events (Windows)

Hammerspoon and AutoHotkey had potential, but I needed to make sure they could do what I wanted, so I wrote some experimental code in these new environments.

## Text selection in AutoHotkey

First I set out to see if AutoHotkey could [emulate Emacs style text selection][18].

```c
#SingleInstance
#Installkeybdhook
#UseHook
SetKeyDelay 0

global selecting := False

SendWithShift(keys)
{
  If (selecting)
  {
    Send +%keys%
  }
  Else
  {
    Send %keys%
  }
}

^Space::
selecting := !selecting
Return

^n::
SendWithShift("{Down}")
Return

^p::
SendWithShift("{Up}")
Return

^f::
SendWithShift("{Right}")
Return

^b::
SendWithShift("{Left}")
Return

^a::
SendWithShift("{Home}")
Return

^e::
SendWithShift("{End}")
Return
```

## Chords in Hammerspoon

Next I checked if Hammerspoon could [execute chord keys][18].

```lua
local ctrlXActive = false
local hotkeyModal = hs.hotkey.modal.new()

function startCtrlX()
  ctrlXActive = true

  hs.timer.doAfter(1, clearCtrlX)
end

function clearCtrlX()
  print('Clearing ctrlXActive flag.')
  ctrlXActive = false
end

function forwardOrOpen()
  hotkeyModal:exit()
  
  if ctrlXActive then
    print('Opening file.')
    hs.eventtap.keyStroke('cmd', 'o')
  else
    print('Moving cursor forward.')
    hs.eventtap.keyStroke(nil, 'right')
  end

  hotkeyModal:enter()

  clearCtrlX()
end

hotkeyModal:bind({'ctrl'}, 'x', startCtrlX, nil, nil)
hotkeyModal:bind({'ctrl'}, 'f', forwardOrOpen, nil, nil)
hotkeyModal:enter()
```

## I need data structure

Success! The experiments showed me that Hammerspoon and AutoHotkey could emulate Emacs in the way I wanted. Next I needed a data structure to store all the information needed to translate keystrokes.

### Tables in Lua

Hammerspoon is scripted in [Lua][9] a dynamic language similar to Ruby. Associative arrays in Ruby are called hashes and in Lua they are called [tables][12].


```lua
user = {}
user['name'] = 'Yuval'
user['job'] = 'Author'
```

Here is an equivalent Hash in Ruby:

```ruby
user = {}
user[:name] = 'Yuval'
user[:job] = 'Author'
```

Unfortunately Lua does not use the nice JSON style syntax for defining multiple properties such as:

```lua
user = { name: 'Yuval', job: 'Author' }
```

In Lua that would look like:

```lua
user = { ['name'] = 'Yuval', ['job'] = 'Author' }

or

user = { name = 'Yuval', job = 'Author' }
```

Lua also uses tables to create arrays:

```lua
fruits = { 'Apple', 'Banana', 'Orange' }

print(fruits[1])  -- Prints 'Apple' because Lua indexes arrays by 1 not 0.
```

In Ruby that might be:

```ruby
fruits = ['Apple', 'Banana', 'Orange']

puts fruits[0]
```

Tables appeared to be the data structure I needed to store the keystrokes I wanted to translate. 

### Objects in AutoHotkey

In AuotHotKey associative arrays are called [Objects][13].

```pascal
User := {}
User["name"] := "Yuval"
User["job"] := "Author"
```

AHK can use a JSON like syntax as well:

```pascal
User := { name: 'Yuval', job: 'Author' }
```

Similar to Lua AHK uses Object's to define both associative arrays and regular arrays:

```pascal
Fruits := ['Apple', 'Banana', 'Orange']

OutputDebug %Fruits[1]% ; Prints 'Apple' because AHK also uses 1-based array indexes
```

## The keystroke translation data structure

To store a look-up table for keystrokes I started with a Lua table like this:

```lua
local keys = {
  ['ctrl'] = {
    ...
    ['b'] = {nil, 'left'},
    ['w'] = {'cmd', 'x'},
    ...
  }
}
```

Hitting `Ctrl+b` would trigger the left directional keystroke which doesn't require holding a modifier key such as `Ctrl` and hitting `Ctrl+w` would trigger the Mac cut shortcut `Cmd+x`.

This config separates modifiers and regular keys into different array entries because that's how Hammerspoon's API activates keys.

```lua
  hs.eventtap.event.newKeyEvent(mods, key, true):post()
  hs.eventtap.event.newKeyEvent(mods, key, false):post()
```

In AutoHotkey modifiers and regular keystrokes can be intermixed. Here is the the same data structure in AHK:

```c
global keys
:= : {"ctrl"
    : {"b": ["{Left}"]
      ,"w": ["^x", False, ""]}}
```

`Ctrl+w` triggers the Windows cut shortcut `Ctrl+x` which is `^x` in AHK. This syntax obeys the [multi-line continuation][14] rules in AHK making sure to start every line with a character indicating that this data structure will span multiple lines.

## Separating the text selection keys

After starting a text selection with `Ctrl+Space` in Emacs you can navigate around the document to increase or decrease the size of the selection with the navigation shortcuts like `Ctrl+n` (down). In contrast other shortcuts can't work while text is selected such as `Ctrl+k` (delete a line of text). To support text selection my config data needed to differentiate these two types of shortcuts.

```lua
local keys = {
  ['ctrl'] = {
    ...
    ['b'] = {nil, 'left', true},
    ['d'] = {'ctrl', 'd', false},
    ...
  }
}
```

`Ctrl+b` shrinks the current text selection by moving one character backwards so it's marked with `true` as the third config setting. `Ctrl+d` deletes a character at the current cursor position which needs to first deselect the current text selection, so it's marked with `false` as the third config setting.

## Working around limitations with macros

The final additional to the data structure was an option to execute a Macro instead of a keystroke. Here is a macro written in AHK:

```c
,"k": ["", False, "MacroKillLine"]
...

; Macro to kill a line and add it to the clipboard
MacroKillLine()
{
  Send {HOME}{ShiftDown}{END}{ShiftUp}
  Send ^x
  Send {Del}
}
```

This macro tries to emulate how Emacs deletes lines by first selecting the entire line, cutting it and then deleting it.

## Compromising on chords

While I was working my way through the [default Emacs keybindings][10] I noticed some shortcuts in Emacs I'd never thought about.

File file is bound to: `Ctrl-x Ctrl-f` (release Ctrl between typing x and f)

and

Set fill column is bound to: `Ctrl-x f` (hold Ctrl between typing x and f)

These shortcuts preform different actions, but exploit the subtle difference of **letting go of Ctrl** vs **holding down Ctrl**. Should I support these shortcuts in my scripts? After reviewing the handful of Emacs shortcuts that differ in this way I decided to drop support for them in the scripts because I wasn't using them anyway. I was probably typing these shortcuts by accident in Emacs. I decided to disable those shortcuts in Emacs with an [elisp config script][20].

## Task switching

One important shortcut not related editing text is `Alt+Tab` or `Cmd+Tab` (the task switcher). With my scripts complete I noticed that `Alt+Tab` was the only reason my hands were leaving the home-row. Could I rebind these shortcuts to some unassigned Emacs shortcuts and keep my hands on the home-row?

All of the shortcuts ranging from A to Z using `Ctrl` were already taken. What about an unassigned chord keys like `Ctrl-xt`?

I hooked up the `Ctrl+xt` shortcut and immediately hit a problem. On Windows `Alt+Tab` requires holding down the `Alt` key and tapping the `Tab` key to step through each currently running app, but I wanted to hit `Ctrl-xt` and then `Tab` to step through the currently running apps. The code that handled chords in my scripts would not support this kind of shortcut.

On stackoverflow I found other programmers struggling with the same problem in [AHK][15]. One solution on Windows was to use `Alt+Esc` instead of `Alt+Tab`. `Alt+Esc` switches to the **last app** that was opened without a user interface. This was compatible with my script, but in practice I found it confusing to switch app with this shortcut. Trying to script `Alt+Tab` and `Cmd+Tab` seemed like a dead end.

As a last resort I looked for alternative task switchers to the ones built into each operating system. For Windows I found a free open source project [switcheroo][16]. I set `Ctrl+xt` to launch switcheroo instead of `Alt+tab` and everything was peachy. On Mac I took the same approach and purchased a paid app called [Witch 3][17].

## The results

The scripts worked! I using them daily on both Windows and Mac. Switching between operating systems is no longer a pain.

If you wanna give them a try for yourself checkout the [Github repository][11] and happy scripting.

[1]: https://www.microsoft.com/en-ca/store/p/ubuntu/9nblggh4msv6
[2]: http://www.cam.hi-ho.ne.jp/oishi/indexen.html
[3]: https://autohotkey.com/
[4]: http://www.hammerspoon.org/
[5]: https://gist.github.com/dulm/ee5ec47cfd2a71ded0e3841ee04e6ea3
[6]: https://github.com/usi3/emacs.ahk
[7]: https://github.com/justintanner/universal-emacs-keybindings
[9]: https://www.lua.org/
[10]: http://wttools.sourceforge.net/emacs-stuff/emacs-keybindings.html
[11]: https://github.com/justintanner/universal-emacs-keybindings
[12]: https://www.lua.org/pil/2.5.html
[13]: https://autohotkey.com/docs/Objects.htm
[14]: https://autohotkey.com/docs/Scripts.htm#continuation
[15]: https://stackoverflow.com/questions/35971452/what-is-the-right-way-to-send-alt-tab-in-ahk
[16]: http://www.switcheroo.io/
[17]: https://manytricks.com/witch/
[18]: https://gist.github.com/justintanner/272aa3a9b5834a1e4027aaaa3702efbe
[19]: https://gist.github.com/justintanner/f31d2ee2ac785401a7d65160087bb995
[20]: https://gist.github.com/justintanner/05790ad55325408ff70632ca4ddd9f5f

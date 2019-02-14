# Mac

## Keys
⌘ – Command Key –––––– \&#x2318; – \&#8984;<br>
⌥ – Option Key ––––––– \&#x2325; – \&#8997;<br>
⇧ – Shift Key –––––––– \&#x21E7; – \&#8679;<br>
⎋ – ESC Key –––––––––– \&#x238B; – \&#9099;<br>
⇪ – Capslock ––––––––– \&#x21ea; – \&#8682;<br>
⏎ – Return ––––––––––– \&#x23ce; – \&#9166;<br>
⌫ – Delete/Backspace – \&#x232b; – \&#9003;<br>
^ – Control Key

## Shortcuts
Open keyboard shortcuts<br>
**Shift + ?**<br><br>
Force Quit Applications<br>
**option + command + Esc**<br><br>
Clipboard<br>
**Cmd-Shift-V**       # Paste Previous<br>
**Cmd-Option-V**      # Paste Next<br>
**Cmd-Option-Ctrl-V** # Show History<br><br>
Chrome<br>
**Shift + Fn + Delete**   # remove input fields auto-suggest<br><br>
Textmate<br>
**Control + Shift + T**   # gives all the list of TODO and FIXME<br>
**Control + G**           # case change<br>
**Command + K**          # git diff(custom)<br>
**Command + T**           # search files<br><br>
Multiple word select<br>
**Ctrl + W**<br><br>

## Commands
```sh
# Hide/Show login users
# https://support.apple.com/en-us/HT203998
sudo dscl . create /Users/postgres IsHidden 1
sudo dscl . create /Users/postgres IsHidden 0

# Flush Cache
dscacheutil -flushcache

# Show hidden files
defaults write com.apple.Finder AppleShowAllFiles YES

# disable spotlight
sudo mdutil -a -i off
```

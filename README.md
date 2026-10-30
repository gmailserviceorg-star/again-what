# ClaudeOS (bare metal, stage 1)

Boots from GRUB (multiboot) into a 32-bit kernel written in C. No libc, no host OS.

## Build and boot
Needs Linux (or WSL) with: `sudo apt install build-essential grub-pc-bin xorriso mtools`
    ./build-iso.sh          # makes ClaudeOS.iso
    ./run-qemu.sh           # optional: QEMU with a blank 64 MB disk for persistence
VirtualBox / VMware: new VM, Other 32-bit, 256 MB RAM, BIOS (not EFI), attach ClaudeOS.iso,
and add a blank IDE/SATA(IDE-mode) virtual disk if you want files to survive reboots.
Login: claude / claude.

## What is implemented
- GRUB boot, VBE framebuffer graphics (double buffered, alpha, rounded corners), 8x16 font
- Drivers: PS/2 keyboard, PS/2 mouse, PIT timer, CMOS RTC, ATA PIO disk, ACPI/emulator power-off, 8042 reset
- Boot log (real hardware facts), login, desktop, taskbar, start menu, draggable windows
- Smooth animations: open, close, maximize/restore, timings read from System/Configuration/Windows.json ("animation" line)
- Filesystem: folders and files, recursive delete, Delete key on desktop icons and in Files
- Config files read at boot: Theme.json, Taskbar.json, Windows.json, Users.json
- Apps: Terminal (about 25 commands incl. exec del long/short forms + PRIVILEGE.json), Files, Editor, Claudebrowse
- Claudebrowse: own HTML tag parser/renderer (headings, paragraphs, lists, bold, links, pre, entities) and a search engine that ranks every page in /www
- Shift+F10 admin terminal, Shift+F11 reset password, Shift+F12 recovery (repair, restore config, factory reset, power), Ctrl+X shutdown, hold Ctrl+X 2 s restart
- Persistence: file system saved to the first ATA disk that is blank or already ClaudeOS-formatted (never touches a disk holding another OS)

## NOT implemented yet (be aware)
- No network stack, so Claudebrowse only opens local pages (/www)
- No JavaScript engine and no CSS engine in the browser
- No USB, sound, GPU acceleration, SMP, 64-bit mode
- Customization depth, Recovery.spm/kernel.spm/.ssh binary formats and the data/log files from the HTML version are not ported yet
- Only a subset of the HTML version's apps and settings exist
- Not yet booted in a real VM by the author (compiled and logic-tested on a host only)

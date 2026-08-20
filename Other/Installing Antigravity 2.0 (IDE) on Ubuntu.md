Tried to install Google Antigravity 2.0 on Linux but it's there is no documentation on the official website (Surprise, coz most of their eng do day to day is documentation).

What you download is a portable Electron build (`Antigravity-x64`) extracted from the official tar.gz. No package manager / installer is provided; Here is every step to make it launchable from the desktop application menu on a Linux/x86_64 host.

---

## 1. Steps performed

### 1.1 Extract the files

cd ~/Downloads
tar -xvzf Antigravity.tar.gz

### 1.2 Move to /opt and fix the sandbox helper

sudo mv ~/Downloads/Antigravity-x64 /opt/antigravity
sudo chown root:root /opt/antigravity/chrome-sandbox
sudo chmod 4755     /opt/antigravity/chrome-sandbox

Verification:

$ stat -c '%U %G %a %n' /opt/antigravity/chrome-sandbox
root root 4755 /opt/antigravity/chrome-sandbox

Why `/opt`: conventional FHS location for self-contained third-party applications; not subject to `~/Downloads` cleanup; readable by all users.

### 1.3 Extract the application icon from app.asar

`app.asar` is a custom Electron archive; `tar`/`unzip` cannot read it. Used the official `asar` CLI via `npx` (node was already on PATH via nvm):

# Confirm an icon exists inside the asar
npx --yes asar list /opt/antigravity/resources/app.asar | grep -i icon
#   /icon.png
#   /trayTemplate.png
#   /trayTemplate@2x.png

# Pull the main icon out
cd /tmp
npx --yes asar extract-file /opt/antigravity/resources/app.asar icon.png
file /tmp/icon.png
#   PNG image data, 512 x 512, 8-bit/color RGBA, non-interlaced

### 1.4 Install the icon

Per-user (no sudo) install under `~/.local`, indexed by the freedesktop spec.

mkdir -p ~/.local/share/applications \
         ~/.local/share/icons/hicolor/512x512/apps

# Icon in the standard hicolor theme path
mv /tmp/icon.png ~/.local/share/icons/hicolor/512x512/apps/antigravity.png

### 1.5 Desktop menu entry

File: `~/.local/share/applications/antigravity.desktop`

[Desktop Entry]
Name=Antigravity
Comment=Google Antigravity IDE
Exec=/opt/antigravity/antigravity %U
Terminal=false
Type=Application
Icon=antigravity
Categories=Development;IDE;
StartupNotify=true
StartupWMClass=Antigravity
MimeType=x-scheme-handler/antigravity;

Notes:

- `Icon=antigravity` is the _theme name_, not a path — the file dropped in step 3.3 resolves it.
    
- `%U` lets the app receive URLs (useful for any deep-link / OAuth callback).
    
- `MimeType=x-scheme-handler/antigravity;` registers a custom-scheme handler so the system can route `antigravity://...` URLs to this binary. Remove the line if you don't want that.
    
- `StartupWMClass=Antigravity` lets the taskbar pair the window with the launcher icon.
    

### 1.6 Refresh the desktop databases and validate

chmod +x ~/.local/share/applications/antigravity.desktop
update-desktop-database ~/.local/share/applications
gtk-update-icon-cache -t ~/.local/share/icons/hicolor
desktop-file-validate ~/.local/share/applications/antigravity.desktop
# (no output = valid)

---

## 2. How to run

- App menu: search "Antigravity"
    
- Custom scheme: `xdg-open antigravity://...`
    

If the menu entry does not appear immediately on GNOME/KDE, log out and back in once, or run `gtk-launch antigravity.desktop`.

---

## 3. Update workflow

The bundled auto-updater talks to Google's update endpoint (see `resources/app-update.yml`). If that fails or you prefer manual updates, replace the install with a fresh tarball:

# fetch + extract the new tarball into ~/Downloads/Antigravity-x64
sudo rm -rf /opt/antigravity
sudo mv ~/Downloads/Antigravity-x64 /opt/antigravity
sudo chown root:root /opt/antigravity/chrome-sandbox
sudo chmod 4755     /opt/antigravity/chrome-sandbox

The `.desktop` file survive the swap because they only reference the install root, which is unchanged.

If a new release changes the icon, re-run step 3.2 and overwrite `~/.local/share/icons/hicolor/512x512/apps/antigravity.png`, then `gtk-update-icon-cache -t ~/.local/share/icons/hicolor`.

---

## 4. Uninstall

sudo rm -rf /opt/antigravity
rm  ~/.local/share/applications/antigravity.desktop
rm  ~/.local/share/icons/hicolor/512x512/apps/antigravity.png
update-desktop-database ~/.local/share/applications
gtk-update-icon-cache  -t ~/.local/share/icons/hicolor

Per-user state (settings, sign-in, caches) lives under:

- `~/.config/Antigravity/` — user config / preferences
    
- `~/.cache/Antigravity/` — caches
    
- `~/.config/antigravity-updater/` — auto-updater state
    

Remove those too for a full wipe.

---

## 5. Troubleshooting

- `SUID sandbox helper ... not configured correctly` — repeat step 1.1. This recurs if the `chmod 4755` / `chown root` was not applied, or if the tree was copied over a filesystem that strips setuid bits (e.g. `vfat`, `exfat`, or a mount with `nosuid`). `/opt` on the root ext4 filesystem is safe.
    
- **Blank window / GPU crash on first launch** — try `antigravity --disable-gpu` once to confirm; if that works, the issue is the host's GPU stack, not Antigravity.
    
- **Icon never appears in menu** — verify with `desktop-file-validate ~/.local/share/applications/antigravity.desktop` and confirm `XDG_DATA_DIRS` includes `~/.local/share`. On most distros `update-desktop-database` plus a fresh login is enough.
    

---

## 6. File inventory after install

|Path|Owner|Purpose|
|---|---|---|
|`/opt/antigravity/`|`xliu:xliu`|App root|
|`/opt/antigravity/antigravity`|`xliu:xliu`|Main Electron binary|
|`/opt/antigravity/chrome-sandbox`|`root:root`|SUID sandbox helper, mode `4755`|
|`~/.local/share/applications/antigravity.desktop`|`xliu:xliu`|Desktop entry|
|`~/.local/share/icons/hicolor/512x512/apps/antigravity.png`|`xliu:xliu`|Icon, extracted from `app.asar`|

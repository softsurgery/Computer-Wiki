You can use `w.setMenu(null)` or set `frame: false` (this also removes buttons for close, minimize and maximize options) on your window. See [setMenu()](http://electron.atom.io/docs/api/browser-window/#winsetmenumenu-linux-windows) or [BrowserWindow()](http://electron.atom.io/docs/api/browser-window/). Also check this [thread](https://github.com/electron/electron/issues/1415)

---

Electron now has [`win.removeMenu()`](https://electronjs.org/docs/api/browser-window#winremovemenu-linux-windows) (_added in v5.0.0_), to remove application menus instead of using `win.setMenu(null)`.

---

Electron 7.1.x seems to have a bug where `win.removeMenu()` doesn't work. The only workaround is to use `Menu.setApplicationMenu(null)`, however, this will disable all the menu shortcuts like F11 for toggling fullscreen etc.

---

In new versions of Electron, you can set `autoHideMenuBar: true` while creating browserWindow, pressing Alt will show the menu bar again.

```javascript
const mainWindow = new BrowserWindow({
  autoHideMenuBar: true,
})
```
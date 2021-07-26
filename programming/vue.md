# Vue + Electron

### Set up

1. `npm install -g @vue/cli` - install vue cli
2. `vue create my-app` - create a vue project
3. `vue add electron-builder` - install electron builder

### Disabling menu

1. Hide the menu with File, Edit, Help, etc. items:

```javascript
    win = new BrowserWindow({...});

    Menu.setApplicationMenu(null); // At this point, F12 and Ctrl + Alt + I will not work anymore to open the developer tools.
```

2. Manually create a shortcut that opens the developer tools:

```javascript
app.on("ready", async () => {
    if (isDevelopment && !process.env.IS_TEST) {
        // ... some code about installing dev tools

        // Cannot use just F12 as electron doesn't allow registering the same keybinding
        // as the one browswer already has registered
        globalShortcut.register("CommandOrControl+F12", () => {
            win.webContents.toggleDevTools();
        });
    }
});
```

# electron-window-state [![Build Status](https://travis-ci.org/mawie81/electron-window-state.svg)](https://travis-ci.org/mawie81/electron-window-state)

> A library to store and restore window sizes and positions for your
[Electron](http://electron.atom.io) app

*Heavily influenced by the implementation in [electron-boilerplate](https://github.com/szwacz/electron-boilerplate).*

## Install

```
$ npm install --save electron-window-state
```

## Usage

```js
const windowStateKeeper = require('electron-window-state');
let win;

app.on('ready', function () {
  // Load the previous state with fallback to defaults
  let mainWindowState = windowStateKeeper({
    defaultWidth: 1000,
    defaultHeight: 800
  });

  // Create the window using the state information
  win = new BrowserWindow({
    'x': mainWindowState.x,
    'y': mainWindowState.y,
    'width': mainWindowState.width,
    'height': mainWindowState.height
  });

  // Maximize the window if it was maximized last time
  if (mainWindowState.isMaximized) {
    win.maximize();
  }

  // Let us register listeners on the window, so we can update the state
  // automatically (the listeners will be removed when the window is closed)
  mainWindowState.register(win);
});
```

## API

#### windowStateKeeper(opts)

Note: Don't call this function before the `ready` event is fired.

##### opts

`defaultWidth` - *Number*

  The width that should be returned if no file exists yet. Defaults to 800.

`defaultHeight` - *Number*

  The height that should be returned if no file exists yet. Defaults to 600.

`path` - *String*

  The path where the state file should be written to. Defaults to
  `app.getPath('userData')`

`file` - *String*

  The name of file. Defaults to `window-state.json`

### state object

```js
const windowState = windowStateKeeper({
  defaultWidth: 1000,
  defaultHeight: 800
});
```

`x` - *Number*

  The saved `x` coordinate of the loaded state. `undefined` if the state has not
  been saved yet.

`y` - *Number*

  The saved `y` coordinate of the loaded state. `undefined` if the state has not
  been saved yet.

`width` - *Number*

  The saved `width` of loaded state. `defaultWidth` if the state has not been
  saved yet.

`height` - *Number*

  The saved `heigth` of loaded state. `defaultHeight` if the state has not been
  saved yet.

`isMaximized` - *Boolean*

  `true` if the window state was saved while the the window was maximized.
  `undefined` if the state has not been saved yet.

`register(window)` - *Function*

  Register listeners on the given `BrowserWindow` for events that are
  related to size or position changes (`resize`, `move`). When the window is
  closed we automatically remove the listeners and save the state.

`unregister()` - *Function*

  Unregister listeners that were previously registered with `register`. You
  usually don't need to call this, since it's taken care of automatically when
  the window is closed.

`saveState(window)` - *Function*

  Saves the current state of the given `BrowserWindow`. This exists mostly for
  legacy purposes, and in most cases it's better to just use `register`.

## License

MIT © [Marcel Wiehle](http://marcel.wiehle.me)

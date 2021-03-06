# Electron-updater

## Feature
- same api like [electron-userland/electron-builder](https://github.com/electron-userland/electron-builder/tree/master/packages/electron-updater)
- do not need code siging for mac
- platform
  - Mac (zip)
  - Windows (exe)
  - Linux (AppImage)
- methods
  - github release
  - custom
- node >= 8

## Usage
````js
import autoUpdater from '@suen/electron-updater'

/* when use github release */
const update = autoUpdater({
    type: 'github',
    options: {
        username: 'your github username',
        repo: 'your repo',
        log: true // will emit log event
    }
})
/* when use custorm url */
const update = autoUpdater({
    type: 'custom',
    options: {
        url: 'you custom url', // need https
        log: true // will emit log event
    }
})
/* when use custorm getRemoteLatest function */
const update = autoUpdater({
    type: 'custom',
    options: {
        getRemoteLatest() {
            return new Promise((resolve) => {
                this.latestRelease.linux = 'linux download url'
                this.latestRelease.osx = 'osx download url'
                this.latestRelease.windows = 'windows download url'
                this.latestVersion = 'v1.0.0' // it decides your downloaded file name, will be helpful on linux
                this.emit('log', this.latestRelease)
                resolve(100000000) // your version num, must be an number
            })
        },
        log: true // will emit log event
    }
})


update.checkForUpdatesAndNotify()
````

## Event
- checking-for-update
- update-available
- update-not-available
- download-progress
- error
- log

example:
````js
update.on('checking-for-update', () => {
    mainWindow.webContents.send('checking-for-update')
})

update.on('update-available', () => {
    mainWindow.webContents.send('update-available')
})

update.on('update-not-available', () => {
    mainWindow.webContents.send('update-not-available')
})

update.on('download-progress', (percent) => {
    mainWindow.webContents.send('download-progress', `${percent}%`)
})

update.on('error', (e) => {
    mainWindow.webContents.send('error', e)
})

update.on('log', (log) => {
    mainWindow.webContents.send('log', log)
})
````
# New: Instructions to install better-sqlite3

This project uses a library submodule, [dent-table-sqlite](https://github.com/dent-table/dent-table-sqlite), to handle sqlite database operations. 
This library uses the native module `better-sqlite3` as peer dependency. So, projects that use `dent-table-sqlite` must provide `better-sqlite3` as a dependency both for development and production builds.

This project declare `better-sqlite3` as app dependency (in `app/package.json`).
Then, specific scripts in `package.json` and custom build configurations in `electron-builder.json` builds the native module against correct node (and electron) version (and include it in dev and production builds).

On Windows, this requires that python3 and Visual Studio BuildTools are correctly installed on system. 
You can easily install both with [windows-build-tools](https://github.com/felixrieseberg/windows-build-tools).

In case of issues, you can manually rebuild `better-sqlite3` by run `npm run sqlite:build` or by run this command:
```shell
electron-rebuild -f -w better-sqlite3 -m ./app
```
# OLD (2019): Instructions to install better-sqlite3
(From [here](https://github.com/JoshuaWise/better-sqlite3/issues/126))

Follow this steps **starting from project root directory**:
- `npm install -g node-gyp`
- `node-gyp --version` <br /> If it fails with 'node-gyp.js not found' go to `%ProgramFiles%\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\node-gyp.cmd` and remove ..\node_modules\ .. on line 2. 

- Follow the instructions on https://github.com/nodejs/node-gyp 
<br/> 
**NB**: node-gyp has some issues with Visual Studio 2019 (see [here](https://github.com/nodejs/node-gyp/issues/1663)). Install Visual Studio BuildTools **2017** from [here](https://visualstudio.microsoft.com/it/thank-you-downloading-visual-studio/?sku=BuildTools&rel=15). 
<br/>
Include also build tools v140. (**<- This will not be necessary if you use windows-build-tools**)
<br>
**NB (2021)**: Now node-gyp works also with Visual Studio 2019
  
- `npm install --save-dev electron-rebuild`
- `npm install --save better-sqlite3`


- go to terminal in the ide and run
  ``` bash
  $ set PYTHON=/path/to/python3/exe
  $ cd app
  $ electron-rebuild -f -w better-sqlite3
  ``` 

# OLD: Instructions to build sqlite3 with Electron

## Windows
- `npm install -g node-gyp`
- `node-gyp --version` <br /> If it fails with 'node-gyp.js not found' go to `%ProgramFiles%\nodejs\node_modules\npm\node_modules\npm-lifecycle\node-gyp-bin\node-gyp.cmd` and remove ..\node_modules\ .. on line 2. 

- Follow the instructions on https://github.com/nodejs/node-gyp 
<br/> 
**NB**: node-gyp has some issues with Visual Studio 2019 (see [here](https://github.com/nodejs/node-gyp/issues/1663)). Install Visual Studio BuildTools **2017** from [here](https://visualstudio.microsoft.com/it/thank-you-downloading-visual-studio/?sku=BuildTools&rel=15). 
<br/>
Include also build tools v140. (**<- This will not be necessary if you use windows-build-tools**)

- `npm install electron-rebuild --save-dev`

- `npm install sqlite3 --save`

- add to your `package.json` - scripts following:<br />
``` bash 
"postinstall": "cd node_modules\sqlite3 && npm install && npm run prepublish && node-gyp --python C:\Progs\Python27\python.exe configure --module_name=node_sqlite3 --module_path=../lib/binding/electron-v4.0-win32-x64 && node-gyp --python C:\Progs\Python27\python.exe rebuild --target=4.0.0 --arch=x64 --target_platform=win32 --dist-url=https://atom.io/download/atom-shell --module_name=node_sqlite3 --module_path=../lib/binding/electron-v4.0-win32-x64"
```
**NB**: Update Python 2.7 exe path and module_path with the version found in node_modules\sqlite3\lib\binding folder. Update also the correct `target` version. You will find the `target` version of your Electron app in the `package.json` file in the root folder of your Electron directory, e.g: `"electron": "4.0.0"`.

If `npm run prepublish` fails remove it.

- `npm run postinstall`


##Linux
See [here](https://gist.github.com/craigvantonder/f59277cd788f8aa755e3bdbe5d21f08e#file-electron-sqlite3-md)

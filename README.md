Mad's original NPM Task Runner now supports Yarn, so choose [NPM Task Runner](https://visualstudiogallery.msdn.microsoft.com/8f2f2cbc-4da5-43ba-9de2-c9d08ade4941) for a better experience

# Yarn Task Runner (Deprecated - Prefer NPM Task Runner)

Mad's NPM Task Runner forked and changed to use Yarn instead.


Download the extension at the
[VS Gallery](https://visualstudiogallery.msdn.microsoft.com/c9f7667c-ac1f-4be8-82c6-9a4e34fef47b)

---------------------------------------------------------

Adds support for yarn scripts defined in package.json 
directly in Visual Studio's Task Runner Explorer.

## Current Known Limitations

- Task Runner Explorer seems to not allow more than one extension to target the same file (in this case the `package.json` file). As such, It won't load if [NPM Task Runner](https://visualstudiogallery.msdn.microsoft.com/8f2f2cbc-4da5-43ba-9de2-c9d08ade4941) is installed
- There's a limitation in one of Yarn's files (at 0.15.1) - `base-reporter.js` - that attempts to access stdin under IISNODE, which is inexistent. To work around this, you currently need to edit the installed `base-reporter.js` file at `%appData%\npm\node_modules\yarn\lib\reporters\base-reporte.js`. At that file, around line 55 change:

```
this.stdout = opts.stdout || process.stdout;
this.stderr = opts.stderr || process.stderr;
this.stdin = opts.stdin || process.stdin;
this.emoji = !!opts.emoji;
```

to

```
this.stdout = opts.stdout || process.stdout;
this.stderr = opts.stderr || process.stderr;

//HACK!!!!
if(process.platform !== 'win32' && !process.env.IISNODE_VERSION)
    this.stdin = opts.stdin || process.stdin;

this.emoji = !!opts.emoji;
```

this situation has been reported to the yarn team and is a pull-request on their repo at [https://github.com/yarnpkg/yarn/pull/1000](https://github.com/yarnpkg/yarn/pull/1000) 



## yarn scripts

Inside package.json it is possible to add custom scripts inside
the "scripts" element.

```js
{
	"name": "test",
	"version": "1.0.0",
	"scripts": {
		"watch-test": "mocha --watch --reporter spec test",
		"build-js": "browserify -t reactify app/js/main.js | uglifyjs -mc > static/bundle.js"
	}
}
```



## Task Runner Explorer
Open Task Runner Explorer by right-clicking the `package.json`
file and select **Task Runner Explorer** from the context menu:

![Open Task Runner Explorer](art/open-trx.png)

### Execute scripts (untested)
When scripts are specified, the Task Runner Explorer
will show those scripts.

![Task list](art/task-list.png)

Each script can be executed by double-clicking the task.

![Console](art/console.png)

### Verbose output (untested)
A button for turning verbose output on and off is located
at the left toolbar.

![Verbose Output](art/verbose-output.png)

The button is a toggle button that can be left
on or off for as long as needed.

### Bindings (untested)
Script bindings make it possible to associate individual scripts
with Visual Studio events such as "After build" etc.

![Visual Studio bindings](art/bindings.png)

## Intellisense (untested)

If you manually edit bindings in package.json, then full
Intellisense is provided.

![Visual Studio Intellisense](art/intellisense.png)

## License
[Apache 2.0](LICENSE)

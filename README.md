# grunt-war

> Pure JavaScript implementation for creating a WAR of your project for deployment on a JVM servlet container. Enjoy!

## Getting Started
This plugin requires Grunt `~0.4.2`

```shell
npm install grunt-war --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-war');
```

[Then sip on some @AnodyneCoffee.](https://anodynecoffee.com)
Enjoy!

## The "war" task

### Overview
In your project's Gruntfile, add a section named `war` to the data object passed into `grunt.initConfig()`.

```js
grunt.loadNpmTasks('grunt-war');

grunt.initConfig({

      /*
       * Build a WAR (web archive) without Maven or the JVM installed.
       */
      war: {
        target: {
          options: {
            war_dist_folder: '<%= build_dir %>',
            war_verbose: true,
            war_name: 'webmagic',
            webxml_welcome: 'index.html',
            webxml_display_name: 'Web Magic',
            webxml_mime_mapping: [ 
		{ 
			extension: 'woff', 
		  	mime_type: 'application/font-woff' 
		} ]
          },
          files: [
            {
              expand: true,
              cwd: '<%= build_dir %>',
              src: ['**'],
              dest: ''
            }
          ]
        }
      }
});
```

### Options

#### options.war_dist_folder
Type: `'string'`
Default value: `'test'`

This is the folder that the war will be placed. This folder has to exist before this task is run.

#### options.war_name
Type: `'string'`
Default value: `'grunt'`

#### options.war_extras
Type: `'Array'`
Default value: `[]`

A list of files and folders entries that are to be included in the war. Each object in the array has
keys `filename` and `data` example: `{ filename: 'name_of_file.ext', data: file_data }`.  If the key `data`
is omitted then an empty folder called `filename` will be added to the WAR. The value of key  `data` can
either be a `string` or a function that returns a `string`.

#### options.war_verbose
Type: `'boolean'`
Default value: `false`

Logs progress to the grunt console log.

#### options.war_compression
Type: `'string'`
Default value: `'DEFLATE'`

Compress ('DEFLATE') or leave uncompressed ('NONE').

#### options.webxml
Type: `'Function'`
Default value: Normally omitted. Only provide if you want absolute control over the format and contents of the web.xml.
If you specifify this option the other `options.webxml_XXX` will have no effect if specified.

Example 1

````js
     /* Return a string that will become the complete contents of the web.xml */
     webxml: function (opts) { 
                return 'string containing contents of web.xml'; 
             },
````

Example 2

````js
     /* Return the contents of a file which will become the web.xml */
     webxml: function (opts) { 
                var fs = require('fs'); 
                return fs.readFileSync(pathToFileWithContentsOfHardCodedWebXML, 'binary'); 
             },
````

#### options.webxml_welcome
Type: `'string'`
Default value: `'index.html'`

#### options.webxml_display_name
Type: `'string'`
Default value: `'Grunt WAR'`

#### options.webxml_mime_mapping
Type: `'Array'`
Default value: `[]`

An array of objects with properties `extension` and `mime_type`.

#### options.webxml_webapp_extras
Type: `'Array'`
Default value: `[]`

An array of objects that are either `'string'` or `'function'` that return `'string'`.  These entries are
included directly into the generated web.xml.
    
    war_extras: [ {filename: 'grunt-war-credits.txt', data: 'This line will appear in the file!\n'} ]

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [Grunt](http://gruntjs.com/).

## Release History

#### 0.3.2
* If the war_dest_folder does not already exist then grunt-war will create it.  This saves the pain of adding this check before calling grunt-war. (skray)

#### 0.3.1 
* Added `options.webxml` so that you may provide the raw contents of the web.xml when necessary. Requested by MartinDoyleUK.

````js
     /* Example of how to return the contents of a file which will become the web.xml */
     webxml: function (opts) { 
                var fs = require('fs'); 
                return fs.readFileSync(pathToFileWithContentsOfHardCodedWebXML, 'binary'); 
             },
````

#### 0.3.0 (Breaking changes)
* This release allows for more flexible output paths inside the war bundle using Grunt's built-in dest write instead of the prior custom and less flexible rewrites. If you want the same behavior as in prior releases that did not depend on the 'dest' configuration then you must modify the task to something similiar to the following snippet.  (shcarrico)

````js
    ...
    war: {
      target: {
        options: { ... },
        files: [
            {
              expand: true,
              cwd: '<%= build_dir %>',
              src: ['**'],
              dest: ''  
            }
        ]
      }
    },
    ...
````

#### 0.2.7
* Fixed the inclusion of source files defined in previous WAR task configurations for all future WAR tasks. Example: a war.foo task includes dist/foo in foo.war, and a war.bar task includes dist/bar in bar.war. bar.war would include both dist/bar and the previously added dist/foo. (jbenner)

#### 0.2.6

* Fixed grunt-war encoding text incorrectly. Example: 'Količina' and 'Osveži' would get encoded as 'KoliÄina' and 'OsveÅ¾i'. (sobrle)

#### 0.2.5

* Updated peerDependencies to include node-zip. (@augier)

#### 0.2.4

* Now deletes `options.war_name` from `options.war_dist_folder` before trying to generate a new `war` file.

#### 0.2.3

* Fixed file names being trimmed when included in war. (@augier)

#### 0.2.1

* Fixed deployment issue resulting from using wrong option when generating webapp tag.

#### 0.2.0

* Renamed `options.war_filename` to `options.war_name`.
* Added `options.war_extras`
* Added `options.webxml_webapp_extras`.

#### 0.1.4 Initial

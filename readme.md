# FFbinaries downloader

[![NPM Version][npm-img]][npm-url]
[![NPM Downloads][npm-dl-img]][npm-url]

[npm-url]: https://npmjs.org/package/ffbinaries
[npm-img]: https://img.shields.io/npm/v/ffbinaries.svg
[npm-dl-img]: https://img.shields.io/npm/dm/ffbinaries.svg


Downloads a precompiled ffmpeg binaries from http://ffbinaries.com.

This module is meant to be used programatically, i.e. as a build step.


# Important info

**This project is not stable just yet.**

**Version 0.0.11 fixes a critical bug with broken zip files being saved. Please update.**

I noticed quite a few downloads of this in the first day already.
Please be aware that this is just something I started last night
and I'm still introducing many breaking changes before I decide how this should
work.

Please raise issues on GitHub or even pull requests for features you'd like to see.
I also just created ffbinaries.com at the same time as this module
so there will be changes in data structure as well.

Documentation may also be slightly inaccurate at the moment - apologies,
if in doubt please refer to the code for now.

Currently the only version of ffmpeg available is 3.2.

## New API

New link structure introduced in version 0.0.6, please allow few hours for DNS updates
if you're having issues.

To see if you're connecting to new API already go to http://ffbinaries.com and see
if you see a raw JSON data file or an actual webpage.

New API is running this: https://github.com/vot/ffbinaries-api
The old API was literally a single static JSON file so this is a slight improvement.


# Usage

You can run it from your code or through CLI.

If `output` argument is specified the binary will be placed there.
In CLI it will default to current working directory.
Programatically the default is `bin/{platform}` folder inside of your copy of ffbinaries.

If `platform` argument is missing the binary for current platform will be downloaded.

Currently only archives with binaries are downloaded without
extracting them yet (this will happen in future versions).


## CLI

When installed globally with `npm i ffbinaries -g` this module will register
itself on command line interface.

### Arguments

CLI uses the following syntax: `ffbinaries {platform} {--output=dir} {--quiet}`

### Examples

```
ffbinaries
ffbinaries linux-64
ffbinaries linux-64 --version=3.2 --output=/home/user/ffmpeg --quiet
```

There are also `ffbinaries help`, `ffbinaries versions` and `ffbinaries clearcache`.


## Programatically

### Methods

`ffbinaries` library exports three methods: `get`, `getData` and `detectPlatform`.

`downloadFiles(platform, output, callback)` method will download the requested binaries.

`getVersionData(version, callback)` fetches the full data set without downloading any binaries.

`listVersions(callback)` returns the list of available versions in the API

`listPlatforms()` returns the list of available platforms

`detectPlatform()` returns the platform code of the machine as detected by the module.

`resolvePlatform(input)` resolves input to a platform code (matches aliases).

`clearCache()` purges local cache


### Example

```
var ffbinaries = require('ffbinaries');
var platform = ffbinaries.detectPlatform();
var output = __dirname;
ffbinaries.downloadFiles(platform, output, function () {
  console.log('Download complete.');
});
```


# Platforms

Platform is the first argument. The following builds are available.

## Windows
* windows-32 (aliases: win, windows, win-32)
* windows-64 (alias: win-64)

## Linux
* linux-32 (alias: linux)
* linux-64
* linux-armhf (alias: linux-arm)
* linux-armel

## OS X
* osx-64 (aliases: mac, osx, mac-64)


# Components

The components of the ffmpeg suite are: ffmpeg, ffprobe, ffserver and ffplay.

|          | Mac | Linux | Windows |
|----------|-----|-------|---------|
| ffmpeg   | v   | v     | v       |
| ffprobe  | v   | v     | v       |
| ffserver | v   | v     |         |
| ffplay   | v   |       | v       |

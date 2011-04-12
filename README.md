# Fluent ffmpeg-API for node.js
This library abstracts the complex command-line usage of ffmpeg into a fluent, easy to use node.js module. In order to be able to use this module, make sure you have [ffmpeg](http://www.ffmpeg.org) installed on your system (including all necessary encoding libraries like libmp3lame or libx264)
## Installation
Via npm:
`$ npm install fluent-ffmpeg`

Or as a submodule:
`$ git submodule add git://github.com/schaermu/node-fluent-ffmpeg.git`
## Usage
You find a lot of usage examples (including a real-time streaming example using [flowplayer](http://www.flowplayer.org) and [express](https://github.com/visionmedia/express)!) in the `examples` folder.
### Simple conversion using preset
This example loads up a predefined preset in the preset folder (currently, fluent-ffmpeg ships with presets for DIVX, Flashvideo and Podcast conversions)

    var ffmpeg = require('fluent-ffmpeg');

    var proc = new ffmpeg('/path/to/your_movie.avi')
      .usingPreset('podcast')
      .saveToFile('/path/to/your_target.m4v', function(retcode, error){
        console.log('file has been converted succesfully');
      });
### Conversion using chainable API
Using the chainable API, you are able to perform any operation using FFMPEG. the most common options are implemented using methods, for more advanced usages you can still use the `addOption(s)` method group.

    var ffmpeg = require('fluent-ffmpeg');
    
    var proc = new ffmpeg('/path/to/your_movie.avi')
      .withVideoBitrate(1024)
      .withVideoCodec('divx')
      .withAspectRatio('16:9')
      .withFps(24)
      .withAudioBitrate('128k')
      .withAudioCodec('libmp3lame')
      .withAudioChannels(2)
      .addOption('-vtag', 'DIVX')
      .toFormat('avi')
      .saveToFile('/path/to/your_target.avi', function(retcode, error){
        console.log('file has been converted succesfully');
      });
### Creating thumbnails from a video file
One pretty neat feature is the ability of fluent-ffmpeg to generate any amount of thumbnails from your movies. The screenshots are taken at automatically determined timemarks using the following formula: `(duration_in_sec * 0.9) / number_of_thumbnails`.

    var ffmpeg = require('fluent-ffmpeg');
    
    var proc = new ffmpeg('/path/to/your_movie.avi')
      .withSize('150x100')
      .takeScreenshots(5, '/path/to/thumbnail/folder', function(err) {
        console.log('screenshots were saved')
      });
### Reading video metadata
Using a seperate object, you are able to access various metadata of your video file.

    var ffmpegmeta = require('fluent-ffmpeg').Metadata;
    
    // make sure you set the correct path to your video file
    ffmpegmeta.get('/path/to/your_movie.avi', function(metadata) {
      console.log(require('util').inspect(metadata, false, null));
    });
## Creating a custom preset
To create a custom preset, you have to create a new file inside the `lib/presets` folder. The filename is used as the preset's name ([presetname].js). In order to make the preset work, you have to export a `load` function using the CommonJS module specifications:

    exports.load = function(ffmpeg) {
      // your custom preset code
    }

The `ffmpeg` parameter is a full fluent-ffmpeg object, you can use all the chaining-goodness from here on. For a good example for the possibilities using presets, check out `lib/presets/podcast.js`.

## Contributing
Contributions in any form are highly encouraged and welcome! Be it new or improved presets, optimized streaming code or just some cleanup. So start forking!

## License
(The MIT License)

Copyright (c) 2011 Stefan Schaermeli &lt;schaermu@gmail.com&gt;

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
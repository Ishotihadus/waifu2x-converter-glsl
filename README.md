 waifu2x-converter-glsl
========================
Original by うえした (@ueshita)

Modification of [waifu2x-converter-cpp](https://github.com/WL-Amigo/waifu2x-converter-cpp) to handle computation on the GPU using OpenGL 3+.

Building outputs a `waifu2x-converter-glsl` command-line utility that *should* be compatible with existing `waifu2x-converter` GUI frontends.

 Requirements
--------------

 - CMake 3.1.0+
 - A C++11 capable compiler
 - OpenCV dev files
 - `libepoxy` dev files
 - OpenGL 3.1 should work; confirmed to work on:
   - Intel HD Graphics 4000 on Linux/Mesa 17.1.6

 Usage
-------

As mentioned above, this is a command line tool. Open any terminal emulator on Linux/BSD/Mac, or `cmd.exe`/Powershell on Windows.

Help can be obtained using the `--help` option.
```
waifu2x-converter-glsl --help
```

Example usage:
```
waifu2x-converter-glsl -i mywaifu.png -m noise_scale -j 8 --scale_ratio 1.6 --noise_level 2
```
The resulting image will be saved in `mywaifu(noise_scale)(Level2)(x1.600000).png`.

Available options are:

-i <filename>,  --input_file <filename>
  Specifies the input file name.

-o <filename>,  --output_file <filename>
  Output file name. File extension is **not** automatically added.
  If unspecified, defaults to the following format:
  `[input file name]``(mode)``(noise removal level (if applicable))``(scale factor (if scaling))``.png`

-m <noise|scale|noise_scale>,  --mode <noise|scale|noise_scale>
  Specify the conversion mode. Defaults to `noise_scale`.
  * noise : Performs noise removal (using a noise removal model)
  * scale : Enlarges (with an existing algorithm, then image processing is performed using the model to sharpen the enlarged image)
  * noise_scale : Noise elimination and enlargement (noise elimination is performed first, then enlargement)

-b <int>,  --block_size <int>
  Specify the block size to be used when chunking the input image.
  The number of threads specified by this option is started in the program.
  Processing can be done more efficiently by increasing this number, but more graphics memory is required.
  The calculation formula of the required graphics memory size is as follows:
    Required graphics memory = (block size) ^ 2 * 4 * 256 + α
  If available VRAM is lacking, the program will terminate with an error.
  The default value is `512`.

--scale_ratio <factor>
  Specify the scale factor. Defaults to `2.0`.
  If anything else than `2.0` is specified, the following happens:
  * Image is 2.0x-scaled `x` times so 2\^`x` > scale factor
  * The image is scaled down using a linear filter

--noise_level <1|2>
  Specify the noise removal level. Available options are `1` and `2`.
  The default value is `1`.

--model_dir <path>
  Specify the path to the directory where models are stored. Defaults to `models`.

--,  --ignore_rest
  Ignore all options past this one.

--version
  Output version information and exit.

-h,  --help
  Display usage and exit.


 Version history
-----------------

 * v1.0.0 : Initial release
 * v1.1.0 : 
    - Fixed a bug where the output image would be blurry
    - Added chunking of the source image to reduce VRAM usage
    - ~60% speed-up
 * v1.1.1 : 
    - Fixed usage with koroshell (added a dummy `-j` switch)

 Acknowledgments
-----------------
Thanks to WL-Amigo for the C++ version of ultraist's original implementation.


References
========================

- Original implementation: https://github.com/nagadomi/waifu2x
- Python implementation: https://marcan.st/transf/waifu2x.py
- C++ implementation: https://github.com/WL-Amigo/waifu2x-converter-cpp

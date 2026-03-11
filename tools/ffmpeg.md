
# Flags

- **-i**
	- input filename
	- if your terminal is in the directory where the file is located you don't need to specify its full path
	- you can put there multiple whitespace-separated files, same stuff will be processed on them
- ***output filename?***
	- it has no flag, has to be at the very end of the command with no flags, just as the last parameter
	- will work in the directory u are in, same as input

- **-preset**
	- encoding speed (slower = better compression)
	- arguments: `ultrafast`, `superfast`, `veryfast`, `faster`, `fast`, `medium`, `slow`, `slower`, `veryslow`
- ***codecs***
	- **-c:v**
		- stands for: *codec:video*
		- **`-c:v libx265`** 
			- the H.265/HEVC encoder library
			- H.265 gives about 50% better compression than H.264 at the same quality.
	- **-c:a**
		- stands for: *codec:audio*
		- audio codec (AAC is standard, `-c:a aac`)
- ***bitrate***
	- **-b:a**
		- stands for: *bitrate:audio*
		- 192k is a good quality
		- 256k-320k near transparent
	- **-crf**
		- stands for: ***Constant Rate Factor*** as opposed to constant bitrate which can be achieved with **-b:v** but usually it does not make sense
		- quality level
			- *lower = better quality*
			- *higher = smaller filesize*
		- for 4K: 18-24 is excellent
		- CRF is cool because video has consistent quality, it lets the encoder allocate more bits when needed (dynamic scenes)
# Examples

> Check what are you working with.

`ffprobe -v error -select_streams v:0 -show_entries stream=codec_name,width,height,bit_rate,duration -of default=noprint_wrappers=1 input.mov`

> Convert crazy huge video file to something reasonably sized and compatible with modern devices (H.265/HEVC)

`ffmpeg -i input.mov -c:v libx265 -crf 22 -preset medium -c:a aac -b:a 192k output.mp4`

> Fancy progress bar with ***pv***

`ffmpeg -i input.mov -c:v libx265 -crf 22 -f matroska - | pv > output.mkv`
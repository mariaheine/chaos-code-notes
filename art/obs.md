
- external camera dongle plugged in through usb should be accessible simply through the builtin `Video Capture Device`

## important settings

## output
- **Video Encoder**
	- make sure that Hardware (NVENC) option is available and selected.
	- this greatly reduces both file sizes and CPU usage while recording.

### flatpak installation encoder issues (don't use obs through flatpak if possible)

- Make sure you have `org.freedesktop.Platform.ffmpeg-full`, check with `flatpak list`
	- Make sure your Runtime version used by obs matches the version of ffmpeg-full
	- `flatpak info com.obsproject.Studio | grep Runtime`
	- You have no way of knowing that easily, ask AI
	- Sadly
- [...]
- ugh, dont use obs through flatpak, oddly since this is their main recommended way of getting it on linux
- installing through apt instantly solved encoder problems
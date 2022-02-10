# Description
This generates an RSS file that can be used by torrent clients to automatically download Debian installers and live media. It can be used with any repository that has the same layout as [cdimage.debian.org](https://cdimage.debian.org) which includes [Tails](https://tails.boum.org/).

# Installation
The following packages need to be installed. You can use your OS's package manager (feedgen doesn't have a Debian package) `pip install -r requirements.txt`.
* beautifulsoup4
* feedgen
* feedparser
* lxml
* requests

1. `git clone https://github.com/haxwithaxe/debian-feed.git`
1. `cd debian-feed`
1. `make install` or `PREFIX=/whatever/install/path/you/want make install`
1. Edit the feed path in `/usr/local/etc/debian-feed.json`.
1. `systemctl start debian-feed.timer`

# Configuration
Example debian-feed.json with tails:
```
{
	"file_extension": ".torrent", 
	"rss_file": "/path/to/rss/server/feeds/debian-feed.rss", 
	"archs": ["amd64", "i386"],
	"sources": [
		"https://cdimage.debian.org/cdimage/release/current-live/{arch}/bt-hybrid/",
		"https://cdimage.debian.org/cdimage/release/current/{arch}/bt-cd/",
		"https://cdimage.debian.org/cdimage/release/current/{arch}/bt-dvd/",
		"https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/{arch}/bt-cd/",
		"https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current/{arch}/bt-dvd/",
		"https://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/current-live/{arch}/bt-hybrid/",
    		"https://tails.boum.org/torrents/files/"
	]
}
```
* `file_extention` should always be `.torrent` but can be changed to `.iso` or `.jigdo` with the appropriate `sources` to get a feed with iso/jigdo file links instead of torrent links.
* `rss_file` is the path to where the torrent client can get to the feed. If you put it in a web server's file directory you can get to it via the web but your torrent client might be fine with a file URI (eg `file:///path/to/debian-feed.rss`).
* `archs` is a list of CPU architectures to substitute `{arch}` in the `sources` urls. In the above example the first source will be expanded to `https://cdimage.debian.org/cdimage/release/current-live/amd64/bt-hybrid/` and `https://cdimage.debian.org/cdimage/release/current-live/i386/bt-hybrid/`. `{arch}` doesn't need to be in the source (eg `https://tails.boum.org/torrents/files/`).
* `sources` is a list of URLs to look for torrents in. These must point to the download pages. debian-feed does not spider around looking for the download pages.

title: Keyboard Maestro macros for podcasts
date: 2013-03-11



Gabe at [Macdrifter] wrote about his [Pause TimeMachine Macro] which he uses when he starts using Skype.  He adds that he knows there are other (potentially "better") to do this, but that his was just a fast and dirty solution.

Seeing Gabe's macros reminded me that I have wanted something like this for when I am [recording the _Impolite Company_ podcast].  Gabe's macro enables and disables Time Machine, but I also want to enable and disable [CrashPlan] and [Dropbox].  Like Gabe, I have a tendency to forget to turn things back on, so I wanted to automate this as much as possible.  Since I tend to only run Skype when I am actively recording a podcast, I decided to trigger my macros around the Skype.app either launching or quitting.

So I wrote two [Keyboard Maestro] macros: one for when Skype *starts* and one for when Skype *quits*.

### Prepare For A Podcast

My first new Keyboard Maestro macro is called *(wait for it)* "Prepare For A Podcast". This is what it looks like in Keyboard Maestro:

<p style="text-align:center"><img src="http://www.blogcdn.com//media/2013/03/km-skype-prepare-for-a-podcast.jpg" border="0" width="456" height="364" alt="" /></p>

**Translation:** In case you don't "speak" Keyboard Maestro's language, this is what the above means:

Whenever Skype launches, Keyboard Maestro will automatically do the following:

1) If a file exists at /Library/LaunchDaemons/com.crashplan.engine.plist (which is where CrashPlan's launchd file is stored) *unload* that file (which will disable CrashPlan) via AppleScript:

	do shell script "launchctl unload /Library/LaunchDaemons/com.crashplan.engine.plist"
	with administrator privileges

*n.b. "with administrator privileges" will prompt the user for their password. Also note that I am using Growl notifications to tell the user what is happening, especially when a password is being requested.*

2) Likewise, disable Time Machine with another bit of AppleScript:

	do shell script "tmutil disable" with administrator privileges

3) If Dropbox is running, quit it.

4) Open the "Sound" preference pane (which I want to make sure that my USB mic is configured properly for Skype).


### Resume After A Podcast

What happens *after* a podcast is pretty much exactly the opposite:

<p style="text-align:center"><img src="http://www.blogcdn.com//media/2013/03/km-skype-resume-after-a-podcast-1363036689.jpg" border="0" width="456" height="343" alt="" /></p>


**Translation:** This macro will be triggered *automatically* whenever Skype quits, and this is what it will do:

1) If the plist is found, load it into launchd:

	do shell script "launchctl load /Library/LaunchDaemons/com.crashplan.engine.plist"
	with administrator privileges

2) Resume Time Machine:

	do shell script "tmutil enable" with administrator privileges

3) If Dropbox is not running, launch it via shell script:

	open -a Dropbox


### Download ###

As with Gabe's macros, I am sure that there are other (potentially better) ways to do this, but I hope that this might be useful to others.

To download my Keyboard Maestro macros, right (control) click on the links below, otherwise they will load in your browser window.

* [prepare-for-a-podcast.kmmacros]
* [resume-after-a-podcast.kmmacros]

[resume-after-a-podcast.kmmacros]: http://files.tjluoma.com/km-podcast/resume-after-a-podcast.kmmacros

[prepare-for-a-podcast.kmmacros]: http://files.tjluoma.com/km-podcast/prepare-for-a-podcast.kmmacros

[Macdrifter]: http://macdrifter.com

[Pause TimeMachine Macro]: http://macdrifter.com/2013/03/pause-timemachine-macro.html

[recording the _Impolite Company_ podcast]: http://www.muleradio.net/impolite/

[CrashPlan]: http://www.crashplan.com

[Dropbox]: http://www.dropbox.com

[Keyboard Maestro]: http://www.keyboardmaestro.com/main/

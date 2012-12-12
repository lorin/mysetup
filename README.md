# My Setup

Here are some details about my Mac OS X setup, mostly for personal reference for moving to a new machine.
Yes, I have [NADD](http://www.randsinrepose.com/archives/2003/07/10/nadd.html).

## Every day

### VoodooPad

I live in [VoodooPad](http://flyingmeat.com/voodoopad/), it's where I keep all of my notes.


I use a couple of custom VP plugins.

#### Copy VoodooPad Link to Clipboard

This copies a link to a VP page, such as x-voodoopad-item://74a35c9a-d8e0-4314-a5ff-272a8fdd9b58

It lives at `Library/Application Support/VoodooPad/Script PlugIns/VoodooPadLink.py`. I have this mapped to Cmd-Opt-L:

    VPScriptMenuTitle = "Copy VoodooPad Link to clipboard"

    from AppKit import *
    from Foundation import *

	def main(windowController, *args, **kwargs):
			page = windowController.currentPage()
			link =  "x-voodoopad-item://" + page.uuid()
			newStr = NSString.stringWithString_(link)
			pb = NSPasteboard.generalPasteboard()
			pb.declareTypes_owner_([NSStringPboardType], None)
			pb.setString_forType_(unicode(link), NSStringPboardType)


#### Paste as Courier New 12 pt

This pastes the text in the clipboard as courier, and then returns the formatting to what it was before.

	VPScriptMenuTitle = "Paste as Courier New 12pt"

	from AppKit import *
	from Foundation import *

	def main(windowController, *args, **kwargs):
		textView = windowController.textView()
		f = NSFont.fontWithName_size_("Courier New", 12)
		attrs = {NSFontAttributeName: f}

		pb = NSPasteboard.generalPasteboard()
		s  = pb.stringForType_(NSStringPboardType)

		if s is None:
			return # in case we didn't have text on the clipboard

		attstr = NSMutableAttributedString.alloc().initWithString_(s)
		attstr.setAttributes_range_(attrs, (0, len(s)))

		# wrapping our change in shouldChangeTextInRange_replacementString_ + didChangeText
		# allows for free undo support
		if textView.shouldChangeTextInRange_replacementString_(textView.selectedRange(), None):
			textView.insertText_(attstr)
			textView.didChangeText()


### Quicksilver

[Quicksilver](http://qsapp.com) is my main launcher app. I use the new Nostromo interface. Custom triggers I've added:

 * Open journal.vpdoc (Opt-J)
 * Open tasks.taskpaper (Opt-Cmd-G)
 * Launch Plain clip (Cmd-V)

 I make heavy use of the Remote Hosts Module plugin. This allows you to define a ``~/.hosts`` file which lists the hosts that can be ssh'd to.
 
### FoldingText

I use [FoldingText](http://www.foldingtext.com/) for ad-hoc todo list, and code and config fragments. Basically, it holds my context information when I'm working on a particular development task.

### iTerm2

iTerm2 is my terminal replacement. I have it configured to handle ssh:// URLs so that it works with the QuickSilver Remote Hosts Plugin. You do:

    iTerm -> Preferences... -> Profiles -> (Default profile) -> General -> URL Schemes -> ssh

I like the "White on Black" preset coloring scheme.


### TaskPaper

TaskPaper is my GTD todo list tool. I like the plain text format. It syncs to the iPhone using Dropbox.

### Sublime Text 2

Sublime Text 2 is my default text editor these days. Installed the following packages:

 * [Package Control](http://wbond.net/sublime_packages/package_control) for installing packages
 * CodeIntel for smarter autocompletion inside of Python
 * [SublimeLinter](https://github.com/Kronuz/SublimeLinter) for Python PEP8 checking (may be redundant with above)
 * [SideBarEnhancements](https://github.com/titoBouzout/SideBarEnhancements) (see also [forum](http://www.sublimetext.com/forum/viewtopic.php?f=5&t=3331) and [web page](http://www.railsexperiments.com/sublime-sidebar-enhancements/)).
 * [nginx](https://github.com/kvs/ST2Nginx) for dealing with nginx files
 * [Emmet](http://docs.emmet.io/) for HTML editing
 * [Djaneiro](https://github.com/squ1b3r/Djaneiro) for Django templates
 * [Origami](https://github.com/adzenith/Origami) for keyboard pane control
 * [Gherkin](https://gist.github.com/864839) syntax highlighting
 * [MarkDownPreview](https://github.com/revolunet/sublimetext-markdown-preview) Markdown preview
 * [SublimeREPL](https://github.com/wuub/SublimeREPL) for running Python inside of Sublime Text

Settings - User (Preferences.sublime-settings)

	{
		"auto_complete_commit_on_tab": true,
		"color_scheme": "Packages/Color Scheme - Default/Monokai.tmTheme",
		"draw_white_space": "all",
		"fade_inactive_panes": false,
		"git_command": "/usr/bin/git",
		"highlight_line": true,
		"hot_exit": false,
		"ignored_packages":
		[
			"Vintage",
			"Emmet"
		],
		"remember_open_files": false,
		"rulers":
		[
			78
		],
		"theme": "Soda Dark.sublime-theme",
		"trim_trailing_white_space_on_save": true,
		"shift_tab_unindent": true
	}


Key Bindings - User (Default (OSX).sublime-keymap):

	[
		{ "keys": ["super+l"], "command": "show_overlay", "args": {"overlay": "goto", "text": ":"} },

		// Find in project
		{ "keys": ["ctrl+alt+super+f"],  "command": "show_panel", "args": {"panel": "find_in_files", "location": "<open folders>" } },

		// Go to the definition of a class/method in Python
		{ "keys": ["super+shift+t"], "command": "show_overlay", "args": {"overlay": "goto", "text": "@"} },

		// Find the Python definition of the thing the cursor is on
		{ "keys": ["super+."], "command": "goto_python_definition" },

		// Run a PEP8 check
		{ "keys": ["super+p"], "command": "sublimelinter", "args": {"action": "lint"} },

		{ "keys": ["ctrl+t"], "command": "transpose" },

		// Multiple carets
		{ "keys": ["super+alt+up"], "command": "select_lines", "args": {"forward": false} },
		{ "keys": ["super+alt+down"], "command": "select_lines", "args": {"forward": true} }

		// Overrides F2 mapping in one of the packages
		{ "keys": ["f2"], "command": "next_bookmark" },

		{ "keys": ["super+r"], "command": "show_panel", "args": {"panel": "replace"} }


	]


	Python settings (File Settings -> Syntax Specific)

	{
		// The number of spaces a tab is considered equal to
		"tab_size": 4,

		// Set to true to insert spaces when tab is pressed
		"translate_tabs_to_spaces": true,

		// Set to "none" to turn off drawing white space, "selection" to draw only the
		// white space within the selection, and "all" to draw all white space
		"draw_white_space": "all"
	}


Restructured text settings (File Settings -> Syntax Specific)

	{
		// The number of spaces a tab is considered equal to
		"tab_size": 4,

		// Set to true to insert spaces when tab is pressed
		"translate_tabs_to_spaces": false,

		"draw_white_space": "all"


	}


Sublime Linter settings (Package Settings -> Sublime Linter -> Settings - User)

    {
	    // Override default setting, which ignores E501
        "pep8_ignore":[ ]
    }



### AntiRSI

[AntiRSI](http://tech.inhelsinki.nl/antirsi/) protects my wrists. Sold nowadays in the Mac app store. There's also a modified version at <http://sabi.net/nriley/software/#antirsi>.


### 1Password

1Password is my password tracker.


### MenuMeters

[MenuMeters](http://www.ragingmenace.com/software/menumeters/) shows you CPU, memory, and network usage in the title bar.


### Homebrew

I use Homebrew to install command-line tools. It's an alternative to Fink and MacPorts.


### Text Expander

I really only use the following snippets, but boy do I use them:

    ddate %1m/%e/%y
    ddate) %1m/%e/%y)


### Tower

Tower is my Git GUI.

### Path Finder

Path Finder is my finder replacement.

`Help -> Reveal in Path Finder Toolbar Item` will give instructions on how to add a "Reveal in Path Finder" icon to the Finder toolbar.


### Default Folder X

I use [Default Folder X](http://www.stclairsoft.com/DefaultFolderX/) to jump to an open Finder or Path Finder window in an open/save dialog box.


### Mou

[Mou](http://mouapp.com) is my Markdown editor.

### LaTeX

I use [MacTeX](http://www.tug.org/mactex/2011/) for LaTeX docs.

### Snapz Pro X

Screencasts


### SizeUp

Window manipulation.

### Skitch

Screen capture

### Skim

[Skim](http://skim-app.sourceforge.net/) is a PDF reader, alternative to Preview.

### Adium

IM & IRC

### Meteorologist

[Weather](http://heat-meteo.sourceforge.net/)

### Fantastical

Quick calendar entry

### BusyCal

iCal replacement

### Growl

Notification

### QuickCursor

Use an external editor for text boxes


### Soulver

Excel replacement for simple calculations.

### Witch

Fast task switching

### Fluid

I use Fluid to make certain web pages look like apps

## RegExhibit

Regex helper

## BibDesk

BibTex-based reference manager

## FocusBar

http://focusbarapp.com/

## .profile

	 PS1='[$(__git_ps1 "(%s) ")\u@\[\e[1;32m\]\h\[\e[1;0m\] \w]\n\$ '

    PROMPT_COMMAND='echo -ne "\033]0;[local]: ${PWD}\007"'
    export CLICOLOR=1
    export EDITOR='subl -w'
    export PATH=$PATH:/usr/texbin:/Users/lorin/work/scripts

	# Pass color control codes to terminal
    export LESS=-R

    # SVN shortcuts
	alias svnadd="svn st | grep '^?' | cut -c9- | perl -p -e 's/\n/\0/;' | xargs -0 svn add"
	alias svnrm="svn st | grep '^!' | cut -c9- | perl -p -e 's/\n/\0/;' | xargs -0 svn rm"
	alias svnco="svn commit -m ' '"
	alias svncommit="svnadd && svnrm && svn commit"
	alias svnup="svn up -N"
	alias svnst="svn st"

	alias pf='open -a "Path Finder.app"'
	alias ip="curl icanhazip.com"


	# Reset the dock
	alias dock="killall Dock"

	function dir {
        echo `pwd` | tr -d \\n | pbcopy
	}

	function big {
        osascript -e "tell application \"Quicksilver\" to show large type \"$1\""
	}

	# bash completion, installed via homebrew
	. /usr/local/etc/bash_completion


## Monosnap
[Monosnap](http://monosnap.com/) is a simple screenshot tool, an alternative to Skitch.

## Grin
[grin](http://pypi.python.org/pypi/grin) is a Python-version of ack, great for text searching.

## Pythonbrew

Useful for testing against Python 3.X stuff.


## SCM Breeze

Git shortcuts

https://github.com/ndbroadbent/scm_breeze

    git clone git://github.com/ndbroadbent/scm_breeze.git ~/.scm_breeze
    ~/.scm_breeze/install.sh
    source ~/.bashrc

## Autojump (jumpstat)

https://github.com/joelthelion/autojump/wiki

## autoenv

https://github.com/kennethreitz/autoenv


In ~/.profile:

	source /usr/local/Cellar/autoenv/0.1.0/activate.sh

In myapp/.env

	use_env myapp


## Hub

https://github.com/defunkt/hub

Install and setup:

	brew install hub
	hub config --global github.user lorin


## Sequel Pro

GUI for MySQL.


## Keyboard system preferences

### Modifier Keys...

Caps Lock is set to Control

### Keyboarding shortcuts

### All Applications

 ^Space is mapped to Show Help Menu (Keyboard Preferences Pane -> Keyboard Shortcuts -> Application Shortcuts -> All Applications -> Show Help Menu). This allows me to select a menu without using the mouse.


### VoodooPad

Copy VoodooPad Link to clipboard: `Cmd-Opt-L`
Paste as Courier New 12pt: `Cmd-Opt-V`



## Stuff I'm not using right now

This is stuf I used to use but don't do so anymore, but want to keep track of.

## Ack

<http://betterthangrep.com>

(I've since moved on to grin)

My .ackrc looks like:

	--type-set=restructuredtext=.rst
	--type-set=xslfo=.fo
	--type-set=svg=.svg
	--type=noxslfo
	--nohtml
	--nojs
	--nosvg
	--ignore-dir=.venv
	--pager=less -R


### GeekTool

Display stuff on your desktop, it's in the App store for Lion.

### SSH tunnel manager

Exactly what you'd expect, from the same guy who does geektool.


### Google Chrome Canary

Bleeding edge google code


### Open Terminal Here

"Open Terminal Here" allows you to open up a terminal window in the current Finder directory.

The original app can be found at <http://www.entropy.ch/software/applescript>. Download the [OpenTerminalHere.app.zip](http://www.entropy.ch/software/applescript) file from that site.

I use a slightly customized version that uses iTerm2 instead, and uses an updated icon.

There's a more modern-looking icon hosted at <http://henrik.nyh.se/2007/10/open-terminal-here-and-glob-select-in-leopard-finder>. As described on that site, save the [openterminalhere-droplet.icns](http://henrik.nyh.se/uploads/openterminalhere-droplet.icns) file, and replace OpenTerminalHere.app/Contents/Resources/droplet.icns with openterminalhere-droplet.icns.

To get it to open up a terminal window with iTerm2, I've edited the OpenTerminalHere.app/Contents/Resources/Scripts/main.scpt file and modify the process_item function (note: I've renamed my iTerm2 app from iTerm.app to iTerm2.app):

	on process_item(this_item)
		set the_path to POSIX path of this_item
		repeat until the_path ends with "/"
			set the_path to text 1 thru -2 of the_path
		end repeat

		set cmd to "cd " & quoted form of the_path & " && echo $'\\ec'"

		tell application "iTerm2"
			activate
			set myterm to (make new terminal)
			tell myterm
				launch session "Default Session"
				tell the last session
					write text cmd
				end tell
			end tell
		end tell

	end process_item


Finally, add this app to the Finder toolbar by right-clicking on the toolbar, choosing "Customize Toolbar...", and dragging the app into the toolbar.

### Spirited Away

[Spirited Away](http://drikin.com/2010/11/spirited-away.html) auto-hides unused apps.

### Rubber

[Rubber](http://launchpad.net/rubber) is a command-line tool to make it easier to build LaTeX documents.

### Moom

Moom is a competitor to SizeUp. 
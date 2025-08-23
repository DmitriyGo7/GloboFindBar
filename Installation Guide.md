You should choose how to install GloboFindBar. You can simply download the exact Firefox version that I am using from Mozilla, and replace two of its files with files prepared by me with all the necessary changes already made in them. Or, if you want to use a newer Firefox version, or if you don't trust me, you can make changes to code yourself as described below (in section "Option 2").

___________________________________

OPTION 1: Use omni.ja files prepared by me for a specific Firefox version.

STEP 1: Download and install Firefox ESR 140.2.0 - https://ftp.mozilla.org/pub/firefox/releases/140.2.0esr/ .

STEP 2: Disable Firefox auto updates.

Here is the guide https://www.webnots.com/how-to-disable-automatic-update-in-firefox/ - use method 2 or method 3.

In short, go to Firefox install folder (for example C:\Program Files\Mozilla Firefox ), create a new folder in "Mozilla Firefox" folder, name it "distribution". Create a new text file in "distribution" folder. Paste

```
{
"policies":
   {
     "DisableAppUpdate": true
    }
}
```

in that file, save it, make sure file name extensions are visible in Windows Explorer, then rename the file to "policies.json".

STEP 3: not yet written.

STEP 4: Clear startup cache.
Click on three horizontal bars (hamburger) button in the top right corner of Firefox window, choose "Help" in menu, then "More troubleshooting information", then click "Clear startup cache..." button on the right side of the window, and confirm restart. DON'T FORGET TO DO THAT EVERY TIME YOU CHANGE ANY OF THE FIREFOX CODE, otherwise changes will not apply.

___________________________________

OPTION 2: Make code changes yourself.

STEP 1: Have any Firefox version you want already installed. I've only tested these code changes in Firefox ESR 140.1.0 and 140.2.0 (August 2025) on Windows, so I can't guarantee that they will work on other, newer or older versions of Firefox. I expect them to work on all desktop operating systems, and I think they will probably work for a couple years or maybe longer in future versions of Firefox, but I can't know for sure.

STEP 2: Disable Firefox auto updates.

Here is the guide https://www.webnots.com/how-to-disable-automatic-update-in-firefox/ - use method 2 or method 3.

In short, go to Firefox install folder (for example C:\Program Files\Mozilla Firefox ), create a new folder in "Mozilla Firefox" folder, name it "distribution". Create a new text file in "distribution" folder. Paste

{
"policies":
   {
     "DisableAppUpdate": true
    }
}

in that file, save it, make sure file name extensions are visible in Windows Explorer, then rename the file to "policies.json".

STEP 3: Unpack both omni.ja files like they are ZIP archives.

Go to Firefox install folder (for example C:\Program Files\Mozilla Firefox ), copy "omni.ja" file and paste it somewhere in unprotected user data folder like in Desktop. Rename the coped file to "omni from root.zip". Extract its contents with WinRAR (other archiver programs will probably work, but I only tested with WinRAR).

Then go from Firefox install folder to its "browser" subfolder (for example C:\Program Files\Mozilla Firefox\browser ). There is a second "omni.ja" file in it. A different one, so MAKE SURE you don't mix things up! Copy this "omni.ja" file and paste it somewhere in unprotected user data folder as well. Rename the coped file to "omni from browser.zip". Extract its contents with WinRAR.

STEP 4: Make code changes.

You can use Notepad or Notepad++ to open, modify and save *.js files with program code like they are ordinary text files.

HERE ARE THE CHANGES IN DETAIL:

CHANGE #1:

Goal: Every time a string in FindBar is changed by user (character is added or deleted, or the whole string is pasted or deleted), the search in page gets launched automatically. In the beginning of that search we store the searched string in window-scoped variable.

Change is made in "omni from root" folder.

The modification is in file omni\chrome\toolkit\content\global\elements\findbar.js

after string

let val = value || this._findField.value;

you should add string

window._globalFindText = val; 		//GloboFindBar

CHANGE #2:

Goal: In FindBar constructor, which is called when user presses Ctrl+F or F3 for the first time in current tab, we place in its input field the string we stored earlier in window-scoped variable, if it is defined.

Change is made in "omni from browser" folder. That is NOT "omni from root" from previous step!

The modification is in file (browser)\omni\chrome\browser\content\browser\tabbrowser\tabbrowser.js

Function async _createFindBar(aTab) {

after string

findBar.browser = browser;

you should replace string

findBar._findField.value = this._lastFindValue;

to block

	  if (typeof window._globalFindText !== 'undefined') { 		//GloboFindBar+
		  findBar._findField.value = window._globalFindText;
	  } else {							//GloboFindBar-
		  findBar._findField.value = this._lastFindValue;
	  }

CHANGE #3:

Goal: When user activates a tab for any reason (by clicking on it, creating it, or it became active (current) after closing another tab), if FindBar already exists (constructed) in that tab, then we place in its input field the string we stored earlier in window-scoped variable, if it is defined.

Change is made in "omni from browser" folder.

The modification is in file (browser)\omni\chrome\browser\content\browser\parent\ext-browser.js (MAKE SURE it is a "parent" folder, NOT "child", as there is "ext-browser.js" in "child" folder as well)

Function emitActivated(nativeTab, previousTab = undefined) {

Right in its beginning (in the next string) you should add block

	if ((typeof nativeTab._findBar !== 'undefined') && (typeof nativeTab.ownerGlobal._globalFindText !== 'undefined')) {		//GloboFindBar+
		nativeTab._findBar._findField.value = nativeTab.ownerGlobal._globalFindText;
	}																//GloboFindBar-

That's it, basically just 5 or 6 lines of code if you don't count lines with curly brackets make FindBar global!

STEP 5: Pack again and replace omni.ja files.

I used WinRAR successfully. Select all files and folders inside your unpacked omni folder, right click and select "Add to archive...", in WinRAR window that appears select Archive format: ZIP and Compression method: None, press OK. The most important thing here is not to compress by right clicking omni folder itself. Select all items inside of it instead. There must be no "omni" root folder inside the created archive, its contents should show up immediately instead.

Compress both omni folders this way, and place them back in C:\Program Files\Mozilla Firefox and C:\Program Files\Mozilla Firefox\browser correspondingly. While Firefox is not open, MAKE A BACKUP of original omni.ja files by renaming them somehow in case something goes wrong, then rename "omni from root.zip" and "omni from browser.zip" back to "omni.ja".

STEP 6: Clear startup cache.
Click on three horizontal bars (hamburger) button in the top right corner of Firefox window, choose "Help" in menu, then "More troubleshooting information", then click "Clear startup cache..." button on the right side of the window, and confirm restart. DON'T FORGET TO DO THAT EVERY TIME YOU CHANGE ANY OF THE FIREFOX CODE, otherwise changes will not apply.

That is all.

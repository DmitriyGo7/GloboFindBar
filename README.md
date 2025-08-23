# GloboFindBar
GloboFindBar makes search string entered in Firefox's FindBar (Ctrl+F, find in page) be the same in all tabs inside a single window.

GloboFindBar is a set of very small changes in Firefox browser's internal JS files which results in its FindBar (which is a panel called by pressing Ctrl+F or F3) no longer remembering search string, entered by user, per tab. Instead, GloboFindBar makes the search string that user entered the last time show up in any other tab as a suggested string for launching search in page. It does not launch the search automatically, so search highlights from user's last search are preserved until user manually initiates a new search in page.

Technically, GloboFindBar simply substitutes search string in input field in FindBar in each tab to the last string that user entered in any other tab's FindBar. It substitutes it at the moment FindBar is called or when user switches tabs. Settings of FindBar (like "Highlight All", "Match Case", even whether FindBar is shown on screen) are not synchronized between tabs, only text is synchronized. Nothing is synchronized between separate windows of Firefox.

___________________________________

THE DRAWBACK:

You'll have to disable Firefox updates and you'll have to stay on obsolete version of Firefox. If you update even once, the update process will replace both omni.ja files in which changes have been made, so you'll have to manually apply code changes again (it is only several minutes of work, but I'd imagine it will very quickly become tiresome even for me to do it at least each month).

___________________________________

HISTORICAL PERSPECTIVE:

Opera Presto had an ability to put its FindBar on toolbar, to be permanently visible at all times. It was a per window FindBar (like GloboFindBar). That was super convenient.

Firefox had a per window FindBar until version 25, when Firefox developers somehow saw per tab FindBar as more logical, probably in attempt to mindlessly copy Chrome like they always do. https://bugzilla.mozilla.org/show_bug.cgi?id=537013 . But they failed even in that, they made it even worse than in Chrome - https://bugzilla.mozilla.org/show_bug.cgi?id=1975832 . But solutions to revert to old behavior quickly appeared: XUL/XPCOM extensions FindBar Tweak, made by Mozilla's then-employee, and GlobalFindbar. But then Mozilla stopped supporting XUL/XPCOM extensions since Firefox 57, so since version 57 until now there was no solution, no way to make FindBar global.

Palemoon intentionally reverted back to per window FindBar since version 28.1, and even gave users a choice. User can set about:config value "findbar.termPerTab" to "true" if per tab FindBar is desired. Unfortunately, Palemoon has low compatibility with modern web sites, and is extremely slow.

There actually is an extension written for modern Manifest v3 extensions platform for Firefox and Chrome, a great idea, that also can act as a global FindBar, called "Multi Find: Search, Highlight, Explore" - https://addons.mozilla.org/firefox/addon/multi-find-search-highlight/ and https://chromewebstore.google.com/detail/multi-find-search-and-hig/dffaiikpbncahnghlfnkhagffaemhgfo . It has useful additional functionality: user can search for multiple phrases at once, highlighting each phrase in its own color. Unfortunately, I found it buggy or working unreliably on certain rare websites (for example Twitch), especially after a page has dynamically loaded further (infinite scrolling). Also it is not possible to map it to Ctrl+F on Firefox, although it is possible on Chrome. Enter or F3 also do not work as expected from standard FindBar. If its author will be able to fix all bugs, and it will be possible to map extensions to default hotkeys in Firefox, GloboFindBar will not be needed any more.

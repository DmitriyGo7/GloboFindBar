# GloboFindBar
Makes search string entered in Firefox's findbar (Ctrl+F, find in page) the same in all tabs inside a single window

GloboFindBar is a set of very small changes in Firefox browser's internal JS files which results in its FindBar (which is a panel called by pressing Ctrl+F or F3) no longer remembering search string, entered by user, per tab. Instead, GloboFindBar makes the search string that user entered the last show up in any other tab as a suggested string for launching search in page. It does not launch the search automatically, so search highlights from user's last search are preserved until user manually launches a new search in page.

Technically, GloboFindBar simply substitutes search string in input field in FindBar in each tab to the last string that user entered in any other tab's FindBar. It substitutes it at the moment FindBar is called or when user switches tabs. Settings of FindBar (like "Highlight All", "Match Case") are not synchronized between tabs, only text is synchronised. Nothing is synchronised between separate windows of Firefox.

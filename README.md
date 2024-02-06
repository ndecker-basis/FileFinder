ReadMe File for the FileFinder DWC App
=============================================

FileFinder Overview
-------------
**FileFinder** is a BBj program designed to streamline file searches within the BBjServices filesystem. It efficiently identifies files by adhering to user-provided naming conventions and/or by searching within files for desired text. Users can conduct recursive file system searches and employ regular expressions to locate their desired content within files.

The BBj program is designed to run in the BASIS [Dynamic Web Client](https://documentation.basis.cloud/WhitePapers/DWCOverviewHelpPage.pdf), allowing for a responsive user interface that adapts to the user's browser window or screen real estate. It also takes advantaee of the [BBjWebManager](https://documentation.basis.cloud/BASISHelp/WebHelp/bui/BBjBuiManager/bbjbuimanager.htm)'s ability to inject links, CSS, and set the default theme to follow the operating system's light or dark theme setting.

The utility can be run in a number of different modes, allowing users to search for files that:
* Match a naming convention
* Match a naming convention and contain user-defined content

File searches may be performed recursively, or can be limited to the initial target directory.   Users can optionally chose to ignore case when matching file names and/or file content. However, it's real power comes into play when the user chooses to supply a regular expression that's used to find matches within the contents of each file searched.

<p align="center"><img src="CodeViewer-Screenshot1.png" width="480"></p>
<p align="center"><b>Figure 1:</b> Screenshot Goes Here</p>

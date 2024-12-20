ReadMe File for the FileFinder DWC App
=============================================

FileFinder Overview
-------------
**FileFinder** is a BBj program designed to streamline file searches within the BBjServices filesystem. It efficiently identifies files by adhering to user-provided naming conventions and/or by searching within files for desired text. Users can conduct recursive file system searches and employ regular expressions to locate their desired content within files.

The BBj program is designed to run in the BASIS [Dynamic Web Client](https://documentation.basis.cloud/WhitePapers/DWCOverviewHelpPage.pdf), allowing for a responsive user interface that adapts to the user's browser window or screen real estate. It also takes advantage of the [BBjWebManager](https://documentation.basis.cloud/BASISHelp/WebHelp/bui/BBjBuiManager/bbjbuimanager.htm)'s ability to inject links, CSS, and set the default theme to follow the operating system's light or dark theme setting.

The utility can be run in a number of different modes, allowing users to search for files that:
* Match a naming convention
* Match a naming convention and contain user-defined content

File searches may be performed recursively, or can be limited to the initial target directory. Users can optionally chose to ignore case when matching file names and/or file content. However, it's real power comes into play when the user chooses to supply a regular expression that's used to find matches within the contents of each file searched.

<p align="center"><img src="FileFinder-Screenshot1.png" width="500"></p>
<p align="center"><b>Figure 1:</b> Screenshot Goes Here</p>


BASIS Documentation
-------------
FileFinder can be extended to perform arbitrary operations on files that match the desired search critera. In addition to the two matching modes (by name and/or contents), developers can add their own operation modes to further process the matched files. The utility has been used in this manner to make systemic changes to the [BASIS Online Help System](https://documentation.basis.cloud/BASISHelp/WebHelp/index.htm). By way of example, the utility can be launched and configured to search for `.htm` files. This means that it will match files with the `.htm` extension as well as the `.html` extension, thus targeting the HTML source files for the documentation set. Extended modes can then be chosen to modify the documentation source files in a particular way. Continuing with the example, the utility relied on the [jsoup](https://jsoup.org/) open source Java library to parse the source HTML, find and extract information by traversing the DOM or via CSS selectors, then manipulate the content and write the updated HTML to the original file with formatted output.

A specific example of one of these modes is a custom method in the `FileFinder` class that scans all matching documentation HTML files and cleans the underlying HTML in their Method Tables. In the custom method, it scans the DOM for `<table>` elements that contain the text *Method* or *Return Value*. This allows it to target the method Tables, such as the `<table>` element with the text ***Methods of BBjButton*** on the [BBjButton](https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjbutton/BBjButton.htm) page. When it finds a Method Table, it iterates through all attributes and styles in the parent `<table>` element as well as the `<tr>` and `<td>` child elements. When it finds an attribute that isn't specific to the MadCap Flare help system, it removes the attribute. In this manner, it cleans and tidys (formats the output adhering to proper indention) the Method Table by:

1. Removing all inline attributes (except for those that are proprietary MadCap attributes, as we wish to retain those)
2. Removing all inlines styles (because BASIS now relies on CSS classes instead of inline styles to format the tables)
3. Removing all empty `<p>` tags (leftovers from the old RoboHelp system)
4. Adding the `Methods_Table` class to the table (to ensure proper styling for the table as the class name exists in the external CSS file to format the table)
5. Converting the first row of `<td>` elements to `<th>` elements and wrapping them in `<thead>``</thead>` tags (properly using the table header element syntax)
6. Trimming all unnecessary trailing whitespace (leftovers from the old RoboHelp system)

After the custom method finishes processing the file's HTML for one or more Method Tables, it updates the file on the disk with the new contents. The custom `FileFinder` method is then complete and returns an integer that informs the calling method as to whether or not it updated the file. The utility then continues its search routine until it finds another match and begins the cleanup process all over again for the newly-matched file.

Note that this particular example required access to the jsoup library in order to parse and manipulate the HTML content within the documentation files. To accomplish this, we did the following:

1. Downloaded the latest [jsoup](https://jsoup.org/download) core library JAR file
2. Moved the file to our custom JAR directory: `<BBjHome>/jars/`.
3. Navigated to **BBjServices > Java Settings > Classpath** in [Enterprise Manager (EM)](http://localhost:8888/bbjem/emapp) and added the jsoup JAR to the **bbj_default** classpath.

Alternatively, you may also add the jsoup JAR file to an existing [Session-Specific Classpath (SSCP)](https://documentation.basis.cloud/BASISHelp/WebHelp/usr/BBj_Components/BBjServices/sessionspecific_classpath.htm), or create a new SSCP and add the JAR to that. In this scenario, you will need to launch the `FileFinder.bbj` program specifying the custom SSCP using the `-CP<SSCP_NAME> ` syntax, also covered in the [Running BBj from the Command Line](https://documentation.basis.cloud/BASISHelp/WebHelp/usr/BBj_Components/BBjServices/Thin_Client/running_from_the_command_line.htm) page.
# FTPDownloaderGen
A Python script that generates custom FTP downloader scripts
<br><br>

About:

This is basically a script that creates other scripts!

The generated scripts are autoexecutable files that connect to a certain FTP address/path and download files according to a few options set by the user:

 - Specific file extension or all file extension types

 - Loop on/off with custom timer (also optional)

 - History mode on/off: history mode creates a .txt file that keeps track of all the downloaded files (both on your local path and the FTP path). The script checks for items in the list every time it runs and skips the ones already on the list, avoiding duplicates/repeat downloads. History mode off is just a simple downloader.
<br><br>

IMPORTANT INFO:

 - For security reasons, there is no FTP information inside any of the files (just mock examples). You have to provide your own for the files to work, or use the ADScriptGen and 
<br><br>

Files and descriptions:

1. ADScriptGen:
 - Autoexecutible version of the script generator. It's going to ask you a few questions and then create your custom downloader!

2. ScriptCode.py:
 - Python code for the ADScriptGen file.

3. autodownExample:
 - Autoexecutabl example of a downloader made using ADScriptGen
 - THIS WILL NOT WORK UNTIL YOU EDIT INSIDE AN IDE AND ADD YOUR OWN FTP ADDRESS/USER/PASSWORD/FILEPATH.

4. ExampleCode.py:
 - Python code for the autodownExample file.

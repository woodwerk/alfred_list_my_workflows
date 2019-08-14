#List My Workflows (Python) - ALFRED Workflow

This workflow assembles a list of your Alfred workflows (names and descriptions).

![](http://bit.ly/2Z6UCO2)

The Alfred workflows are buried in gibberish-named folders deep inside the user's library folder.

![](http://bit.ly/2Z3CiWj)

The workflow is triggered by a hotkey that runs a Python script. The steps of the code are:

* get the root workflow path
* get a listing of the folders in that path (these are the folders that begin `user.workflow` followed by an alphanumeric ID)
* loop through the folders and in each pass
	- assemble the path to the `info.plist` file
	- read the contents of the file
	- extract name and description
	- append the results array with name and description
* sort the array of names & descriptions
* join the array into a string seperated by new lines and push as output


```
#!/usr/bin/env python

import os 	
import sys
import re
import plistlib

workflow_path = os.path.expanduser('~/Library/Application Support/Alfred/Alfred.alfredpreferences/workflows/')

wflows = [] # initialize array

fnames = os.listdir(workflow_path) # get list of workflow folders

for x in fnames:
    fn = workflow_path + x + '/info.plist' # path to XML file for this workflow
    
    plist_content = plistlib.readPlist(fn) # read the XML contents

    wname =  plist_content['name'] # grab the workflow name

    wdesc = plist_content['description'] # grab the workflow description
        
    wflows.append(wname + ': ' + wdesc) # append workflow into to wflows array

wflows.sort() # sort the array

sys.stdout.write('\n'.join(wflows)) # return joined string, separated by new lines
```

The results are a block of text with each line representing the  name and description of all the workflows:

```
Advanced Google Maps Search: Search Google Maps
Alfred Maestro: Search Keyboard Maestro Macros
Alfred PDF Tools: Optimize, encrypt and manipulate PDF files
Amphetamine Control: Keep your Mac awake with Amphetamine.
Bit.ly Link: Copies a custom bit.ly url to the clipboard
Bluetooth On/Off: Turn Bluetooth On or Off
CSD GUI SuperUser: Launch the CSD Depot GUI
CSD GUI: Launch the CSD Depot GUI
CamelCase: Splits CamelCase names with spaces
Change/Transform Text Case: Change text case to upper, lower, title, camel, kebab, or snake.
Colors: Color tools for developers
Convert Word to PDF: Converts a Word document to PDF (pre-launch Word app)
Convert: Free-form conversions between different units
Copy filename or path: Rebuilt with Python
DataHound: Access dataHound remotely via Jump Desktop
DiceWare: Create a diceware password
Direct Dropbox URL: Direct, shortened Dropbox URL copied to clipboard
Drafts: use callback URLs to interact with Drafts
Evernote: Use Evernote with Alfred
Fantastical: Add Fantastical entries with Alfred
FastUserSwitch: Hardwired user switch
Funnel: Funnel selected text or a file through various filters.
Giphy: Search Giphy for animated gifs
Google Autocomplete: Provide Google Autocomplete Suggestions
Google Map Directions: Get directions from work to a destination
Google URL Shortener: Shortens a long URL using goo.gl
HoudahSpot: search HoudahSpot for " "
Image Utilities: Do all kinds of things with images!
Kill Process: Easily find and kill misbehaving processes.
List My Workflows: Create a list of my workflows
Lorem Ipsum: Lorem Ipsum Generator via freeformatter.com
Mail.app Search: Search Mail.app by author, content or subject line
Merge Images into PDF: Convert selected image files into single PDF
Modify SWA ICS files: Adjust SWA flights to my preferred format
Mount dataDog: Mount/unmount dataDog volumes
Mount network shares: Mount/unmount network volumes
Network Location: Change your network location
New text file here: Creates a new text file in the folder of the currently selected file.
OCR: Apply PDFpen to OCR the selected PDF
Paste as plain text: 
Phone Number Cleaner: Returns only digits of phone number
Pocket for Alfred: Manage Your Pocket List With Alfred
PopClipAppear: Nudges PopClip to appear
Print2Color: prints selected files to work color printer
Rename: Rename multiple files - fixed or flexible
RipRename: Rename the most recent MKV to the name of the current DVD
Sandbox: A place to explore new workflow ideas
Show Hidden Files: 
StrongPassword: Get a strong password by leveraging multiple sources
Terminal here!: iterm or terminal cd into topmost Finder folder
TextCount: Returns character, word and paragraphs in selected text
TextSoap Cleaners: This workflow allows for working with the TextSoap program from Alfred.
TimeZones v1.9.0: A World Clock for Alfred
Toggle Fn keys as standard keys: Toggle Fn keys as standard keys from previous state
Toggle Keyboard Viewer: 
VPN: Launch or kill S3 VPN
Video Downloader: Download videos from YouTube, Vimeo, DailyMotion and more...
Wi-Fi: Set Wi-Fi function ON/OFF
``` 


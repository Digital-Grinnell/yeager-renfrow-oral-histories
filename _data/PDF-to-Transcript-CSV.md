# PDF to Transcript CSV Conversion

All of this is done in Adobe Acrobat and the VSCode editor.  Note that it helps to have the VSCode extension for Rainbow CSV installed and enabled! 

## Steps

1) Open the transcript PDF file in Adobe Acrobat.  We will copy/paste the text of the transcript from this file AND use it for visual guidance.

2) Lookup the interview's `objectid` value from the first column of our Google Worksheet (https://docs.google.com/spreadsheets/d/1z2W5L8JVdNcqaPdNZyXAThw2wnf7TSUK2Y2giO9i6NM/edit?gid=600970621#gid=600970621).

3) Create a new file in VSCode and name it using the `objectid` value from step 2 with an extension of `.csv`.  

4) Copy the text of the transcript from the open Adobe Acrobat window.

5) Paste the copied text into the open `<objectid>.csv` file.  Now we can clean up the new `.csv` file with a few find/replace operations.  

6) We need to change all the embedded `commas` in the text from ASCII commas to UTF-8 commas so they are not perceived as field seperators.  Use the following find/replace characters (copy/paste them into the find/replace widget in the editor):  `find=, replace=‚`   Note: Those two characters ARE NOT THE SAME.  Use a simple find/replace, not a regex here.  The find/replace widget should look something like this: 

![](documents/images/2024-09-18-15-15-10.png)


Note that if you have Rainbow CSV installed and working you should see the colors of the text change as you click through the substitutions, this is because the field delimiter commas are being replaced with non-delimiter commas as you proceed, turning two adjacent "fields" into a single field.  If you convert a few of these and are satisfied with the result, feel free to use the "change all" button to complete the process.

7) Now, in similar fashion to the commas, we need to replace all the embedded double quotes, changing them from ASCII double quotes to UTF-8 "smart quote" characters like using these find/replace elements:  `find="  replace=“`.  Again take note that these two characters ARE NOT THE SAME!  Use a simple find/replace, not a regex here.  The find/replace widget should look something like this: 

![](documents/images/2024-09-18-15-21-38.png)

8) Next, we need to replace all the newline endings so that we get one big blob of text.  To do that in a `regex` find/replace we need `find=$\n  replace= `  The "replace" character is just a space, so you are chaging all line endings to a single space rather than a newline character.  The find/replace widget should look something like this:

![](documents/images/2024-09-18-15-25-10.png)

Note that this IS a `regex` find/replace!  The `.*` control is turned ON.  

9) OK, now we need to break all the speaker changes into individual lines.  Begin by finding the names of the speakers, usually they appear with a space before the name and a colon after, like ` Smith:`.  The find/replace for this is a bit complex but not a regex.  Assuming a speaker name of `Smith` the find/replace will be:  `find= Smith:   replace=<shift return>,Smith,`  To achieve this when you enter the replace string begin by holding down the shift key and hitting return.  Note the space before the name in the `find` string, with no spaces in the `replace` string.  Your widget should look something like this:

![](documents/images/2024-09-18-15-30-59.png)

10) Repeat step 9 for each speaker.  It should be easy with the find/replace widget open to change just the speaker name in both the `find` and `replace` operations and execute the change.

11) The final step is to review and remove all the page numbers.  Page numbers are typically formatted like `-1-`, `-2-`, ...`-20`, etc.  You can easily find these with a `regex` find/replace with `find=-\d+-  replace=<nothing>`  The replace field should contain no characters, not even a blank space, and your widget should look like this:

![](documents/images/2024-09-18-15-35-50.png)

Page breaks sometimes don't scan well, so it can be helpful to click through the page numbers BEFORE making changes (use the down arrow in the widget to do so).  Examine each one and the text around it, comparing what you see with the same location in the PDF displayed in Adobe Acrobat.  Make manual changes to the CSV text if needed since the scans are sometimes wonky around the page numbers.  

12) Copy and paste the following header line into the top of your CSV file as a new line:

```
timestamp,speaker,words,tags,highlight,timelink 
```

That will provide the necessary headings in your CSV file and for all practical purposes YOU ARE DONE!  Just be sure to save the file and send a copy to mcfatem@grinnell.edu for incorporation into the project.

THANK YOU! 



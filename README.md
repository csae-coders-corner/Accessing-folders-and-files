# Accessing-folders-and-files
The stata package fs, written by Nicholas Cox (Durham University), allows us to access files from a given folder using the command “fs” and also allows us to access multiple folders in the directory using the command “folders”. These commands can be used to access several files and folders of a particular type, especially when the number of files is huge or if they are stored across different folders. 

Suppose we specify a working directory and wish to import files of a particular type from across different folders within the directory. To illustrate this, let’s assume that we want to access all dta files which contain the phrase “corner” from folders which contain the phrase “coders”. For now, let’s also assume that we want to access these files to rename a variable “oldname” present in some of them and then save them in the main directory. The following code can be run in order to do this- 

clear all
ssc install fs

global directory “this is the path name for the main directory which contains all folders”
cd "$directory"

folders "*coders*"  // this is how we access the folders which contain the phrase “coders” 

foreach folder in `r(folders)' {

cd “$directory/`folder’" // we are now working inside the folder labelled `folder'

fs “*corner*” // this is how we access the files which contain the phrase “corner”

foreach file in `r(files)'  {

clear

cd "$directory/`folder'"

use  `file’

capture rename oldname newname  //using capture since we’re not sure if the variable is present

cd “$directory" //we can save the dta files in the main directory 

save `file', replace 
}
}

clear

In this simple example, we used the commands folders and fs to access dta files of a particular type across different folders of a particular type, rename a variable (if present) and then save them in the main directory. In general, these commands can be used to access any file type by identifying them, not only with a contained phrase, but also with a prefix or suffix. For instance, if we want to access all csv files we can just type fs “*.csv” or if we want to access all folders which begin with the phrase “coders” we can just type folders “coders*”. This is especially useful when we aren’t sure about the location of certain files and want to access them without having to check manually. 

**Vatsal Khandelwal, DPhil Candidate in Economics, Linacre College, Oxford**
**03 June 2019**

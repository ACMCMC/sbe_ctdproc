# sbe_ctdproc
A set of scripts to help initialize directories and scripts for basic CTD processing using<br>
the SBE Data Processing programs on Windows, OSX and linux.<br>
Use wine to install and run the SBE Data Processing software on *nix systems.<br>

The batch script that "SBEBatch.exe" takes, "sbe_batch.dat" is embedded in the "process_ctd" script<br>
and is created on the fly each time process_ctd is used.<br> This allows one to use the scripting 
capabilities of the operating system to enhance the SBEBatch processing.<br>

Disclaimer
==========
This repository is a scientific product and is not official communication of the National Oceanic and
Atmospheric Administration, or the United States Department of Commerce. All NOAA GitHub project code is
provided on an ‘as is’ basis and the user assumes responsibility for its use. Any claims against the Department of
Commerce or Department of Commerce bureaus stemming from the use of this GitHub project will be governed
by all applicable Federal law. Any reference to specific commercial products, processes, or services by service
mark, trademark, manufacturer, or otherwise, does not constitute or imply their endorsement, recommendation or
favoring by the Department of Commerce. The Department of Commerce seal and logo, or the seal and logo of a
DOC bureau, shall not be used in any manner to imply endorsement of any commercial product or activity by
DOC or the United States Government.


Contents
========
Included are two scripts to initialize a ctd processing environment and a folder with<br>
raw data files for station 004 of a Western Boundry Time Series(WBTS) cruise<br>
carried out in May of 2017.<br>

*  init_ctd_proc       A BASH script to process ctd data.
*  init_ctd_proc.bat   A BATCH script to process ctd data.
*  psa_1db_2nix        A BASH script that encodes 1db PSA files for inclusion in the process_ctd script.
*  psa_1hz_2nix        A BASH script that encodes 1hz PSA files for inclusion in the process_ctd script.



raw_data<br>

*  AB1705_004.bl         Bottle trip file
*  AB1705_004.hdr        Header file
*  AB1705_004.hex        Raw hex data file
*  AB1705_004.mrk        Markscan file
*  AB1705_004.XMLCON     Configuration file



Requirements
============

The SeaBird processing software installed in one of the default locations.<br>


If using wine.<br>
```bash
$HOME/.wine/drive_c/Program Files\ \(x86\)/Sea-Bird
$HOME/.wine/drive_c/Program\ Files/Sea-Bird
```

If using Windows<br>
```windowsbatch
C:\Program Files (x86)\Sea-Bird
C:\Program Files\Sea-Bird
```
These scripts were written to process files that were created with the version of SeaSave that creates<br>
configuration files with the XMLCON extension. I think SeaBird Began using the XMLCON file with version 7.<br>
The scripts also require that the raw data files be created with the following file naming convention.<br>

CRUISE_ID_nnn<br>

CRUISE_ID is the cruise designation and nnn is the three digit station number. The two must be seperated by an underscore.<br> 

Obviously all this can be modified to suite your needs, so please do so.<br>
You can  modify the actual initilization script so that all cruises you initialize have your modified parameters or you<r>
can modify the config and process_ctd files after initializing a cruise.<br>
   




Example
=======

In this example we'll prepare an environment to process CTD station 004 from cruise AB1705.<br>

1) Open a terminal and navigate to the install scripts.<br>
You should see two files.<br>
*  init_ctd_proc
*  init_ctd_proc.bat
   
On linux/OSX<br>

Make the script executable. you should only have to do this once.<br>
```bash
chmod +x ./init_ctd_proc
```
Copy and paste the following to initialize the environment<br>
```bash
./init_ctd_proc AB1705 $HOME/data
```

Response<br>

```
This is now configured for basic CTD processing for cruise AB1705. Make sure wine is
installed and that the SeaBird processing software is installed and running.
This script assumes that wine is installed in the default prefix $HOME/.wine
and that SBEBatch.exe is installed in $HOME/.wine/drive_c/Program Files/Sea-Bird/SBEDataProcessing-Win32
If they arn't, modify config.cfg and process_ctd with the correct paths.

Place the raw CTD files in $HOME/data/ctd_proc/raw_data
To process navigate to $HOME/data/ctd_proc/batch_files and type ./process_ctd nnn
where nnn is the station number to process.
```
   
This will install the processing script with it's configuration file and PSA files.<br>

Copy the included raw data files to<br>
$HOME/data/ctd_proc/raw_data<br>

Navigate to <br>
$HOME/ctd_proc/batch_files using the terminal.<br>
   
to process station 004 type<br>
```bash
./process_ctd 004
```
   
The SBEBatch should run and deposit the processed files in<br>
```   
$HOME/data/ctd_proc/1db/proc_data
AB1705_004.cnv
AB1705_004.ros
dAB1705_004.cnv
uAB1705_004.cnv
   
$HOME/data/ctd_proc/1hz/proc_data
AB1705_004.cnv
```  
On Windows<br>

1) Open a terminal and navigate to the install scripts.<br>
You should see two files.<br>
*  init_ctd_proc
*  init_ctd_proc.bat

```winbatch
init_ctd_proc AB1705 c:\users\Public\Documents\data
```

Response<br>
```
This is now configured for basic CTD processing for cruise AB1705.
Make sure the SeaBird processing software is installed and running.
This script assumes that SBEBatch.exe is installed in either
C:\Program Files (x86)\Sea-Bird\SBEDataProcessing-Win32
or C:\Program Files\Sea-Bird\SBEDataProcessing-Win32.

Place the raw CTD files in c:\users\Public\Documents\data\ctd_proc\raw_data
To process navigate to c:\users\Public\Documents\data\ctd_proc\batch_files and type process_ctd nnn
where nnn is the station number to process.
```


This will install the processing script with it's configuration file and PSA files.<br>
Copy the included raw data files to<br> 
c:\users\Public\Documents\data\ctd_proc\raw_data<br> 

Using the terminal navigate to <br>
c:\users\Public\Documents\data\ctd_proc\batch_files<br>
   
to process station 004 type<br>

```winbatch
process_ctd 004
```

The SBEBatch should run and deposit the processed files in<br>
```   
c:\users\Public\Documents\data\ctd_proc\1db\proc_data
AB1705_004.cnv
AB1705_004.ros
dAB1705_004.cnv
uAB1705_004.cnv
   
c:\users\Public\Documents\data\ctd_proc\1hz\proc_data
AB1705_004.cnv
```

Generating PSA files.
=====================

The two initialization scripts init_ctd_proc and init_ctd_proc.bat,<br>
generate PSA files for 1hz and 1db processing. To build these two scripts,<br>
PSA files were encoded into the scripts by surrounding the PSA data<br>
with shell commands, arguments and variables.<br>

If you want to use PSA files other than the ones generated by the script,<br>
you could overwrite the PSA files that are created after running the init_ctd_proc scripts<br>
or you could replace the code inside the init_ctd_proc scripts.<br>


There are two scripts included that can help you do this.<br>
Unfortunately these conversion scripts only work under BASH so you can't<br>
use them in a windows CMD terminal. I guess if you have Windows 10 you could<br>
enable WSL and install Ubuntu from the windows store but I have not<br>
tested that.<br>


These scripts would be very difficult for me to write as Batch scripts.<br>


-psa_1db_2nix   Generates the 1db code for both windows Batch and BASH<br>
-psa_1hz_2nix   Generates the 1hz code for both windows Batch and BASH<br>

Make the scripts executable.<br>

```bash
chmod +x psa_1db_2nix
chmod +x psa_1hz_2nix
```
Run the scripts<br>

```bash
./psa_1db_2nix path_2_1db_PSA_Files
./psa_1db_2nix path_2_1hz_PSA_Files
```
Four files will be generated.<br>

copy and paste the contents of nix_psa_1db.txt into init_ctd_proc<br>
copy and paste the contents of win_psa_1db.txt into init_ctd_proc.bat<br>

copy and paste the contents of nix_psa_1hz.txt into init_ctd_proc<br>
copy and paste the contents of win_psa_1hz.txt into init_ctd_proc.bat<br>

Now you can run the init_ctd_proc scripts with the new PSA files.<br>





Additional Notes
================

You'll notice that an SBE batch file doesn't exist before you process a cast for the first time.<br>
This is because the process_ctd scripts generate "sbe_batch.dat" when run.<br> 
A new file is generated every time so any changes made to the file will be destroyed on the next run.<br>

The reason behind this is so that you can use the scripting capabilities of the underlying OS instead of the limited <br>
options available with the SBE batch scripting.<br>

Below is the BASH version of process_ctd. you can use standard BASH to manipulate the creation of the "sbe_batch.dat" file.<br>
The same can be done with the BATCH version for Windows.<br>
```bash
#!/bin/bash
# Edit this file to setup ctd processing using wine.

# This line brings in the variable values
source config.cfg

echo "Checking for SBEBatch.exe in $SBE_DIR"
echo """"
if [ ! -f "$SBE_DIR/SBEBatch.exe" ]; then
    echo ""
    echo "SBEBatch.exe not found!"
    SBE_DIR="/home/user/.wine/drive_c/Program Files/Sea-Bird/SBEDataProcessing-Win32"
else
    echo ""
    echo "Found SBEBatch.exe!"
fi


echo "Checking for SBEBatch.exe in $SBE_DIR"
echo """"
if [ ! -f "$SBE_DIR/SBEBatch.exe" ]; then
    echo ""
    echo "The SeaBird processing software doesnt seem to be installed."
    echo "please install it in the default location and try again."
    exit 1
else
    echo ""
   echo "Found SBEBatch.exe!"
fi
echo ""
########################################################################
# The basename that will be used to assemble the filenames needed.     #
# This corresponds to %2 in the sbe_batch.dat script.                  #
#                                                                      #
BASENAME="$CRUISE_ID"_"$1"
########################################################################

echo "@ %1 Path to the raw data files." > $BATCH_FILE
echo "@ %2 Basename used to create the filenames." >> $BATCH_FILE
echo "@ %3 Path where the processed 1db files will be placed." >> $BATCH_FILE
echo "@ %4 Path where the processed 1hz files will be placed." >> $BATCH_FILE
echo "@ %5 Path to the 1db psa files." >> $BATCH_FILE
echo "@ %6 Path to the 1hz psa files." >> $BATCH_FILE
echo "@ %7 Path where the bottle files will be placed." >> $BATCH_FILE
echo "" >> $BATCH_FILE
echo "" >> $BATCH_FILE
echo "@DatCNVW AlignCTDW WildEditW FilterW CellTMW LoopEditW RosSumW DeriveW BinAvgW StripW TransW SplitW" >> $BATCH_FILE
echo "" >>$BATCH_FILE
echo "@@@@@@@@@@@@@@@@@@" >> $BATCH_FILE
echo "@ 1DB PROCESSING @" >> $BATCH_FILE
echo "@@@@@@@@@@@@@@@@@@" >> $BATCH_FILE
echo "" >> $BATCH_FILE
echo "datcnv      /i%1/%2.hex   /p%5/DatCnv.psa      /f%2.cnv /o%3    /c%1/%2.xmlcon" >> $BATCH_FILE
echo "@markscan   /i%1/%2.mrk   /p%5/MarkScan.psa    /f%2.cnv /o%3" >> $BATCH_FILE
echo "alignctd    /i%3/%2.cnv   /p%5/AlignCTD.psa    /f%2.cnv /o%3" >> $BATCH_FILE
echo "wildedit    /i%3/%2.cnv   /p%5/WildEdit.psa    /f%2.cnv /o%3" >> $BATCH_FILE
echo "filter      /i%3/%2.cnv   /p%5/Filter.psa      /f%2.cnv /o%3" >> $BATCH_FILE
echo "loopedit    /i%3/%2.cnv   /p%5/LoopEdit.psa    /f%2.cnv /o%3" >> $BATCH_FILE
echo "celltm      /i%3/%2.cnv   /p%5/CellTM.psa      /f%2.cnv /o%3" >> $BATCH_FILE
echo "rossum      /i%3/%2.ros   /p%5/BottleSum.psa   /f%2.btl /o%7    /c%1/%2.xmlcon" >> $BATCH_FILE
echo "derive      /i%3/%2.cnv   /p%5/Derive.psa      /f%2.cnv /o%3    /c%1/%2.xmlcon" >> $BATCH_FILE
echo "binavg      /i%3/%2.cnv   /p%5/BinAvg.psa      /f%2.cnv /o%3" >> $BATCH_FILE
echo "trans       /i%3/%2.cnv   /p%5/Trans.psa       /f%2.cnv /o%3" >> $BATCH_FILE
echo "split       /i%3/%2.cnv   /p%5/Split.psa       /f%2.cnv /o%3" >> $BATCH_FILE
echo "" >> $BATCH_FILE
echo "@@@@@@@@@@@@@@@@@@" >> $BATCH_FILE
echo "@ 1HZ PROCESSING @" >> $BATCH_FILE
echo "@@@@@@@@@@@@@@@@@@" >> $BATCH_FILE
echo "" >> $BATCH_FILE
echo "@DatCNVW AlignCTDW WildEditW FilterW CellTMW BinAvgW StripW TransW" >> $BATCH_FILE
echo "" >> $BATCH_FILE
echo "datcnv      /i%1/%2.hex   /p%6/DatCnv.psa      /f%2.cnv /o%4    /c%1/%2.xmlcon" >> $BATCH_FILE
echo "alignctd    /i%4/%2.cnv   /p%6/AlignCTD.psa    /f%2.cnv /o%4" >> $BATCH_FILE
echo "wildedit    /i%4/%2.cnv   /p%6/WildEdit.psa    /f%2.cnv /o%4" >> $BATCH_FILE
echo "filter      /i%4/%2.cnv   /p%6/Filter.psa      /f%2.cnv /o%4" >> $BATCH_FILE
echo "celltm      /i%4/%2.cnv   /p%6/CellTM.psa      /f%2.cnv /o%4" >> $BATCH_FILE
echo "binavg      /i%4/%2.cnv   /p%6/BinAvg.psa      /f%2.cnv /o%4" >> $BATCH_FILE
echo "trans       /i%4/%2.cnv   /p%6/Trans.psa       /f%2.cnv /o%4" >> $BATCH_FILE


# wine is used to run the SBEBatch.exe program that will do the 
# processing. Seven arguments are passed to the sbe_batch.dat script.
# this order is important. If the order is changed, make sure to make
# the changes in the sbe_batch file.

wine  "$SBE_DIR"/SBEBatch.exe \
      "$BATCH_FILE" \
      "$RAW_DIR" \
      "$BASENAME" \
      "$PROCDATA_1DB_DIR" \
      "$PROCDATA_1HZ_DIR" \
      "$PSA_1DB"  \
      "$PSA_1HZ" \
      "$BOTTLE_DIR"
```      










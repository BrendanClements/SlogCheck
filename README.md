# SlogCheck
Checks for SSLKEYLOGFILE environment variable to disable it and delete the corresponding log file if needed.


Background: When doing a packet capture using Wireshark, web traffic secured by TLS will be encrypted leaving the
data unavailable for review. This can make some objectives difficult, if not impossible to achieve. A solution is
to create an environment variable labled SSLKEYLOGFILE and have it point to a directory/file for the TLS keys to
be stored.

Purpose: This script, when executed by a batch file, will check if the SSLKEYLOG environment variable is set. If
the variable exists, the user is asked if they would like it disabled. A new variable is set (SSLKEYLOG_DISABLED)
with the same directory/file name as read from the existing SSLKEYLOG variable. This disables the SSL key logging
while making it easy for the user to enable it again when needed. The original SSLKEYLOG variable is then removed
before checking to see if an SSL key log file exists. That file will then be deleted if directed by the user.

Usage:
--Create a Startup.bat file that will execute the powershell script with the following two lines:

@echo off
Powershell -noprofile -executionpolicy bypass -file "C:\Users\<UserName>\<Directory>\SLogCheck.ps1"

--Save the .bat file and note the location. Start the task scheduler (Win+R > taskschd.msc).
--Click 'Action' > 'Create Task'
--Type "SlogCheck" for the name and type a description (don't skip the description).
--Click the 'Triggers' tab.
--Select 'At log on' from the 'Begin the task' dropdown.
--Click 'OK' on the 'New Trigger' box.
--Click the 'Actions' tab.
--Click 'New.'
--Browse for the Startup.bat file.
--Click 'OK' on the 'New Action' box. followed by "OK" on the 'Create Task' box.


Notes: This PowerShell script will not work if it's executed by itself. Environment variables can't be changed
via PowerShell by default and it isn't a good idea to change that. Make sure the directory/file in the Startup.bat
file accurately points to the SlogCheck.ps1 file and make sure the task scheduler is executing the Startup.bat
file. With the @echo off line, there shouldn't be any indication the script ran if the SSLKEYLOGFILE variable isn't
set.
#>

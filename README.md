# command-check-bat-file
# Overview
This is a sample batch file for checking the installation status of any command.  
If installed, it displays version information.  
Commands that do not exist will display "NOT INSTALLED".  

# Execution Results
Here are the execution results.
The commands java, mvn, quasar, npm, and aws are installed.  
The command not_found (a fictitious command) is not installed.  
![Screenshot 2024-02-09 021500.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1618333/aaec8cda-7cce-21c2-2a21-04ad1b7acee7.png)

```Batchfile
@echo off
setlocal enabledelayedexpansion

echo [94m----------------------------[0m
echo [94mVerify command installation.[0m
echo [94m----------------------------[0m

REM check list.
set "commands=java mvn quasar npm aws not_found"

REM version command.
set "version_options=java:-version mvn:-v quasar:--version npm:-v aws:--version not_found:--version"

REM command check.
for %%i in (%commands%) do (
    set "version_option="
    for %%v in (%version_options%) do (
        for /f "tokens=1,2 delims=:" %%a in ("%%v") do (
            if "%%a"=="%%i" set "version_option=%%b"
        )
    )

    where "%%i" >nul 2>&1
    if not errorlevel 1 (
        echo [32mINSTALLED    %%i[0m

        set "version_info="
        for /f "delims=" %%x in ('%%i !version_option! 2^>^&1') do (
            if "!version_info!"=="" (
                echo              %%x
            ) else (
                echo              %%x
            )
        )
    ) else (
        echo [91m[1m[7mNOT INSTALLED %%i[0m
    )
)

cmd /k echo end file.

```
# How to Add Other Commands
Add the command you want to check to commands.  
Add the version check option for the command you want to check to version_options.  
For example, in this case, java is executed with java -version, and mvn with mvn -v.  

```Batchfile
REM List of commands to check
set "commands=java mvn quasar npm aws not_found"

REM Options to retrieve version information for each command
set "version_options=java:-version mvn:-v quasar:--version npm:-v aws:--version not_found:--version"
```

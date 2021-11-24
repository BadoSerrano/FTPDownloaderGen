#!/usr/bin/env python3

import ftplib
import os
import sys
import time

class color:
    pink = '[1m[95m'
    blue = '[1m[94m'
    cyan = '[1m[96m'
    green = '[1m[92m'
    yellow = '[1m[93m'
    red = '[1m[91m'
    end = '[0m'
    bold = '[1m'
    underline = '[4m'

address = "ftpDOTmyaddressDOTcom"
user = "myusernameATmyaddressDOTcom"
pswrd = "myPasSword1234"

timer = "6"

extension = "zip"

def check_history():
    if histFile not in contents:
        print(color.red + "No FTP history found, searching for local history" + color.end)
        if os.path.isfile(os.path.join(sys.path[0], histFile)) == False:
            print(color.yellow + "No local history found, creating new history file" + color.end)
            newHistory = open(os.path.join(sys.path[0], histFile), 'x')
            print(color.green + "New history file created" + color.end)
        ftp.storbinary('APPE ' + histFile, open(os.path.join(sys.path[0], histFile), 'rb'))
        print(color.cyan + "FTP history file updated with local history" + color.end)
    else:
        print(color.green + "FTP history found" + color.end)
        ftp.retrbinary('RETR ' + histFile, open(os.path.join(sys.path[0], histFile), 'wb').write)
        print(color.cyan + "Local history file updated with FTP history" + color.end)

def history_download():
    history = open(os.path.join(sys.path[0], histFile), 'r').read()
    for file in contents:
        if file.endswith(extension) and not os.path.isdir(file) and str(file) not in history:
            print(color.bold + 'searching for new files...' + color.end)
            ftp.retrbinary('RETR ' + file, open(os.path.join(sys.path[0], file), 'wb').write)
            print(color.green + 'File ' + str(file) + ' downloaded successfully' + color.end)
            append = open(os.path.join(sys.path[0], histFile), 'a').write(str(file) + '\n')
            print(color.pink + 'File ' + str(file) + ' added to local history' + color.end)
    ftp.storbinary('STOR ' + histFile, open(os.path.join(sys.path[0], histFile), 'rb'))
    print(color.green + "FTP history file updated" + color.end)

while True:
    with ftplib.FTP(address) as ftp:
        print(color.blue + "Running script" + color.end)
        ftp.login(user, pswrd)
        print(color.green + "Login Successful" + color.end)
        ftp.cwd("public_html/myFolder/myFTPplace/specificPath")
        contents = ftp.nlst()

        histFile = "history" + str.upper(extension) + ".txt"
        check_history()
        history_download()

    print(color.cyan + "Restarting in " + timer + " seconds" + color.end)
    time.sleep(int(timer))

#!/usr/bin/env python3
import os
import sys


#-----------------------


# 1. USER PROMPTS


# prompt for FTP connection info and filter out quotes
ftpVariables = [
input("FTP Address (no quotes): "),
input("FTP User (no quotes): "),
input("FTP Password (no quotes): "),
input("FTP Directory Path (no quotes): ")
]

quotes = ['''"''', "'"]

for i in range(len(ftpVariables)):
    for j in range(len(quotes)):
        if quotes[j] in ftpVariables[i]:
            ftpVariables[i] = ftpVariables[i].replace(quotes[j], "")

address = ftpVariables[0]
user = ftpVariables[1]
pswrd = ftpVariables[2]
path = ftpVariables[3]


# bool variables
boolTimer = None
boolFixedTimer = None
boolExtension = None
boolFixedExtension = None
boolHistory = None


# Option 1: add loop and timer? yes/no + fixed loop yes/no
while True:
    promptTimer = str.lower(input("Add loop and timer (y/n): "))
    if promptTimer in ("y", "yes"):
        boolTimer = True
        while True:
            promptFixedTimer = str.lower(input("Fixed timer (always the same) (y/n): "))
            if promptFixedTimer in ("y", "yes"):
                boolFixedTimer = True
                while True:
                    fixedTimer = input("Fixed time between loops (seconds): ")
                    if not str.isdecimal(fixedTimer):
                        continue
                    else:
                        break
                break
            elif promptFixedTimer in ("n", "no"):
                boolFixedTimer = False
                break
        break
    elif promptTimer in ("n", "no"):
        boolTimer = False
        boolFixedTimer = False
        break


# Option 2: choose extension types(s)? yes/no + fixed extension yes/no
while True:
    promptExtension = str.lower(input("Choose extension type(s) (y/n): "))
    if promptExtension in ("y", "yes"):
        boolExtension = True
        while True:
            promptFixedExtension = str.lower(input("Fixed extension (always the same) (y/n): "))
            if promptFixedExtension in ("y", "yes"):
                boolFixedExtension = True
                fixedExtension = str.lower(input("Fixed extension: "))
                if "." in fixedExtension:
                    fixedExtension = fixedExtension.replace(".", "")
                break
            elif promptFixedExtension in ("n", "no"):
                boolFixedExtension = False
                fixedExtension = ""
                break
        break
    elif promptExtension in ("n", "no"):
        boolExtension = False
        boolFixedExtension = False
        break


# Option 3: add history mode? yes/no
while True:
    promptHistory = str.lower(input("History Mode (y/n): "))
    if promptHistory in ("y", "yes"):
        boolHistory = True
        break
    elif promptHistory in ("n", "no"):
        boolHistory = False
        break


# choose filename and filter out spaces and extensions
name = input("Choose filename (no spaces or extensions): ")
if " " in name:
    name = name.replace(" ", "")
if "." in name:
    name = name.replace(".", "")


#--------------------------


# 2. SCRIPT WRITING:


# Open custom script in append mode
script = open(os.path.join(sys.path[0], name), 'a')


# write imports + color class
script.write('''#!/usr/bin/env python3

import ftplib
import os
import sys
''')

if boolTimer:
    script.write("import time\n")

script.write('''
class color:
    pink = '\033[1m\033[95m'
    blue = '\033[1m\033[94m'
    cyan = '\033[1m\033[96m'
    green = '\033[1m\033[92m'
    yellow = '\033[1m\033[93m'
    red = '\033[1m\033[91m'
    end = '\033[0m'
    bold = '\033[1m'
    underline = '\033[4m'
''')


# write variables
script.write('''
address = "{}"
user = "{}"
pswrd = "{}"
'''.format(str(address), str(user), str(pswrd)))

if boolTimer and boolFixedTimer:
    script.write('''
timer = "{}"
'''.format(fixedTimer))

if boolTimer and not boolFixedTimer:
    script.write('''
timer = input(color.green + "Choose interval between scripts(seconds): " + color.end)
''')

if boolExtension and not boolFixedExtension:
    script.write('''
extension = str.lower(input(color.green + "Please choose an extension: " + color.end))
if "." in extension:
    extension = extension.replace(".", "")
''')

if boolExtension and boolFixedExtension:
    script.write('''
extension = "{}"
'''.format(fixedExtension))

if not boolExtension:
    script.write('''
extension = ""
''')


# write functions
if boolHistory:
    script.write('''
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
            append = open(os.path.join(sys.path[0], histFile), 'a').write(str(file) + {})
            print(color.pink + 'File ' + str(file) + ' added to local history' + color.end)
    ftp.storbinary('STOR ' + histFile, open(os.path.join(sys.path[0], histFile), 'rb'))
    print(color.green + "FTP history file updated" + color.end)
'''.format(repr("\n")))

if not boolHistory:
    script.write('''
def simple_download():
    for file in contents:
        if file.endswith(extension) and not os.path.isdir(file):
            print(color.bold + 'searching for files...' + color.end)
            ftp.retrbinary('RETR ' + file, open(os.path.join(sys.path[0], file), 'wb').write)
            print(color.green + 'File ' + str(file) + ' downloaded successfully' + color.end)
''')


# write procedural code
if boolTimer:
    script.write('''
while True:
    with ftplib.FTP(address) as ftp:
        print(color.blue + "Running script" + color.end)
        ftp.login(user, pswrd)
        print(color.green + "Login Successful" + color.end)
        ftp.cwd("{}")
        contents = ftp.nlst()
'''.format(str(path)))

if boolTimer and boolHistory:
    script.write('''
        histFile = "history" + str.upper(extension) + ".txt"
        check_history()
        history_download()
''')

if boolTimer and not boolHistory:
    script.write('''
        simple_download()
''')

if boolTimer:
    script.write('''
    print(color.cyan + "Restarting in " + timer + " seconds" + color.end)
    time.sleep(int(timer))
''')

if not boolTimer:
    script.write('''
with ftplib.FTP(address) as ftp:
    print(color.blue + "Running script" + color.end)
    ftp.login(user, pswrd)
    print(color.green + "Login Successful" + color.end)
    ftp.cwd("{}")
    contents = ftp.nlst()
'''.format(str(path)))

if not boolTimer and boolHistory:
    script.write('''
    histFile = "history" + str.upper(extension) + ".txt"
    check_history()
    history_download()
''')

if not boolTimer and not boolHistory:
    script.write('''
    simple_download()
''')


# convert file into executable
path = os.path.join(sys.path[0], name).replace(" ", "\ ")
os.system("chmod 755 " + path)

#!/usr/bin/env python3.5
# -*- coding: utf-8 -*-
"""
Created on Mon Feb 13 22:45:25 2017

@author: hhemied
"""

import os
import sys
import subprocess
from shutil import copyfile
import hashlib


Red = '\033[91m'
Green = '\033[92m'
Default = '\033[0m'

LIB = "/usr/lib/vmware/lib/libvmwareui.so/libvmwareui.so"
BS = "/etc/vmware/bootstrap"
useLibs = "VMWARE_USE_SHIPPED_LIBS"

vmmon = "/usr/lib/vmware/modules/source/vmmon.tar"
vmnet = "/usr/lib/vmware/modules/source/vmnet.tar"

"""
building md5sum function
"""

def md5(fname):
    hash_md5 = hashlib.md5()
    with open(fname, "rb") as f:
        for chunk in iter(lambda: f.read(4096), b""):
            hash_md5.update(chunk)
    return hash_md5.hexdigest()


"""
checking VMWare Workstation installed or not
"""
print("\nChecking VMWare workstation ...")
if os.path.exists(LIB):
    print("libvmwareui.so present, continuing...")
else:
    print(Red, " VMWare Workstation not detected, exit!", Default)
    sys.exit(1)
    

"""
Bootstrapping VMWare bundle libs
"""
print("Writing necessary values to {} if needed".format(BS))

if os.path.exists(BS):
    if useLibs in open(BS).read():
        print("Bootrap file \"{}\" has the value of \"{}\"".format(BS, useLibs))
    else:
        with open(BS, 'a') as file:
            file.write("export VMWARE_USE_SHIPPED_LIBS=force")



"Manipulating FS to build its structure"
mylibs = subprocess.check_output("rpm -ql glib2|grep '/usr/lib64/libg.*so\.0$'", shell=True)
for mylib in mylibs.split():
    tgtlib = os.path.join("/usr/lib/vmware/lib/", os.path.basename(mylib.decode("utf-8")), os.path.basename(mylib.decode("utf-8")))
    if not os.path.exists(tgtlib + ".back"):
        copyfile(tgtlib, tgtlib + ".backup")
    print("Manipulating {} ....".format(tgtlib))
    copyfile(mylib.decode("utf-8"), tgtlib)
    

print("Fixing Networking issue ...")

if md5(vmmon) != md5("vmmon.tar"):
    copyfile("./vmmon.tar", vmmon)
if md5(vmnet) != md5("vmnet.tar"):
    copyfile("./vmnet.tar", vmnet)


print(Green, "Enjoy VMWare WorkStation", Default)
        
        

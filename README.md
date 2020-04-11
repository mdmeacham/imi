# A python library for accessing IGEL's IMI

# Dependancies
This library depends on the Python requests library.  Install requests with:

``` {.sourceCode .python}
pip install requests
```
# Installation
After installing requests, install this library like this:
``` {.sourceCode .python}
pip install pyimi
```

# Some example code

``` {.sourceCode .python}
from pyimi import IMI, Devices, Directories, Profiles
import sys

# First, create your IMI session
imi = IMI(server='192.168.56.12', user='igel', password='igel#123')

# retrieve a list of your devices
devices = Devices(imi)

# and also a list of your directories
directories = Directories(imi)

# and also a list of your Profiles
profiles = Profiles(imi)

# you can iterate through the retreived items
for device in devices:
    print(device.name)

for directory in directories:
    print(directory.name)

for profile in profiles:
    print(profile.name)

# Or, to find a paricular device, you can search
# by name, IP address, or MAC
device1 = devices.find(name="ITC080027B8A48E")
device2 = devices.find(ip="192.168.56.104")
#device3 = devices.find(mac="080027B8A48E")

if device1:
    print("device1 IP address is", device1.ip)

if device2:
    print("device2 name is", device2.name)

# Check if your device is online like this
print("Is device1 online?", device1.online)
print("Is device2 online?", device2.online)

# You can find a directory by name
my_directory = directories.find(name="Portland")
if my_directory:
    print("Found directory")
else:
    print("directory was not found")

# Create a new directory for devices like this
directories.create("Vancouver")

# Now that you have both a device and a directory,
# you can move the device into that directory
device1.move(my_directory)

# And you can also move a device directory into another directory
my_directory2 = directories.find(name="Bend")
my_directory2.move(my_directory)

# Here's how you assign a profile to a device or a directory
browser_profile = profiles.find(name='Browser')
device1.assign(browser_profile)
my_directory.assign(browser_profile)

# And then you can unassign a profile like this
device1.unassign(browser_profile)
my_directory.unassign(browser_profile)


# Get some additional information about your device
device1.get_info()
print("A smaller set of device info includes:", device1.info)

# Get more detailed information
device1.get_info(detailed=True)
# Now, device info shows more
print("A much larger set of device info includes:", device1.info)

# Run some commands on your device
device1.reboot()
device1.shutdown()
device1.wakeup()
device1.settings2tc()
#device1.factory()

# For now, when you factory a device
# you should retrieve all devices again to have a
# valid list
```
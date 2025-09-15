# JamfRestrictions
To be used with powershell 5

Top bundle is self explanatory

**Which profile?**
ios or macos - this variable helps with the path as ios follows the iOSConfigurationProfiles path, whereas macos follows OSXConfigurationProfiles. This allows for us to use the script for either as our base template (not both, as that's too messy)
This is your target config profile. Whether you're adding groups to be exempt or targets, this is the ID of the profile you're dealing with.

**Groups**
Target group you want to have - This will have the config applied
Empty group - This is good as a backup, but it redundant as I am directly changing the config profile, instead of a smart group.

**Exclusions**
Do you have students that are on leave for any reason? Ensure this user group is referenced. If you do have one of these, ensure you are keeping on top of the membership so kids aren't coming back and not having the config applied to them.

**Mode**
Auto - Auto adds/removes based on the next section of times you set
Add - Adds the group membership above
Remove - Removes the target group above, but keeps the exclusion group

**Allowed Windows**
I probably didn't need this but did it anyway in case we wanted to apply multiple variables down the track.
It's simple, Day, Start time and End time of when you want the ADD to be done. Outside of this window, it will remove the target groups from the profile.
Public & School Holidays - Required for EDU clients

If used in EDU, you will want to have the school holidays excluded and public holidays. If there are school pupil free days, you can also add them in here.
There are some other helpers in the script that allow for different time formats, choose the time zone you're applying the config based on (WA is W. Australia Time Zone)


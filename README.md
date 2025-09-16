# JamfRestrictions

PowerShell 5.1 scripts for managing Jamf Pro configuration profile scopes.
This repo contains two independent tools, each solving a different use case.

⸻

## Timetabled-Restrictions

### Purpose
Handles a single configuration profile (macOS or iOS) and automatically switches scope based on time of day, public holidays, and school holiday ranges.

Key Settings
```
ProfileType - ios or macos
ProfileId - the Jamf configuration profile you want to control
TargetGroupId - group applied during allowed times
EmptyGroupId - fallback group applied outside times / on holidays
Optional exclusions - device and/or user group IDs

Modes:
Auto - switch groups based on timetable + holidays
Add - force apply target group
Remove - force apply empty group
```

### Example Config Snippet

### Which profile?
```
$ProfileType  = "ios"
$ProfileId    = 110
```

### Groups
```
$TargetGroupId = 182
$EmptyGroupId  = 42
```

### Exclusions
```
$ExclusionDeviceGroupIds = @(174)
$ExclusionUserGroupIds   = @(10)
```

### Mode
```
$Mode = "Auto"
```

### Allowed Windows
```
$AllowWindows = @(
    @{ Day="Monday";    Start="08:10"; End="14:55"; Label="Monday" }
    @{ Day="Tuesday";   Start="08:10"; End="14:55"; Label="Tuesday" }
)
```
Best for Whole-school or site-wide toggles
> “During school hours, cameras allowed; outside hours/holidays, cameras blocked”

⸻

## Daily-Restrictions

### Purpose
Handles multiple profiles and exclusions, driven from a CSV schedule.
The script checks the current day/time and applies the relevant exclusion groups.

Key Settings
```
CSV defines:
ProfileId
ObjectType (Computer or Mobile)
Days, Start, End
GroupIds - exclusion groups to apply
Name - friendly label
Run with -DryRun to preview changes without applying.
```

Example CSV (cameraoverrides.csv)

```
ProfileId,ObjectType,Days,Start,End,GroupIds,Name
157,Computer,Mon,12:00,13:00,87,07DAN.3
157,Computer,Wed,14:20,15:20,87,07DAN.3
157,Computer,Fri,08:50,09:50,89,08DAN.2
157,Computer,Fri,11:05,12:05,88,08DAN.3
157,Computer,Thu,14:20,15:20,80,ENG7.4
157,Computer,Tue,14:25,15:20,79,ENG8.5
```

Running a Dry Run

```.\Daily-Restrictions.ps1 -DryRun```

This will print the planned adds/removes without touching Jamf.

Best for Class-by-class or period-based restrictions 
> Monday 12:00–13:00 exclude Group 87, Wednesday 14:20–15:20 exclude Group 87, Friday 08:50–09:50 exclude Group 89, etc

⸻

### Choosing the Right Script
  Use Timetabled-Restrictions if you want one profile to follow a daily/holiday timetable.
  Use Daily-Restrictions if you want many profiles to follow a lesson-by-lesson exclusion schedule.
  You can use a combination of both as well if it works in your instance.

Both scripts use Jamf Pro’s Classic API with OAuth token flow and are PowerShell 5.1 compatible.

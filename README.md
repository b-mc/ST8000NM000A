# ST8000NM000A
A dump of low level information about the amazing Seagate Exos 7E 8TB 512e/4Kn Fastformat SATA drive model - for all of you tech-savvy NAS builders. I use both `openseachest` and `seachest` tools and might use them interchangeably.

# Notable features
The drive supports both 512 and 4096 sector sizes. This wasn't that obvious in the older data sheet versions as the drive was listed "just" as a 512e model. It ships with 512e as the default sector size.

# Changing the sector size
## List the supported formats
```
# SeaChest_Format -d /dev/sg0 --showSupportedFormats
==========================================================================================
 SeaChest_Format - Seagate drive utilities - NVMe Enabled
 Copyright (c) 2014-2021 Seagate Technology LLC and/or its Affiliates, All Rights Reserved
 SeaChest_Format Version: 2.3.1-2_2_3 X86_64
 Build Date: Jun 17 2021
 Today: Sun Feb 13 09:38:23 2022	User: root
==========================================================================================

/dev/sg0 - ST8000NM000A-2KE101 - WSDXXXXX - ATA

Supported Logical Block Sizes and Protection Types:
---------------------------------------------------
  * - current device format
PI Key:
  Y - protection type supported at specified block size
  N - protection type not supported at specified block size
  ? - unable to determine support for protection type at specified block size
Relative performance key:
  N/A - relative performance not available.
  Best    
  Better  
  Good    
  Degraded
--------------------------------------------------------------------------------
 Logical Block Size  PI-0  PI-1  PI-2  PI-3  Relative Performance  Metadata Size
--------------------------------------------------------------------------------
*               512     Y     N     N     N                   N/A            N/A
               4096     Y     N     N     N                   N/A            N/A
--------------------------------------------------------------------------------
```

## Change
```
# SeaChest_Format -d /dev/sg0 --setSectorSize 4096 --confirm this-will-erase-data
==========================================================================================
 SeaChest_Format - Seagate drive utilities - NVMe Enabled
 Copyright (c) 2014-2021 Seagate Technology LLC and/or its Affiliates, All Rights Reserved
 SeaChest_Format Version: 2.3.1-2_2_3 X86_64
 Build Date: Jun 17 2021
 Today: Sun Feb 13 09:49:54 2022	User: root
==========================================================================================

/dev/sg0 - ST8000NM000A-2KE101 - WSDXXXXX - ATA
Set Sector Size to 4096

WARNING: It is not recommended to do this on USB enclosures as not
         all USB adapters can handle a 4k sector size.
WARNING (SATA): Do not interrupt this operation once it has started or 
         it may cause the drive to become unusable. Stop all possible background
         activity that would attempt to communicate with the device while this
         operation is in progress
Press CTRL-C to cancel this operation before the timer runs out.
  0
Setting the drive sector size quickly.
Please wait a few minutes for this command to complete.
It should complete in under 5 minutes, but interrupting it may make
the drive unusable or require performing this command again!!
Successfully set sector size to 4096
```

## Verify
```
# SeaChest_Format -d /dev/sg0 --showSupportedFormats
(...)
  * - current device format
(...)
--------------------------------------------------------------------------------
 Logical Block Size  PI-0  PI-1  PI-2  PI-3  Relative Performance  Metadata Size
--------------------------------------------------------------------------------
                512     Y     N     N     N                   N/A            N/A
*              4096     Y     N     N     N                   N/A            N/A
--------------------------------------------------------------------------------
```
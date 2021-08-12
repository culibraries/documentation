# Self-checkout

Procedure to update Sierra self-checkout machines

1. Request OIT move machines from limited access OU via ServiceNow ticket.
1. Update Sierra client through Software Center or download from https://libraries.colorado.edu/web/installers/
1. Get information from old shortcut
    1. Type `Win+r` to open a cmd prompt
    1. Type `shell:common startup` to open the startup folder.
1. Open Sierra shortcut and copy  `“program=milselfcheck username=xxxnor[1,2,3,4] password==xxxnor[1,2,3,4]`
1. Edit new desktop shortcut
    1. Add `"program=milselfcheck” username=xxxnor[1,2,3,4] password==xxxnor[1,2,3,4]` to the shortcut target.
    1. Click Apply
1. The shortcut can be tested at this point if desired, but you will need admin credentials for Sierra to close self-checkout window)
1. Delete the old shortcut from the startup folder and then copy the new desktop shortcut to the startup folder. (Requires super-user credentials requested from OIT via ServiceNow)
1. Restart the machine and confirm self-check program auto-starts
1. Notify OIT to move machines back to limited access OU.

#MSI GE70-2OE-017US

##General
 * Patched BIOS with [PMPatch](https://github.com/NikolajSchlej/PMPatch) to enable native kernel power management. Without this, a modified mach_kernel is required to boot properly (Bronzovka's AMD kernel worked pretty well, but patching would provide the most permanent solution).
 * Wireless does not work. I threw in another card from my old Dell Mini 10v, and it works OOB. You can get the same card [on Amazon](http://www.amazon.com/Broadcom-Wireless-Internet-Adapter-Netbooks/dp/B002OB0FPI/ref=sr_1_1?s=electronics&ie=UTF8&qid=1397847460&sr=1-1) for $7.

##DSDT

Patched to disable GTX765m on startup (no chances it will ever work anyways, unless Apple chooses to adopt Nvidia's Optimus technology):

```python
    Method (WAK, 1, NotSerialized)
    {
        \_SB.PCI0.LPCB.SWAK (Arg0)
        \_SB.PCI0.NWAK (Arg0)
        \_SB.PCI0.LPCB.SIOW (Arg0)
        \_SB.PCI0.PEG0.PEGP.SGOF () // Disable GTX 765m on wake (does not work)
    }

    Method (OSCM, 4, NotSerialized)
    {
        Return (Zero)
    }

    Method (PINI, 0, NotSerialized)
    {
	\_SB.PCI0.PEG0.PEGP.SGOF () // Disable GTX 765m on boot
    }

```
 * If the above patch is not used. You will have to remove the NVDAGK104 kernel extension to boot. It is probably a good idea to remove it anyways as it doesn't serve any purpose
 * Patched to enable AppleHDA (The patched AppleHDA kernel extension is also required)
 * Patched to enable sleep (It takes about 5-10 seconds to go to sleep, and after wake the Nvidia chip turns back on ~98% of the time. Not sure why it does this)

##Kernel Extensions
 * Elan trackpad enabled: http://forum.osxlatitude.com/index.php?/topic/1948-elan-touchpad-driver-mac-os-x/
 * AppleHDA
 * Killer e2200 ethernet [is working](http://www.tonymacx86.com/network/120084-working-kext-killer-e2200.html). Do not put this in /System/Library/Extensions. It goes in /System/Library/Extensions/IONetworkingFamily.kext/Contents/Plugins.

##Boot.plist

  ```xml
  <!-- This is needed for Quartz Extreme and Core Image hardware accelaration on the HD4600 card -->
  <key>GraphicsEnabler</key>
  <string>Yes</string>
  <key>IntelAzulFB</key>
  <string>11</string>
  ```

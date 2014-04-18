#MSI GE70-2OE-017US Hackintosh Resourses

* General
 * Patched BIOS with [PMPatch](https://github.com/NikolajSchlej/PMPatch) to enable native kernel power management. Without this, a modified mach_kernel is required to boot properly (Bronzovka's AMD kernel worked pretty well, but patching would provide the most permanent solution).
* DSDT
 * Patched to disable GTX765m on startup (no chances it will ever work anyways, unless Apple chooses to adopt Nvidia's Optimus technology)
 * Patched to enable AppleHDA (The patched AppleHDA kernel extension is also required)
 * Patched to enable sleep (It takes about 5-10 seconds to go to sleep, and after wake the Nvidia chip turns back on ~98% of the time. Not sure why it does this)
* Kernel Extensions
 * Elan trackpad enabled: http://forum.osxlatitude.com/index.php?/topic/1948-elan-touchpad-driver-mac-os-x/
 * AppleHDA
* Boot.plist
  ```xml
  <!-- This is needed for Quartz Extreme and Core Image hardware accelaration on the HD4600 card -->
	<key>GraphicsEnabler</key>
	<string>Yes</string>
	<key>IntelAzulFB</key>
	<string>11</string>
  ```

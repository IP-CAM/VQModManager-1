# VQMod Manager

VQMod Manager is an OpenCart 1.5.x extension which allows you to centrally manage your VQMod scripts (upload, install/uninstall, delete, troubleshoot) from the OpenCart Admin.

## Installation

Install the latest version of [VQMod](vqmod.com) and upload the contents of the ```/upload``` folder to the root of your OpenCart store.  It will appear in the menu under ```Extension``` -> ```VQMod Manager```.  Make sure your user group has ```access/modify``` permissions for ```module/vqmod_manager``` for access.

## Changes

### v2.0.0
  - Confirms VQMod is properly installed
  - New tab layout
  - Added ability to download vqcache files to assist troubleshooting
  - Fixed error output when a previously uploaded VQMod script has malformed XML
  - Show logging, useCache, cacheTime settings
  - Spelling correction
  - If any issues exist in the VQMod error log the tab will be highlighted
  - "Disabled" VQMod script highlighting
  - VQMod script table zebra stripes for better readability
  - Inline CSS removed
  - Language localization for upload button
  - About tab
  - Updated licensing
# SuperKiosk
Browser usually have an option for being started in "kiosk" mode. The purpose of this is to allow a web application to run on a public computer without the user being able to exit from the application.

The default "kiosk" modes however does not disable all features

## Firefox
Download the file `firefox/policies.json`. This file contains settings to disable problematic features, and must be put in a special directory for Firefox to find it when it starts.

The policy file will disable:
* Disable telemetry (Firefox call-home)
* Disable Firefox Sync (Firefox Account/FxAccount)
* Disable Firefox Studies
* Disable prediction and DNS prefetch
* Disable automatic updates
* Clear everything on default home page
* Disable all touch gesture shortcuts (previous page etc.)
* Disable auto-fill forms
* Disable remembered logins
* Disable warning about insecure connection when filling out forms

Kiosk mode is only available in versions from 2019 and newer.

## Windows/MacOS/Other
Refer to Firefox Enterprise Policy documentation to find the correct place to save the file. Start firefox from a batch file like `firefox.exe --kiosk http://url-to-`

## Linux


## Other browsers
You are welcome to investiagte how to do this with other browsers, like Chrome. Chrome has known issues in kiosk mode with not mapping touch gestures to JavaScript mouse events, and it also prints annyoing "Translate this page" messages.

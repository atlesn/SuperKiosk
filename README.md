# SuperKiosk
Browsers usually have an option for being started in "kiosk" mode. The purpose of this is to allow a web application to run on a public computer without the user being able to exit from it.

The default kiosk modes however does not disable all features, and these features **will sooner or later break your kiosk application**. SuperKiosk is a guide to disable features known to cause problems.

## Firefox
The file `firefox/policies.json` contains settings to disable problematic features, and must be put in a special directory for Firefox to find it when it starts.

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

Note that kiosk mode is only available in versions from 2019 and newer.

## Windows/MacOS/Other
Refer to Firefox Enterprise Policy documentation to find the correct place to save the `policies.json` file.

* Save the file `policies.json` file in the correct directory
* Close all Firefox windows and start it normally
* Go to the page `about:policies` and verify that the policy file has been read
* To start in proper kiosk mode, Firefox must be started from a script/batch file
 * (Windows) Put something like this in a bat or cmd file: `C:\path-to-firefox\firefox.exe -kiosk -private-window http://url-to-application/`
 * (MacOS) *help needed, please create an issue*

It is recommended to have the start script delete all Firefox settings/profiles before each start.

## Linux
The location to save `policies.json` may vary across distributions.

* First, try the default directory:

```
	$ sudo su -
	# mkdir -p /etc/firefox/policies/
	# curl https://raw.githubusercontent.com/atlesn/SuperKiosk/main/firefox/policies.json > /etc/firefox/policies/policies.json
	# exit
```

* Close all Firefox windows and start it normally
* Go to the page `about:policies` and verify that the policy file has been read
* If the file has not been read, `strace` may be used to find the correct location (you might need to install `strace`)
* Close all Firefox windows and run:

```
	$ strace -f firefox 2>&1 | grep policies.json
```

* Look for output like
```
	[pid 444717] access("/etc/firefox/policies/policies.json", F_OK <unfinished ...>
	[pid 444717] openat(AT_FDCWD, "/usr/lib/firefox/distribution/policies.json", O_RDONLY <unfinished ...>
```

* Save the file in one of the specified directores, close Firefox, and re-check `about:policies` again. *Note that it is recommended to always save the file in a directory under `/etc/` (if possible) to avoid that any packaging system overwrites the file.*
* Create a startup-script containing something like `firefox --kiosk --private-window 'http://url-to-application'`

## Other browsers
You are welcome to investiagte how to do this with other browsers, like Chrome. Chrome has known issues in kiosk mode with not mapping touch gestures to JavaScript mouse events, and it also prints annyoing "Translate this page" messages.

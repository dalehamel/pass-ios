Pass for iOS
============

![Icon](https://raw.github.com/rephorm/pass-ios/master/Resources/Icon.png)

View your [password-store][] passwords on your iDevice.

[password-store]: http://zx2c4.com/projects/password-store

Dependencies
------------

The following packages are available from Cydia:

  * gnupg  (required)
  * git    (optional)

Building
--------

Note: This will only work on a jailbroken device.

This will build on the iPhone (read up for cross compiling from OSX/Linux) You'll need to install theos:

1. Open Cydia and install "iOS Toolchain". This will upgrade you from saurik's iOS 2.0 build tools to the latest ones
2. Delete /var/theos if you have an old version
3. Over SSH, type in this: "cd /var && git clone git://github.com/coolstar/theos.git
4. Grab a copy of a newer iOS SDK and place it so that it's in /var/theos/sdks/iPhoneOS.sdk

The iOS SDK can be obtained from your mac after installing Xcode at /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/

Clone this repo, and run "make package" followed by "make package install".

Once the package is installed, you'll need to restart springboard with "killall SpringBoard", and may need to restart your iPhone before the app shows up.

Setup
-----

### Copy your `pass` password-store to /var/mobile/.password-store

The preferred way to do this is to store your passwords in a `git` repository, which you can then clone. Alternatively, you can use scp, iFile or any other method to transfer the passwords over.

### Set up your gpg key

1) Export your *private* key from your desktop/laptop/etc:

    (desktop)$ gpg --export-secret-key --armor ${KEY_ID} > ${KEY_ID}.asc

2) Copy this file to your device

3) On the device, import the key

    (device)$ gpg --import ${KEY_ID}.asc

4) Delete the key file

5) Test decrypting one of your passwords

    (device)$ gpg -d ~/.password-store/ENTRY.gpg

### Using the Pass App

After launching the app, you will be presented with a listing of files and directories in `~/.password-store`. Files starting with '.' are hidden, and `.gpg` extensions are stripped.

![Main Screen](https://raw.github.com/rephorm/pass-ios/screenshots/screenshots/1_main_screen.png)

Clicking on a directory will show its contents.

![Subdirectory Listing](https://raw.github.com/rephorm/pass-ios/screenshots/screenshots/2_subdir.png)

Clicking on a password file will show a screen with the password file details (name and \*'d out password).

![Subdirectory Listing](https://raw.github.com/rephorm/pass-ios/screenshots/screenshots/3_entry.png)

Clicking on the name or password box will copy the respective contents to the pasteboard (clipboard). Since the password is encrypted, you will have to enter you passphrase before it can be copied.

![Subdirectory Listing](https://raw.github.com/rephorm/pass-ios/screenshots/screenshots/4_gpg.png)

Todo
----

* Full screen iOS7 view
* Require password /touchID to unlock (optional)

* Simplify initial setup

  - enter git repo url to clone
  - paste gpg key

* Better details screen
  - allow viewing multi-line content
  - auto-decrypt if passphrase stored?

* Allow storing gpg key passphrase in iOS Keychain
  - this is partially done. passphrase is stored until app exits for now

* Reset pasteboard contents after 45 s when copying password.

* Configurable Settings

  - base directory location (other than /var/mobile/.password-store)
  - whether to store passphrase in keychain
  - passphrase storage duration (X minutes or forever)
  - pasteboard reset time

* Password editing / adding
  - auto-commit ala pass bash script

* trigger 'git pull' from app (also 'git push' after editing is implemented)
* dropbox support?

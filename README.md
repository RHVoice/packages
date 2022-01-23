# RHVoice packages directory

## Purpose

This repository contains the information about the data packages, that
is the language and voice packages.

The information about each language or voice is stored in one or more
separate json files (see the src/ directory).

The build script creates a ssingle json file merging all the
information. This file itself is not tracked by git, but when one of
the source files is updated and the update is approved, the newly
generated single json will be uploaded to our server. This file will
be downloaded by the app for Android and hopefully other clients in
the future, so no applications need to be recompiled and updated on
the users' devices to give them access to the up-to-date versions of
the language and voice packages, unless those packages require
features only available in a later version of the RHVoice core.

## Branches and versions

The main branch is for testing the updates before making them
available to the stable versions of RHVoice. There will be multiple
stable branches where the updates will be cherry-picked once they are
considered ready for general use. There is one such branch right now,
named 1.8. If a new feature is introduced in the RHVoice core which
could make a new voice or language incompattible with older
builds of RHVoice, a new branch will be created, corresponding to that
version of RHVoice.

## For contributors

If you want to publish a new voice or aan update to the existing
language or voice, follow these steps.

1. Make sure that each of your commits changes only one language or
   one voice at a time, so individual changes are easy to track and
   revert if necessary.

2. Create a pull request to the main branch. When this request is
   reviewed, approved and merged, the updated single json file will be
   made available to the development versions of RHVoice. Right now it
   means the alpha version of the app for Android.

3. If a problem is discovered by the testers and a fix is
   implemented, create a new pull request with your changes.

4. After the update and possible fixes have been tested, the
   maintainer of this repository will merge the changes into the
   appropriate stable branches and upload the json to the server. If
   you feel this is taking too long, please open an issue.


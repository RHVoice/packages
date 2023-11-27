# RHVoice packages directory

## Purpose

This repository contains the information about the data packages, that
is the language and voice packages.

The information about each language or voice is stored in one or more
separate json files (see the src/ directory).

The build script creates a single json file merging all the
information. This file itself is not tracked by git, but when one of
the source files is updated and the update is approved, the newly
generated single json will be uploaded to our server. This file will
be downloaded by the app for Android and hopefully other clients in
the future, so no applications need to be recompiled and updated on
the users' devices to give them access to the up-to-date versions of
the language and voice packages, unless those packages require
features only available in a later version of the RHVoice core.

## Branches and versions

### Main

Any updates first appear in this branch. No users will have access to
them at this point. This is for testing by developers.

### Alpha

When an update has been tested by the developers, it will be
cherry-picked into the alpha branch. The json file built from this
branch is used by the Alpha version of the RHVoice app, so the updates
to this branch will be available to the testers using that version of
the app.

### Stable branches

When the update is considered ready for general use, it will be
cherry-picked into one or more of the stable branches. The only such
branch so far is 1.8. But future updates of the RHVoice core may make
it necessary to introduce new stable branches. Contributors will need
to document the usage of any core features introduced after RHVoice
v1.8.

The production version of the app will contain the latest core and use
the data from the most recent stable branch.

## For contributors

If you want to publish a new voice or an update to the existing
language or voice, follow these steps.

### Create the package.

Packages are simply zip archives of the data. Everything in your
data/languages/&lt;language&gt; or data/voices/&lt;voice&gt; directory goes into
the archive. If you compile under Linux or Windows, you will find a
ready-made archive in build/linux/packages/zip or build/windows/packages/zip.

### Upload your package

You need to provide or arrange the hosting for your packages.

The server hosting your packages must support https.

### Upload the voice demo clip if necessary

If you are contributing a new voice or have updated an existing voice
such that it has changed the way it sounds, synthesize the
language-specific demo text and upload the clip. The audio file must be in either Ogg Vorbis or MP3 format.

If you are contributing a language, please help us extend this table with a demo message for your language. Just translate the English message.

Language | Text
---|---
cs | Tento hlas není nainstalován. Posloucháte předem nahraný vzorek.
en | This voice is not installed. You are listening to a pre-recorded sample.
eo | Ĉi tiu voĉo ne estas instalita. Vi aŭskultas antaŭregistritan ekzemplon.
ka | ეს ხმა არ არის დაინსტალირებული. თქვენ უსმენთ წინასწარ ჩაწერილ მაგალითს.
ky | Бул үн орнотулган эмес. Сиз алдын ала жазылган үндү угуп жатасыз.
mk | Овој глас не е инсталиран. Слушате претходно снимен примерок.
pl | Ten głos nie jest zainstalowany. Słuchasz jego gotowej próbki.
pt | Esta voz não está instalada. Você está escutando uma amostra pré-gravada.
ru | Этот голос не установлен. Вы слушаете заранее записанный пример.
sk | Tento hlas nie je ešte nainštalovaný. Počúvate ukážkovú nahrávku slovenského hlasu.
sq | Ky zë nuk është i instaluar. Po dëgjoni një shembull të regjistruar paraprakisht.
sr| Ovaj glas nije instaliran. Vi slušate prethodno snimljen primer.
tt | Бу тавыш җиһазга урнаштырылмаган. Сез алдан яздырылган мисалны тыңлыйсыз.
uk | Цей голос не встановлено. Ви слухаєте попередньо записаний приклад.
uz | ушбу овоз ўрнатилмаган, сиз аввалдан тайёрланган намунани тинглаяпсиз.

### Create or update the json file describing your package.

See the existing files in the src directory for examples.

Some notes on the expected formats of values:

* Two-letter language codes are ISO 639-1 codes.
* Three-letter language codes are ISO 639-3 codes.
* Country codes are ISO 3166 codes.
* MD5 hashes are encoded as Base64. For example, you could use the
  following command to generate the hash, if you have openssl and
  base64 programs installed on your Linux system:
  ```
  openssl md5 -binary <YourPackageFile> | base64
  ```
  Also, you can use the md5_hash Python script from the packages directory.
  To use the md5_hash Python script, you need to specify a directory containing language and voice packages, and to specify a file name where MD5 hashes will be stored, (EG)
  ```
  md5_hash </home/packages> </home/md5_hashes.txt>
  ``````
  From Windows:
  ```
  python md5_hash <D:\packages> <D:\md5_hashes.txt>
  ``````

### Check your changes

#### Simple automated check

Let's imagine your two-letter language code is xy. And, if you have
created or updated a voice, you've saved the information about it in src/languages/xy/voices/myvoice.json.

Now let's run the check.

If you are publishing a language, run:
```./check xy```

If you are publishing a voice, run:
```./check xy myvoice```

The script will do some basic checks, including downloading your
package and verifying that the hash matches.

#### Runtime testing

Before your contribution is made available to the Alpha testers, you
must test it on an Android device and confirm that it works as
expected and causes no major issues, such as an RHVoice crash.

1. Build the package directory:
   ```./build```
2. Upload the packages-dev.json, created by the previous command, somewhere.
3. Setup the build environment to build the RHVoice app.
4. Create or edit ~/.gradle/gradle.properties, configuring the url to
   download the package directory:
   ```
   RHVoice.devPkgDirUrl=<url>
   ```
5. Build the development flavour of the app:
   ```./gradlew assembleDevDebug```
6. Install and test the app you have built, on an Android device with your voice.

### Make a pull request

Make a pull request to the main branch. When it's merged, the
maintainer will also upload the development package directory.
Make a final test of the development app build with the url pointing
to that directory. The build process is the same, except the
RHVoice.devPkgDirUrl property must not be set.

### Alpha testing

When you are sure your update is ready for testing by users, leave a
comment confirming it in the pull request.

When the update is available in the Alpha version of the app, there
will be a comment about it in the same pull request.

The testing is done as a closed test on Google Play, so you need to
find testers for your voice and provide us with their gmail addresses
in an email to rhvoice@rhvoice.org.

When we register your testers, they will have to use the special link we will
send to you to confirm to Google that they really want to become testers.

Gather feedback from the testers. If necessary and possible, fix the
issues and publish the fixes.

### Publishing to production

When your update is ready for use by the general public, open an
issue in this repository requesting the publication. Your package will be made available to the stable version of the app.


# Notices

The archives published by this repository contain one program, `waa-signer`,
built without source changes from third-party code. This repository holds only
the build workflow.

## waa-signer

Source: <https://github.com/DeconstructedCube/aistudio-api>, directory
`waa-signer`, at the commit named in `SOURCE.txt`.

That repository's README states that the project is released under the MIT
License and links to a `LICENSE` file; the file itself is not present in the
repository at that commit. The signer is derived from
<https://github.com/Mag1cFall/AIStudio2API>, which is MIT licensed
(Copyright (c) 2026 Mag1cFall). Its license text is in
`licenses/MIT-AIStudio2API.txt`.

## SpiderMonkey

The program statically links SpiderMonkey, the JavaScript engine of Mozilla
Firefox, licensed under the Mozilla Public License 2.0. The license text is in
`licenses/MPL-2.0.txt`. The source of the linked build is the tag named in
`SOURCE.txt` of <https://github.com/Mag1cFall/firefox>, a fork of
<https://github.com/mozilla-firefox/firefox>.

## Go modules

The program also links Go modules listed in `waa-signer/go.mod` of the source
repository, each under its own license.

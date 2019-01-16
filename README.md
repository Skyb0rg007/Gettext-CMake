# Gettext_helpers.cmake

## Assist in i18n for CMake projects

This repo provides Gettext_helpers.cmake, which implements the following function:

    configure_gettext(
        DOMAIN <domain-name>
        TARGET_NAME <target-name>
        SOURCES <file> ...
        POTFILE_DESTINATION <dir>
        [POFILE_DESTINATION <dir>]
        [GMOFILE_DESTINATION <dir>]
        LANGUAGES <file> ...
        [ALL]
        [INSTALL_DESTINATION <dest>]
        [INSTALL_COMPONENT <dest>]
        [XGETTEXT_ARGS <args> ...
        [MSGMERGE_ARGS <args> ...]
        [MSGINIT_ARGS <args> ...]
        [MSGFMT_ARGS <args> ... ]
        )

```configure_gettext``` creates a target <target-name> which creates and
maintains a .pot file, .po files, and .gmo files, as well as optional
install rules

# Installation

Copy Gettext_helpers.cmake into your project and ensure its directory
is on the CMAKE_MODULE_PATH

    list(APPEND CMAKE_MODULE_PATH <Gettext_helpers.cmake location>)

Then simply call ```include(Gettext_helpers.cmake)``` and ```configure_gettext(...)```

# Required arguments

* DOMAIN
    - The argument to ```bindtextdomain("@DOMAIN@", "@LOCALEDIR@")```
    - The resulting files will be @DOMAIN@.pot, @DOMAIN@.po, etc.
* TARGET_NAME
    - The name of the target which maintains the gettext files
    - Note that the target is a ```UTILITY``` target, so you cannot use
    ```target_link_libraries()```, you must use
    ```add_dependency(main-tgt gettext-tgt)```
* SOURCES
    - The source-files to search for gettext strings with ```xgettext```
    - Note that by default no arguments (outside "--output") are passed to
    ```xgettext```, so additional keywords such as ```_``` need to be specified
    with XGETTEXT_ARGS
* LANGUAGES
    - The languages to support. This will create directories
    "@POFILE_LOCATION@/@LANG@/" to hold .po files, and the directory structure
    for installation. See the gettext documentation on how specific you need
    to be with your language, ("en" vs "en_US" vs "en_US.UTF-8")
* ALL
    - Add the target to the default, ie. ```make all``` will compile it
* POTFILE_DESTINATION
    - Where to store the .pot file (template file)
    - Note that this file is regenerated on source file update, so
    do not edit it. Set package-name, etc. with XGETTEXT_ARGS
* POFILE_DESTINATION
    - Where to store the .po files (language-specific translation files)
    - A directory is created per .po file with the name of the given language
    - defaults to POTFILE_DESTINATION/po/
    - This file is never overwritten, only updated. To provide translations,
    update this file, but on source-file modification the file will be synced
    with the .pot file, so make certain you save before rebuilding
* GMOFILE_DESTINATION
    - Where to store the .gmo files
    - defaults to POFILE_DESTINATION
* INSTALL_DESTINATION
    - If set, the .gmo files will be installed to this directory in proper
    hierarchal structure
    - This should be a relative path, ex. share/locale/
* INSTALL_COMPONENT
    - Which component to install the .gmo files with
* <PROG>_ARGS
    - A list of arguments to pass to the given program
    - XGETTEXT_ARGS are most likely to be used to specify additional keywords and
    package details, while the others are not as useful

# Godot iOS Plugin template

This repo contains a *starter* Xcode and SCons configuration to build Godot plugin for iOS.
Xcode project and Scons configuration allows user to build static `.a` library, that could be used with `.gdip` file as Godot's plugin to include platform functionality into exported application.

# Initial setup

## Getting Godot engine headers

To build iOS plugin library it's required to have Godot's header files including generated ones. So running `scons platform=iphone target=<release|debug|release_debug>` in `godot` submodule folder is required.

To build iOS plugin library it's required to have Godot's header files
1. You must use the headers of the same godot engine version as your editor (e.g. version 4.3)
```
cd godot
git fetch
git checkout origin/4.3
```
2. run `scons platform=ios target=<template_debug|template_release|editor>` in `godot` submodule folder to generate header files (use `template_debug` for development and `template_release` for release)
**Note**:
- You don't have to wait for full engine compilation, as header files are generated first. Once the actual compilation starts, you can stop this command by pressing `Ctrl + C`
- The scons command may vary depend on the version of Godot engine. Older version may use `scons platform=iphone target=<release|debug|release_debug>` (Check SConstruct file to construct the right command)

## Building Godot Plugin

- run `scons platform=ios arch=<arm64|armv7|x86_64> target=<release|debug|release_debug> target_name=<library_name> version=<3.2|4.0>` to build GodotShare (the generated `.a` file is found in /bin)
- The above command only generate library that works in IOS device but not in simulator. To test in simulator, run `scons platform=ios arch=<arm64|armv7|x86_64> target=<release|debug|release_debug> target_name=<library_name> version=<3.2|4.0> simulator=yes`

## Using Godot Plugin
- Create the directory [Your_Godot_project]/ios/plugins/[Library_name] in your Godot project and copy both the generated `.a` file and `plugin_example.gdip` file there
- Update `plugin_example.gdip` to point to correct `.a` file (The plugin should show up in the export template)
- Compiling for multiple archs and using `lipo -create .. -output ..` might be required for release builds.

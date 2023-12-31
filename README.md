# Feather

![Build](https://img.shields.io/github/actions/workflow/status/OrnitheMC/feather-mappings/build.yml?label=Build&branch=main)
![Build All](https://img.shields.io/github/actions/workflow/status/OrnitheMC/feather-mappings/build_all.yml?label=Build%20All&branch=main)
![Publish](https://img.shields.io/github/actions/workflow/status/OrnitheMC/feather-mappings/publish.yml?label=Publish&branch=main)
![Discord](https://img.shields.io/discord/922262455453888542?color=5865F2&label=Discord&logo=Discord&logoColor=ffffff)

Feather is a set of open, unencumbered Minecraft mappings, free for everyone to use under the Creative Commons Zero license. The intention is to let 
everyone mod Minecraft freely and openly, while also being able to innovate and process the mappings as they see fit.

## Usage
To use feather-deobfuscated Minecraft for Minecraft modding or as a dependency in a Java project, you can use [loom](https://github.com/OrnitheMC/ornithe-loom) Gradle plugin. See [fabric wiki tutorial](https://fabricmc.net/wiki/tutorial:setup) for more information.

To obtain a deobfuscated Minecraft jar, [`py feather.py mapNamedJar <minecraft version>`](#mapNamedJar) will generate a jar named like `<minecraft version>-named.jar`, which can be sent to a decompiler for deobfuscated code.
You can also directly generate a mapped jar and decompile the code using one of the following commands (no need to run the `mapNamedJar` task first):
- CFR: `py feather.py decompileCFR <minecraft version>`
- Quiltflower: `py feather.py decompileQuiltflower <minecraft version>`
- Procyon: `py feather.py decompileProcyon <minecraft version>`

## Contributing

Our goal is to provide high-quality names that make intuitive sense. In our experience, this goal is best achieved through contribution of original names, rather than copying names from other mappings projects. While we believe that discussions relating to the names used by other mappings projects can be useful, those names must stand up to scrutiny on their own - we won't accept names on the grounds that they're present in other mappings projects.

We recommend discussing your contribution with other members of the community - either directly in your pull request, or in our other community spaces. We're always happy to help if you need us

Please have a look at the [naming conventions](/CONVENTIONS.md) before submitting mappings.

### Getting Started

1. Fork and clone the repo
2. Run `py feather.py feather <minecraft version>` to open [Enigma](https://github.com/OrnitheMC/Enigma), a user interface to easily edit the mappings
3. Save your changes by running one of the following save tasks (`py feather.py <task> <minecraft version>`):
   - `propagateMappings`: propagate your changes up and down the version graph and save them to every applicable Minecraft version (this is most likely the task you want to use)
   - `insertMappings`: save your changes only to the specified Minecraft version
   - `propagateMappingsDown`: propagate your changes down the version graph (to versions further away from the root) and save them to every applicable Minecraft version
   - `propagateMappingsUp`: propagate your changes up the version graph (to versions closer to the root) and save them to every applicable Minecraft version
4. Commit and push your work to your fork
5. Open a pull request with your changes

#### NOTE

The `feather` task separates the mappings for the specified version out into temporary files in the `/run/` folder. Enigma will read and write to these files, and the save tasks will use these files to save the mappings back into the version graph.

- DO NOT MANUALLY EDIT THESE FILES! You may corrupt the mappings!
- Running the `feather` task **will** overwrite these files. If you have unsaved changes, make sure to run one of the save tasks **before** running the `feather` task to open Enigma again!
- You can safely open multiple Enigma instances for *different* Minecraft versions. You **cannot** safely open multiple Enigma instances for *one* Minecraft version.

## Gradle
Feather uses Gradle to provide a number of utility tasks for working with the mappings.

### `feather`
[`setupFeather`](#setupFeather) and download and launch the latest version of [Enigma](https://github.com/FabricMC/Enigma) automatically configured to use the merged jar and the mappings.

Compared to launching Enigma externally, the gradle task adds a name guesser plugin that automatically map enums and a few constant field names.

### `build`
Build a GZip'd archive containing a tiny mapping between official (obfuscated), [intermediary](https://github.com/FabricMC/intermediary), and feather names ("named") and packages enigma mappings into a zip archive..

### `mapNamedJar`
Builds a deobfuscated jar with feather mappings and automapped fields (enums, etc.). Unmapped names will be filled with [intermediary](https://github.com/FabricMC/Intermediary) names.

### `decompileCFR`
Decompile the mapped source code. **Note:** This is not designed to be recompiled.

### `download`
Downloads the client and server Minecraft jars for the current Minecraft version to `/feather-build-cache/game-jars/` in your user gradle cache.

### `mergeJars`
Merges the client and server jars into one merged jar, located at `/feather-build-cache/merged/<minecraft_version>-merged.jar` in your user gradle cache.

### `setupFeather`
[`download`](#download) and [`mergeJars`](#mergeJars)

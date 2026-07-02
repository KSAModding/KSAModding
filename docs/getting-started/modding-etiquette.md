# Modding Etiquette

This page describes common etiquette when creating mods and good mod creation hygiene.
NOTE: Standards will change as time goes on to better serve the modding community. Please make sure to keep up with current standards.

## Format & File Structure

All mods should follow similar standards with the format in creation to help improve compatibility.

  - Concerning `Core\`
    - KSA itself is structured as a mod for the BRUTAL framework. As such the game itself is stored the exact same way as a mod would be.
    - When modding it is bad practice to directly modify Core\
      - Directly modifying the game files not only hurts compatibility with other mods, it also means that you may have to reinstall KSA to get original functionality back.
      - Directly modifying core will not allow you to use tools such as StarMap and KSA Mod Loader.
      - Instead, create your own mod in `Content\` with its own mod.toml and files.
  - Naming your Mod
    - When naming your mod, please follow standard program folder naming etiquette.
      - Do not name your mod `Core\`! Just don't do it!
      - Make the name for your main mod folder the same as what is in your mod.toml. If the name in your mod.toml is `ExampleMod` your folder should be called `ExampleMod`.
      - Avoid using illegal characters. (e.g. `ExampleMod`, `Example-Mod`, and `Example_Mod` are good. However, `Example.Mod` or `Example\Mod` are not good.)
  - Using `Core\` Assets
    - You may end up making a mod that uses default KSA textures.
    - In this case, do not copy and paste the `Textures\` folder into your mod, instead link to the texture you want in your mod file.
      - For example, to link to the Earth Normalmap, put the path to it as `..\Core\Textures\Earth_Normal.dds`
  - Mod Assets
    - You will end up having assets in your mods, these are things ranging from an `Astronomicals.xml`, to a custom system, to textures. Please follow some standard conventions for including these.
      - All textures should be in a `Textures\` sub-folder. Please do not just paste them into your main folder I don't want to have to read through a giant conglomeration of png files to try and find the custom system in the mod.
      - All assets should be named in an easy to understand format.
        - If its a custom `Astronomicals.xml` call it something like `ModnameAtronomicals.xml` or even just `Astronomicals.xml`
        - If its a custom system, maybe include "System" in the file name. (e.g. `Polar-Earth-Only-System.xml` or `StockSystem.xml`)
  - Documentation
    - Document everything that your mod does. This can be done in a `ReadMe` for your mod or comments in code. We also reccomend mentioning all of this in the forum page for your mod.

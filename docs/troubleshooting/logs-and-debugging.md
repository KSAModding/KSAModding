# Logs and Debugging

## Finding Log Files

KSA stores its log files in your Documents folder:

**Location:** `C:\Users\[YourName]\Documents\My Games\Kitten Space Agency\Logs\`

The main log file is `KittenSpaceAgency.log`, which gets updated as you play. Older logs are automatically archived in the `Archives` subfolder with timestamps.

## Reading Log Files

Open `KittenSpaceAgency.log` in any text editor (Notepad, VS Code, etc.) to view the log.

The log file contains useful information for debugging:

- **Loading errors**: Look for lines containing "ERROR" or "Exception"
- **Asset loading**: Search for your mesh/texture filenames to see if they loaded successfully
- **XML parsing errors**: Check for "XML" or "Parse" related errors
- **Instantiation issues**: Look for "instantiated" or "Vehicle" mentions


## Common Error Patterns

### NullReferenceException in SubPartModel
**Error:** `NullReferenceException at SubPartModel..ctor`

**Cause:** Missing texture files (Normal or RoughMetalAo)

**Solution:** Ensure all three texture types (Diffuse, Normal, RoughMetalAo) are specified in your PbrMaterial definition

### Model Not Loading
**Search log for:** Your mesh filename (e.g., "your_model.glb")

**If not found:** Check the file path in PartAssets.xml matches the actual filename

**If found with errors:** Check the GLB file format and vertex attributes

### XML Parse Errors
**Error:** Lines mentioning XML parsing or schema validation

**Cause:** Syntax error in XML files (missing closing tags, invalid attributes)

**Solution:** Validate your XML syntax - common issues:
- Missing closing tags `</Part>`, `</SubPart>`, etc.
- Misspelled attribute names
- Unclosed quotes in attribute values

## Debugging

Debugging your mod can be done by following these steps:

### For Visual Studio:
1. Open Debug Properties:
![Creating an executable configuration](/KSAModding/assets/images/troubleshooting/logs-and-debugging/vs-edit-debug-properties.png)

2. Create a new Executable Launch Profile and make it point to the StarMap.exe executable file (don't forget to set the working directory to the StarMap folder)
![Creating an executable configuration](/KSAModding/assets/images/troubleshooting/logs-and-debugging/vs-create-executable-task.png)

And that's it! You can now set breakpoints in your mod code and run StarMap from Visual Studio to debug your mod.

### For JetBrains Rider:
1. Open the Run/Debug Configurations dialog:
![Creating an executable configuration](/KSAModding/assets/images/troubleshooting/logs-and-debugging/rider-edit-configurations.png)

2. Create a new Executable Launch Configuration and make it point to the StarMap.exe executable file (don't forget to set the working directory to the StarMap folder)
![Creating an executable configuration](/KSAModding/assets/images/troubleshooting/logs-and-debugging/rider-run-executable.png)

You can now set breakpoints in your mod code and run StarMap from Rider to start debugging

#### Pro tip

In Rider you can setup a task that will output your binaries in the Content folder of your KSA installation automatically
and then you can make this task run before launching the game, so whenever you hit debug, your latest code changes will be deployed to the mods folder.

![Creating an executable configuration](/KSAModding/assets/images/troubleshooting/logs-and-debugging/rider-publish-to-folder-task.png)

# Logs and Debugging

## Finding Log Files

KSA stores its log files in your Documents folder:

**Location:** `C:\Users\[YourName]\Documents\My Games\Kitten Space Agency\Logs\`

The main log file is `Brutal.log`, which gets updated as you play. Older logs are automatically archived in the `Archives` subfolder with timestamps.

## Reading Log Files

Open `Brutal.log` in any text editor (Notepad, VS Code, etc.) to view the log.

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

# Star Binaries

## Overview
This guide explains how to add custom star binaries to the game. 

## Requirements
- Text editor for TOML editing
- Star binary
- Optional program(like python) for creating custom star binaries

## File Locations
- **Game Directory**: `C:\Users\[YourName]\AppData\Local\Programs\Kitten Space Agency\` or `C:\Program Files\Kitten Space Agency\`
- **Default star binary**: `Content\Core\hip_main.bin`

## Step 1: Creating your star binary

Star binaries are like the name says binary files(` .bin `)

### Structure
The first 4 bytes in the file make an `int32` which tells KSA the count of stars contained in your file.

All the following data represents the stars themselves

All numbers in star binaries use little-endian byte order

#### Each star takes up 16 bytes
 - First 12 bytes tell KSA the unit vector position( 3 * `float32`) of the star using KSA's default [coordinate conventions](/KSAModding/fundamentals/coordinates-and-units/)
 - The 13th byte tells KSA the size of the star (0 - 255)
 - The last 3 bytes tell KSA the color of the star (0 - 255, RGB)

This is an example of an binary that creates one large blue star on the positive X axis
```bin
           00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F

00000000   01 00 00 00 00 00 80 3F 00 00 00 00 00 00 00 00  ......?........
00000010   FF 00 00 FF                                      ....            

```
![star-binary-structure](/assets/images/star-binary-structure.png)

## Step 2: Register the star binaries file
Edit the `mod.toml` file to include 
```toml
starBinaries = ["YOUR_FILE"]
```
ATTENTION: every mod including core will stack their star binaries ontop of each other(like shown in the example)


## Example
![star-binary-example](/assets/images/star-binaries-example.png)

## Limitations

 - You cannot assign custom star binaries for every [system](/KSAModding/modding-guides/system/)
 - Mods can't override each other's star binaries

## Common mistakes
 - The position must be a normalized unit vector
 - Wrong byte order (Must be little endian)
 - Size out of range (0 - 255)
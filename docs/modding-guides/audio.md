# Creating And Playing Audio

## Overview
Here you will learn how to set up sound files that are in YourModAssets.xml through your own mod, MySoundMod.dll in the case of this guide.

## Requirements
* This guide assumes you already know how to create [your own mod.](/KSAModding/modding-guides/code/first-steps/)
* Any text editor to edit the .xml files.

### Setting Up YourModAssets.xml

Create a new folder in the Kitten Space Agency Content folder, name this folder "MySoundMod". Then in any text editor enter in,
```
<?xml version="1.0" ?>
```

and save the file as "YourModAssets.xml" inside of the MySoundMod folder you just made. This will create a completely blank new xml file that will house all of your mod assets, including audio as this example will show. Next, we will structure it so that "Assets" becomes the main XML root element of the file.
```
<?xml version="1.0" ?>
<Assets> <!--This is an element, and because it will house other elements it is called the root element.-->
<!--I am a comment!-->
</Assets>
```

Now that we have the basics built let's incorporate a `<Sound>` element, along with some its attributes.
```
<?xml version="1.0" ?>
<Assets> 
<Sound Id="TestSound" SimSpeed="Ignore" Channel="Music"> <!--Element Attributes go between the end of the element name and the ending '/>' -->
<SoundFile Path="..\Core\Music\00_Drifting.ogg" TwoD="true" StreamFromDisk="true" Volume="0.2">
<Loop>
<Start Value="0" Unit="Ms"/>
</Loop>
</SoundFile>
</Sound>
</Assets>
```
For this example we will only be using the elements and attributes as shown, a full list of attributes can be found in the following locations in KSA.dll;

* For `<Sound/>` check the SoundReference class 
* For `<SoundFile/>` check the SoundFileReference class
* For `<Loop/>` check under the LoopData class 

Note: The Loop Start element is not required and the loop will go back to the beginning of the audio file by default, having no Loop element will result in the sound ending after fully playing through the file, unless it is stopped earlier.

###Referencing the Audio in Visual Studio
Follow the steps [here](/KSAModding/modding-guides/code/first-steps/) to create your Visual Studio project.
Change it to look similar to this:
```
using StarMap.API;
using KSA;

namespace MySoundMod
{
    [StarMapMod]
    public class MySoundMod
    {
	private bool UiThisFrame = false;

        [StarMapBeforeGui]
        public void OnBeforeUi(double dt)
        {
            UiThisFrame = false;
	//we will play our audio in here so it will update every frame, you can run it wherever you want but make sure it's after the mod is loaded
        }
    }
}
```
Also edit the mod.toml to include a line to load in your xml file;
```
name = "MySoundMod"
assets = ["YourModAssets.xml"]
```
To play the audio we will be using SoundReference.Play(SpatialAudio, Volume, Channel, StartPaused)
Fortunately, it can accept SpatialAudio and Channel variables that have nothing assigned to them, so we can create those variables quickly. For the SoundReference variable we will have to find where the mod's assets are stored and then reference the sound we want with its Id="" attribute.
Luckily KSA handles all of this in KSA.ModLibrary so we simply do;
```
using StarMap.API;
using KSA;

namespace MySoundMod
{
    [StarMapMod]
    public class MySoundMod
    {
	private bool UiThisFrame = false;
	public IChannel channel;
	public SpatialAudio spatial; //When we dont assign anything to spatial it will use the default settings, which is 2d audio that will play the same regardless of the camera

        [StarMapBeforeGui]
        public void OnBeforeUi(double dt)
        {
            UiThisFrame = false;

	    SoundReference MySound = ModLibrary.Get<SoundReference>("TestSound"); //The "TestSound" here is the <Sound Id="TestSound"/> that we made earlier
	    MySound.Play(spatial, 1, out IChannel? channel, false); //there is no stop command specifically so to stop an audio related to a Sound Id we can run it again but replace the false with true

        }
    }
}

```
Now build it and move the mod over to your mod folder, which should look something like this. ![mod folder](/assets/images/modding-guides/code/FilePathReference.png)

Note: Make sure to edit the Manifest to make sure your mod is being loaded!

Now run it through StarMap and see what happens (if you hear anything it would be a slight stuttering). The audio doesn't play through because it's being called every frame, a simple check to see if it's playing can fix this issue.

```
using StarMap.API;
using KSA;

namespace MySoundMod
{
    [StarMapMod]
    public class MySoundMod
    {
	private bool UiThisFrame = false;
	public IChannel channel;
	public SpatialAudio spatial; //When we dont assign anything to spatial it will use the default settings, which is 2d audio that will play the same regardless of the camera
	public bool Playing = false;

        [StarMapBeforeGui]
        public void OnBeforeUi(double dt)
        {
            UiThisFrame = false;

	    SoundReference MySound = ModLibrary.Get<SoundReference>("TestSound"); //The "TestSound" here is the <Sound Id="TestSound"/> that we made earlier
	if(Playing == false)
	{
	    Playing = true;
	    MySound.Play(spatial, 1, out IChannel? channel, false); //there is no stop command specifically so to stop an audio related to a Sound Id we can run it again but replace the false with true
	}

        }
    }
}

```
This new code should now have the audio playing on loop, and it will keep looping until you reference the same Sound Id again and Play it with the startPaused = true, which is what the true/false bool at the end of the SoundReference.Play() represents.

You may also notice that the game audio plays alongside your audio, this could be an issue for some purposes so to stop it use;
`LocationMusicPlayer.Stop();`

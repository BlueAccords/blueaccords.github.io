---
layout: post
title: A Long Time Coming
date: 2015-10-28
tags:
- skyrim
- papyrus
- study
published: true
---


Welp haven't updated in a while.

## Skyrim Papyrus
Jumping into the deep end I'm going to just try and decode what these source files from
[Apocalypse - Magic of Skyrim](http://www.nexusmods.com/skyrim/mods/16225)
Is actually doing.
More specifically, the Hethoth's Grimoire spell that spawns a spellbook that uses the spell in your left hand and fires it repeatedly at where your crosshairs are aiming.

## Goals for this study
1. Find out how to spawn an object as a magic effect
	- determine where the object spawns in (X, Y, Z) Coordinates
    - to delete the object after the magic effect duration ends
    - make the object intangible/tangible if it was explicitly set.
    - set the object's duration.
2. Find out how to get the magic effect/spell in the player's current left hand.
  - make the object fire the spell.
3. Find out how to make the object spawn where the cursor is pointing if it doesn't already do that.
4. Find out how to make the object spawn anywhere
5. Find out how the spell uses the player's reticule to determine where to aim the spell
	- Make the spell autoaim on hostile targets/everyone/only the player/

## Setup  

- First of all is using [xanderdunn's guide](https://github.com/xanderdunn/skaar/wiki/Papyrus:-Getting-Started) to set up Papyrus and using [STEP's BSAopt guide](http://wiki.step-project.com/Guide:BSA_Extraction_and_Optimization#tab=Setup_and_Use_of_BSAopt) to set up BSAopt to actually extract the source file scripts from the BSA files from Apocalypse Magic.  

- You might have to also follow these instructions:

> It appears the update removed files from the data/scripts/source folder. The missing files are inside a Scripts.rar file in the data folder. So extracting that and copying the skse sources again fixed it.

So in your  
```C:\Program Files (x86)\Steam\SteamApps\common\Skyrim\Data```  
or  
```C:\Program Files\Steam\SteamApps\common\Skyrim\Data```

Directories there should be a ```Scripts.rar``` Archive you should extract the Scripts folder to your data folder(The same folder the ```Scripts.rar``` file is in).

- And Finally you'll have to install the creation the kit if you haven't already using this [guide](http://wiki.step-project.com/Guide:Mod_Organizer#Creation_Kit_.28CK.29) if you're using mod organizer.

## Scripting Guide
Welp. Luckily the day I started trying to learn scripting someone uploads an amazing resource for it [here](http://www.nexusmods.com/skyrim/mods/70883). Thanks Mattiewagg.

## Setting up Files
You will have to extract the source files from Apocalypse Spell Magic using BSAopt and put the files in the source folder under ```Data/Scripts/Source``` And you still might get errors in the creation kit when trying to view script properties.  

Under the Magic Effect "WB_A100_HethothsGrimoire_Effect" in the creation kit should be ```wb_hethothsgrimoire_script.psc```
And if you get an "Error unable to reload scripts" Or something while trying to view the script properties you can try adding a property to the greyed out property window and hoping it fixes itself.

## Hethoth's Grimoire Source File
The source file is the .psc file. the .pex file is the compiled version so if a mod doesn't have a bsa with the script's .psc source file or the source files as just .psc files downloadable then I don't know what you'd be able to do besides decompiling the .pex files.


{% highlight ruby linenos %}
Scriptname WB_HethothsGrimoire_Script extends activemagiceffect  

; -----
; Property Declarations
; ALL OF THESE PROPERTIES ARE REFERENCED IN THE CK UNDER PROPERTIES
; THAT IS WHERE YOU CAN FIND WHAT OBJECTS/VALUES THEY REFERENCE.
; I'm assuming they are set OUTSIDE the script by "filling the property"
; WB..HethothsGrimoire and FX..Activator are references to a WorldObjects > Activators inside the creation kit
; These are referenced properties in the CK of the script.
Activator Property WB_AlterationAlt_Activator_HethothsGrimoire Auto
Activator Property FXEmptyActivator Auto
; These four values are properties of this script in the creation kit
; I have no idea why they're set there instead of here but they have actual values there.
; Those values being:
; 1.25, 1000.00, 64.00, 96.00
Float Property WB_UpdateRate Auto
Float Property WB_DistanceInFront Auto
Float Property WB_DistanceInFrontSpawn Auto
Float Property WB_Height Auto
; Message object from Miscellaneous > Message in the CK
Message Property WB_AlterationAlt_Message_HethothsGrimoire_Fail Auto
; Explosion Object
Explosion Property WB_AlterationAlt_Explosion_CreateLodeOrb Auto
; Reference to 2 other spells in the same mod.
; These 2 are ALSO referenced properties in the CK
Spell Property WB_D100_ForbiddenSun_Spell_PC Auto
Spell Property WB_D100_StaticDome_Spell_PC Auto

; -----
; Variable Declarations
; Unintialized I'm assuming so theyre all "default" values of 0/none/false ect.
Actor NearestActor
ObjectReference FinisherBox
Int CurrentCharges
ObjectReference CreatedBook
Actor CasterActor
Actor TargetActor
Spell TriggeredSpell

; -----
; Event
; http://www.creationkit.com/OnEffectStart_-_ActiveMagicEffect
; Event called when the active magic effect has just started on the specified target.
; akTarget: The Actor this effect was applied to.
; akCaster: The Actor that cast the spell this effect was from.
Event OnEffectStart(Actor akTarget, Actor akCaster)
; Triggered spell is the caster's left hand spell.
	TriggeredSpell = akCaster.GetEquippedSpell(0)
	; check if the triggered spell is a spell AND checks if the spell is NOT forbidden sun/static dome(from apoc)
	; else returns a message saying the spell failed.
	If TriggeredSpell && (TriggeredSpell != WB_D100_ForbiddenSun_Spell_PC && TriggeredSpell != WB_D100_StaticDome_Spell_PC)
		; CasterActor = akCaster provided from the event
		CasterActor = akCaster
		; TargetActor = akTarget provided from the event
		TargetActor = akTarget
		; PlaceAtMe is an objective reference function
		; http://www.creationkit.com/PlaceAtMe_-_ObjectReference
		; only the activator is given as a form while the other 3 values will be their default values.
		; aiCount = 1(1 instance will be made), abForcePersist = False(So the object will NOT be persistent)
		; abIntiallyDisabled = false(So the object will NOT be initially disabled.)
		CreatedBook = akTarget.PlaceAtMe(WB_AlterationAlt_Activator_HethothsGrimoire)
		; MoveTo is an object reference function
		; http://www.creationkit.com/MoveTo_-_ObjectReference
		; akTarget is the target reference to move the CreatedBook to.
		; afXOffset = (WB_DistanceInFrontSpawn*Math.Sin(akTarget.GetAngleZ())) is the X direction offset
		; afYOffset = (WB_DistanceInFrontSpawn*Math.Cos(akTarget.GetAngleZ())) is the Y direction offset
		; afZOffset = WB_Height 												is the Z direction offset
		; abMatchRotation is undefined so it will default to true and WILL match the object rotation of the target object.
		CreatedBook.MoveTo(akTarget,(WB_DistanceInFrontSpawn*Math.Sin(akTarget.GetAngleZ())),(WB_DistanceInFrontSpawn*Math.Cos(akTarget.GetAngleZ())),WB_Height)
		; SetAngle sets the object's rotation in the world
		; http://www.creationkit.com/SetAngle_-_ObjectReference
		; afXAngle = 0 						X Axis Rotation(degrees)
		; afYAngle = 0 						Y Axis Rotation
		; afZAngle = akTarget.GetAngleZ()	Z Axis Rotation
		; GetAngleZ() is an ObjectReference Function which returns the object's Z axis rotation in degrees.
		CreatedBook.SetAngle(0,0,akTarget.GetAngleZ())
		; FinisherBox is set as the FXEmptyActivator (which should be in the CK)
		FinisherBox = CreatedBook.PlaceAtMe(FXEmptyActivator)
		; Will call this function every x real time seconds(assuming its WB_UpdateRate that is the seconds)
		; Until UnregisterForUpdate is called.
		; Continually calls the OnUpdate() Event with an update rate of x seconds
		; where X = WB_UpdateRate
		RegisterForUpdate(WB_UpdateRate)
	Else
		; If the spell is invalid show this message
		; This message is defined as a "Message" in the CK
		WB_AlterationAlt_Message_HethothsGrimoire_Fail.Show()
		; Removes the magic effect that is affecting the actor
		; http://www.creationkit.com/Dispel
		Dispel()
	EndIf

EndEvent

; -----

; Event called when the effect is finished.
; http://www.creationkit.com/OnEffectFinish_-_ActiveMagicEffect
Event OnEffectFinish(Actor akTarget, Actor akCaster)
	; Place an explosion at where the book is.
	CreatedBook.PlaceAtMe(WB_AlterationAlt_Explosion_CreateLodeOrb)
	; Sets the x/y/z position of the book (i think this just moves this underground to make it "disappear")
	; http://www.creationkit.com/SetPosition_-_ObjectReference
	CreatedBook.SetPosition(0,0,20000)
	; Utility.wait is a global function
	; Utility.wait will pause for 2.0 seconds
	; This a "Normal" wait which will pause the entire script for 2 realtime seconds not counting menu time
	; There is a wait for in game hours(Utility.WaitGameTime) and menu mode realtime seconds(Utility.WaitMenuMode).
	; There is another time of waiting that runs simultaneously with the script and calls another event after x time.
	; This is called RegisterForUpdates
	Utility.Wait(2.0)
	; Deletes the objectReferences
	; http://www.creationkit.com/Delete_-_ObjectReference
	; I guess this makes it so you don't need to call UnRegisterForUpdate
	CreatedBook.Delete()
	FinisherBox.Delete()

EndEvent

; -----

; This event is going to be called continuously after RegisterForUpdate() is called
; http://www.creationkit.com/RegisterForUpdate_-_Form
Event OnUpdate()
	; Checks if player is NOT in the menu mode before executing the update.
	; http://www.creationkit.com/IsInMenuMode_-_Utility
	If !Utility.IsInMenuMode()
		; Moves the finisher box to where the CreatedBook is.
		; I currently have no idea what purpose this box serves.
		FinisherBox.MoveTo(CreatedBook,WB_DistanceInFront*Math.Sin(TargetActor.GetAngleZ()),WB_DistanceInFront*Math.Cos(TargetActor.GetAngleZ()),(WB_DistanceInFront*Math.Tan(-TargetActor.GetAngleX()) + 0))
		; Trigger spell is a script of Spell Script
		; http://www.creationkit.com/Spell_Script
		; http://www.creationkit.com/RemoteCast_-_Spell
		; Thus RemoteCast is also a function of the spell script
		; akSource = CreatedBook		Is the source of where the spell will be casted.(doesn't have to know the spell.)
		; akBlameActor = CasterActor	Is the actor that's blamed for the crime if this spell causes one.
		; akTarget = FinisherBox(optional) Is ObjectReference to where to aim the spell.
		TriggeredSpell.RemoteCast(CreatedBook,CasterActor,FinisherBox)
	EndIf

EndEvent

; -----
{% endhighlight %}

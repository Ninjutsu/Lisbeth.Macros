# Lisbeth.Macros
Here you'll find a collection of Lisbeth macros that several community members have created. Although Lisbeth has a general crafting AI integrated, these macros might prove useful for specific circumstances.

Macros are a text file with a list of skills and conditionals that Lisbeth executes, and they can be used for both crafting and collectable gathering rotations.

Macros can be used in two ways: 

1. You can assign them directly to an order through that order's individual settings. This forces Lisbeth to use that macro when performing that order, regardless of priority or usage conditionals.
2. You can use them as **overrides**. When Lisbeth crafts something or gathers collectables, it will first check every enabled macro that you have to see if any of their usage conditionals are valid for the craft that is about to happen. If it finds one, it'll use that instead of the default crafting AI. In other words, it *overrides* the default behavior when the conditionals in the macro are met. This happens with primary and secondary orders (suborders). Only enabled macros are used in this manner.

## Macro Syntax

Macros are text files that reside in the **/Rebornbuddy/Settings/Lisbeth/Macros** folder. The files can be named anything you want, as long as their name is unique within the folder. Below you can see an example of a macro:

```
Name: Level 70 3Star 35 Durability - Specialist
Type: Craft
Enabled: False
Conditions: (MaxDurability == 35 && MaxProgress == 2365 && Craftsmanship >= 1500 && Control >= 1350 && Cp >= 524 && RecipeLevel == 350 && Specialized == True)

S1:
- Initial Preparations
- Comfort Zone
- Inner Quiet
- Specialty: Reflect
- Steady Hand II
- Manipulation II
- Piece by Piece
- Piece by Piece
- Prudent Touch
- Prudent Touch
- Observe
- Focused Synthesis
- Comfort Zone
- Ingenuity
- Steady Hand II
- Innovation
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Steady Hand II
- Prudent Touch
- Ingenuity II
- Innovation

S2: (IsExcellent == true)
- Byregot's Blessing
- Observe
- Focused Synthesis

S3:
- Great Strides
- Byregot's Blessing
- Observe
- Focused Synthesis
```

Here is what the macro's properties do:

* **Name:** This is the name that will show on Lisbeth's UI. Try keep it the same as the file name for consistency; although this is not a requirement. 
* **Type:** Tells Lisbeth if the macro is for crafting or gathering.
* **Enabled:** This enables the macro as an override. You don't need to enable the macro to use it directly on an order's individual settings.
* **Conditions:** These conditions are checked when searching for a macro to use it as an override. Conditions can only have logical AND (&&) statements and must be inside parentheses. 

## Labels

As you can see from the example above, you can write multiple **segments** with skills, and give each one a label (i.e. S1, S2). Right beside the label, on the same line, you can add a conditional that uses the same syntax as the macro's general conditional property:

```
S2: (IsExcellent == true)
```
Lisbeth will execute the macro one **label** after the other, as in S1 => S2 => S3. On each label, it'll choose to use the first segment whose conditional is met. It only uses one segment per label (or none if no segments for that label had their conditional met). This means the relationship between segments with the same label is a logical OR.

*For S2, try use this segment first. If the condition is met, execute the skills and proceed to label S3. If the condition is not met, then try this other segment instead.*

## Jumps

Instead of using conditionals on the labels, you can also do jumps at the end of a label. For example:

```
S1:
- Initial Preparations
- Comfort Zone
- Inner Quiet
- Specialty: Reflect
- Steady Hand II
- Manipulation II
- Piece by Piece
- Piece by Piece
- Prudent Touch
- Prudent Touch
- Observe
- Focused Synthesis
- Comfort Zone
- Ingenuity
- Steady Hand II
- Innovation
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Steady Hand II
- Prudent Touch
- Ingenuity II
- Innovation
=> S2 (IsExcellent == true)
=> S3

S2:
- Byregot's Blessing
- Observe
- Focused Synthesis

S3:
- Great Strides
- Byregot's Blessing
- Observe
- Focused Synthesis
```

## Jumps

You can also put conditionals in a line to pick the skill for that step. For example:

```
S1:
- Muscle Memory
- Comfort Zone
- Inner Quiet
- Steady Hand II
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Manipulation II
- Steady Hand II
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Prudent Touch
- Careful Synthesis II
- Steady Hand
- Great Strides
- (IsGoodOrExcellent == true) Tricks of the Trade | Ingenuity II
- Byregot's Blessing
- Careful Synthesis III
- Careful Synthesis III
- (Cp >= 7) Careful Synthesis III | Careful Synthesis II
- (Cp >= 7) Careful Synthesis III | Careful Synthesis II
```

**NOTE:** Whatever skill the macro selects on each step must be castable at that moment or Lisbeth will stop. In other words, Lisbeth assumes that all the skills the macro gives it as it executes are skills you want to cast, and if one of them doesn't get cast it considers it an error.

## Crafting Variables

These are all the variables you can use inside crafting macro conditionals. If there's a variable that's missing and you think it's useful let me know and I can add it.

```
int Recipe 
int RecipeLevel 
int RecipeDisplayLevel 
int RealLevelDifference 
int DisplayedLevelDifference 

int PlayerLevel 
int PlayerBaseLevel 
bool Specialized 
int Craftsmanship 
int Control 

int Cp 
int MaxCp 
int Progress 
int MaxProgress 
int Quality 
int MaxQuality 
int Durability 
int MaxDurability 

int Step 
int HqPercent 

bool IsGood 
bool IsExcellent 
bool IsPoor 
bool IsNormal 
bool IsGoodOrExcellent 

bool IsAspectFire 
bool IsAspectWater 
bool IsAspectLightning 
bool IsAspectWind 
bool IsAspectEarth 
bool IsAspectIce 

int SteadyHandAura
int SteadyHand2Aura
int InnerQuietAura
int ComfortZoneAura
int MakersMarkAura
int WasteNotAura
int WasteNot2Aura
int GreatStridesAura
int InnovationAura
int ManipulationAura
int Manipulation2Aura
int IngenuityAura
int Ingenuity2Aura
int InitialPreparationsAura

int RemainingCp (cp + CZ stacks * 8)
```

## Gathering Macros

Gathering macros are used for collectable rotations. They have the same syntax as crafting macros, the only difference is the variables and skills you can use. Here's an example of one:

```
Name: Yellow Scripts 600GP
Type: Gather
Enabled: True
Conditions: (MaxGp >= 600)

S1:
- Discerning Eye
- Impulsive Appraisal II
- (DiscerningEyeAura > 0) Single Mind | Discerning Eye
- Impulsive Appraisal II
- (DiscerningEyeAura > 0) Single Mind | Discerning Eye
- Methodical Appraisal
- (Rarity < 470) Methodical Appraisal
```
## Gathering Variables

These are all the variables you can use inside gathering macro conditionals. If there's a variable that's missing and you think it's useful let me know and I can add it.

```
int PlayerLevel
int Gathering
int Perception
int MaxGp
int Gp
int Item

int SwingsRemaining
int MaxSwings
int Rarity
int Wear

int DiscerningEyeAura
int SingleMindAura
```
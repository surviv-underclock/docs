# surviv.io Timers

This guide explains how to underclock ("overclock") in surviv.io.

## Table of Contents
* [Instructions](#instructions) (how to underclock + advanced instructions requiring more math)
* [Notation](#notation) (how to understand [#instructions](#instructions))
* [Timers](#timers) (why underclock works)
* [Glossary](#glossary) (some useful keywords)
* [Videos](#videos) (watch [#instructions](#instructions) in action)
* [No Overclock](#no-overclock) ("There is no overclock!")

Other pages
* [Data Tables](data-tables.md) (switch delays and deploy groups of all weapons)
* [Old Guide](2020-04-20-old-guide-stark.md) by Stark

## Instructions
### Experimental
These timings work well even with network latency, as long as jitter is low.

#### Double-shot primary
`A 875: ! c 1050: A 1300: ! c`

#### Melee overclock
impossible, so don't bother

#### Grenade quickswitch/overclock (nade spam)
repeat `d ! c` once per throwable

### Theoretical
#### Double-shot primary over 250ms
`999 gun1 251 gun2`  
`A 999: ! c 1000: A 1250: ! c`  
999 can be increased as long as it remains less than 1000, which makes the 250ms possible as a limit

#### Double-shot primary then secondary
`A 999: ! c 1000: B 1250: ! c`  
similar for shooting gun2 then gun 1, and for gun2 twice

#### Shoot M870 3 times over 1250ms, first shot immediately
`m870 900 m870 100 fs 250 m870`  
`! c a 900: ! c 1000: A 1250: !`

#### Shoot SPAS-12 3 times over 1250ms, first shot immediately
`spas 750 spas 250 fs 250 spas`  
`! c a 750: ! c 1000: A 1250: !`

#### Shoot M870 3 times over 1150ms
`999 M8 1 fs 250 M8 900 M8`  
`A 999: ! c 1000: A 1250: ! d a 2150: !`

#### Shoot SPAS-12 3 times over 1000ms
`999 SPAS 1 fs 250 SPAS 750 SPAS`  
`A 999: ! c 1000: A 1250: ! d a 2000: !`

#### Triple shot with MP220+M870 (aka MP220+M870 desync)
`mp220 250 m870 300 mp220`  
`! B 250: ! a 550: !`  
`A 1000: ! B 1250: ! a 1550: !`

### Advanced
5x to 7x shot combos with M870+MP220 (minimize spacing between shots, as in minimize maximum time between adjacent shots)

*These were determined manually and are not guaranteed to be optimal.*
```
5 shots (312.5 ms, 1250 ms total)
m870 312.5 m870 312.5 mp220 312.5 mp220 312.5 m870
m870 fs 312.5 m870 312.5 mp220 312.5 mp220 62.5 fs 250 m870
A 999: ! c 1000: A 1312.5: ! b 1625: ! c b 1937.5: ! c 2000: A 2250: !

6 shots (450 ms, 2250 ms total)
m870 450 m870 450 mp220 450 m870 450 mp220 450 m870
m870 fs 450 m870 450 mp220 100 fs 350 m870 450 mp220 200 fs 250 m870
A 999: ! c 1000: A 1450: ! b 1900: ! c 2000: A 2350: ! b 2800: ! c 3000: A 3250: !

7 shots (625 ms, 3250 ms total)
m870 250 m870 625 mp220 625 m870 625 mp220 625 m870 625 m870
m870 fs 250 m870 625 mp220 125 fs 375 m870 625 mp220 fs 625 m870 375 fs 250 m870
A 999: ! c 1000: A 1250: ! b 1875: ! c 2000: A 2375: ! b 3000: ! A 3625: ! c 4000: A 4250: !
```

Fire all 7 M870+MP220 shots as quickly as possible (minimum first-to-last shot time of 2900 ms)
```
700 M870 300 MP220 fs 250 M870 900 M870 900 M870 300 MP220 fs 250 M870
A 700: ! b 1000: ! A 1250: ! d a 2150: ! d a 3050: ! b 3350: ! A 3600: !
```
Fire all 7 M870+MP220 shots as quickly as possible (minimize time to last shot, at 3150ms)
```
M8 900 M8 100 fs 250 M8 300 MP 300 MP 150 fs 250 M8 900 M8
! d a 900: ! c 1000: A 1250: ! b 1550: ! d b 1850: ! c 2000: A 2250: ! d a 3150: !
```

An optimized algorithm to find best sequences that optimize certain objectives (minimize time from first to last shot, minimize time from start to last shot, minimize maximum spacing) is a difficult and open problem to solve.

surviv.io is actually a very hard math game just because of its weapon timer system.

## Notation
Here is a formal definition of the notation, in ABNF (as described in RFC 5234):
```abnf
sequence = *space 1*(instruction *space)

instruction = label / shoot / shoot-start / shoot-end / switch
                / equip-1 / equip-2 / equip-3 / equip-4

space = SP / CR / LF

; label indicates a wait until a certain time
label = label-delay ":"
label-delay = label-delay-absolute / label-delay-relative
label-delay-absolute = number ; in ms since the beginning
label-delay-relative = "+" number ; in ms since the last label
number = *DIGIT / *DIGIT "." *DIGIT

; all following instructions are executed immediately after each other

shoot-start = "<" ; shoot key down (default is mouse down)
shoot-end = ">" ; shoot key up (default is mouse up)

; press key and release immediately
shoot = "!"
switch = "\" ; switch gun slots (default keybind is T)
; use uppercase to denote free switches
equip-1 = "a" / "A" ; equip primary
equip-2 = "b" / "B" ; equip secondary
equip-3 = "c" / "C" ; equip melee
equip-4 = "d" / "D" ; equip throwables
```
This defines a language that compactly describes a sequence of timed actions that could be converted to a macro and executed.

Switching with `equip other weapon` or a `number key` is irrelevant, so they are not distinguished in the notation. However, `switch weapon slots` can cancel burst weapons' burst shots.

## Timers
### Old System
Every weapon tracks the last time it was fired. After being fired, movement is slowed and no shot is allowed until the `fire delay` has passed. Switching weapons removes the slow penalty and uses the `switch delay` instead of `fire delay` for determining whether a shot is allowed.

For example, one can shoot the SPAS-12 (switch delay 750ms), switch to SV-98 (switch delay 1000ms), shoot, switch to melee, switch to SPAS-12 at time 749 ms, shoot at time 750 ms, switch to SV-98, shoot at time 1000ms, switch to SPAS-12, and shoot at time 1500ms. This pattern explains the name **desync**.

Stark says this system was used until the double pump nerf (Sep 2018), but it existed in client code by Jan 2018, probably unused at the server.

### New system
When switching to a weapon, if the `free switch timer` has expired (initial state), the switch is a **free switch**, and the `free switch timer` expires again after 1000ms (a switch exactly 1000ms later is a free switch).

- If a switch is to melee or throwables, the `effective switch delay` is zero.
- Otherwise, if a switch is a **free switch** and the new weapon's `deploy group` is not that of the old weapon, the `effective switch delay` is 250ms.
- Otherwise, the `effective switch delay` is the `switch delay` of the weapon.

A weapon cannot be fired until its `effective switch delay` has elapsed after the last switch. This is different from the old system, which considers the time of the *last shot* instead of the *last switch*.

In other words,
> When switching weapons, if the last free switch was at least 1000ms ago (or it is the first switch), it's a **free switch**, which allows the player to shoot the new gun usually after 250ms, unless the `deploy group`s of the old and new weapon are the same. If there is no **free switch** (or the `deploy group`s match), then the original `switch delay` is applied.

Melee and grenades always have zero `effective switch delay`. All other weapons have at least 250ms `switch delay`, so they either benefit from the `free switch` or are unaffected if their `switch delay` is already 250ms.

As a result, **desync** now commonly refers to noslowing weapons, but shooting one with 250ms or 300ms switch delay multiple times before using the free switch on the one with large switch delay.

#### Wasted Free Switches
**Free switch**es from pump to pump or cannon to cannon (same `deploy group`) *waste* the **free switch**, and so do **free switch**es to melee or throwables.

In both cases, wasted free switches can be avoided by switching to melee or throwables before the `free switch timer` expires.

#### Melee
Melee cannot be overclocked, even though it always has zero `effective switch delay`. Melee applies damage after a delay, and switching will cancel it. When damage is applied, a cooldown timer is applied so that melee damage will be nullified before it expires. This prevents any potential damage increase over spam-clicking melee.

#### Throwables
Throwables always have zero `effective switch delay`. However, by clicking and immediately switching, the grenade is dropped rather than thrown. This trick can be used to drop many nades in a short time.

#### How to Time
- use a med for a short time and then cancel
- use reload timer on empty secondary gun and then cancel
- use timer when cooking a grenade
- use a metronome
- have a monotonic clock on the wall and refer to it
- get skilled at timing
- use macros

### Corollaries
Useful inferences are listed here.

When using a free switch to shoot twice over 250ms, it is universal and works on any two weapons, regardless of deploy group:
- different weapon types in both slots (1-2 or 2-1)
- same weapon type but different slot (1-2 or 2-1)
- identical weapon in either slot (1-1 or 2-2)

When a free switch is available, using it to switch to a different deploy group is easy by switching directly to the other weapon.

To avoid switching to the same deploy group, it is necessary to switch to melee/throwables before the free switch, and then free switch to the new weapon.

Burst weapons can also be used by having the burst end before switching to melee. Alternatively, to satisfy noslow constraints, bursts can be canceled by swapping weapon slots.

Melee and Throwables are already explained in #timers

Legacy terms
- "hard overclock" is just using free switch to shoot twice over 250ms.
- "soft overclock" is just shooting slowly but also using free switch.

## Glossary
- **Underclock**: sacrificing shots to delay other shots (trade DPS for improved unpredictability), usually to noslow two weapons in rapid succession with a free switch
- **Overclock**: switching weapons to increase DPS (doesn't actually exist except for throwables, and attempts usually result in underclock for guns and melee)
- **Noslow**: switching after a shot to minimize movement penalty to 1 frame
- **Maxfire**: firing both guns at maximum damage rate, measured in damage per second (e.g. shoot Model 94 twice, switch to other Model 94)
- **Desync**: maxfire, but all shots require noslow (e.g. M870 and two MP220 shots, in any of the 3 orders)
- **Free Switch**: a feature of the new switching system, allowing guns to be shot shortly after switching, under certain conditions
- **Deploy Group**: a property of weapons limiting their use of free switch

## Videos
SV-98 underclock (SV-98 double-shot)
https://clips.twitch.tv/BlatantSuspiciousPigeonUWot

M870+SPAS-12 underclock ("double pump" revived)
https://clips.twitch.tv/BillowingDarkKoalaNomNom

Grenade overclock (nade spam)
https://clips.twitch.tv/HomelyKawaiiSproutPRChase

also check out Stark's clips in his guide

## Credits
- beyblade (for solving the mystery behind underclocking within days of seeing it initially and writing the second guide)
- Stark (for writing the first version of a guide)
- Research Team (for confirming my solution and providing various feedback)

Research Team on Discord:
- Stark#0163 (since 2020-03-23)
- Sleepy#3631 (since 2020-03-23)
- wxu#7830 (since 2020-03-23)
- OmegaBox#8139 (since 2020-03-23)
- PenguinChungus#7043 (since 2020-03-23)
- n?#0558 (since 2020-03-24)
- 165 Sniper#6198 (since 2020-03-24)
- Squsodu#8240 (since 2020-04-02)
- SwiPe#0175 (since 2020-04-04)
- beyblade (since 2020-04-12)
- Arctic#9133 (2020-04-02 to 2020-04-10)
- apricot#5676 (2020-03-23 to 2020-04-04)
- robopig3002#7162 (2020-03-23 to 2020-04-02)
- desmos#1685 (2020-03-23 to 2020-04-02)

## No Overclock
**There is no overclock (only underclock).**

> **Spoon boy**: Do not try and do the overclock. That's impossible. Instead, only try to realize the truth.  
> **Neo**: What truth?  
> **Spoon boy**: ***There is no overclock.***

It's very ironic that underclocking is actually a method to ***delay*** shots, yet it's being portrayed as something to ***speed up*** shots. Imagine `spas 250 mosin 750 spas 250 mosin` from regular quickswitch. Then with underclocking, one can forfeit the second spas shot in exchange for being able to delay the first mosin shot by less than 750ms. One can even forfeit the first spas shot and replace the spas with melee, making it appear purely as a mosin double-shot, yet shooting it no faster than regular naïve quickswitch.

Justin and Nick clearly designed the timer system the way they did just to troll very hard against those who are less competent at basic math (without regard to their more advanced math skills).

**There is no overclock.**
<p align="center">
  <img src="no-overclock.png" title="There is no overclock." alt="There is no overclock.">
</p>

This article has been reposted with permission from the original author. Originally posted at `2020-04-20T04:20-0400`, it uses outdated terminology for *underclocking*, but the part about *overclocking* melee and nades is accurate.

# Overclocking Guide by Stark#0163

Overclock, or noslowing one gun two or more times in rapid succession, is a very advanced variation of desync, which is firing both guns at maximum noslowed rates. Overclock seeks to maximize burst damage by minimizing the "critical window" across which damage is dealt. As an extension of desync, it is necessarily also useful in determining optimal maxfire (maximum damage dealt over a certain critical window, full stop) pattern.

It is possible to overclock with every solitary weapon in the game, from the M870 [https://streamable.com/b0aoxv] to the Mosin-Nagant [https://streamable.com/4ar2xv] to the UMP9 (as you must go through 3 shots, start the first shot earlier; N.B. that one may also switch slot positions and switch to other gun at the same time to cut off the burst, thus pushing the first shot time back) [https://streamable.com/ze0n9y] to the MP220 [https://streamable.com/jsll2q], although the usefulness of MP220 overclock is very negligible as it is only narrowly faster than traditional noslow. If overclocking is attempted with 1 shot left, instead of doubleshotting, as it's obviously impossible with 1 cartridge, the reload starts very quickly [https://streamable.com/jl6nks].

Note that overclock does not increase the actual rate of fire, but rather "compresses" shots, slowing the rate of fire of the overclocked gun, but doubling up shots in compensation.

So, why does overclocking work? Well, prior to the removal of the famous double pump, surviv's switch model basically counted from previous switch delays and went off that. If one shot a SPAS-12 (sdelay 0.75), and noslowed off of the SPAS-12, they waited 0.75 seconds (the sdelay) before being able to shoot again. A very simple system.

However, post-double pump removal, the switch system changed. As described by the current Free Switch Model (FSM, also stands for Flying Spaghetti Monster), whenever you switch, if the last free switch was at least 1000ms ago (or you switch for the first time), it's a free switch. A free switch allows the player to shoot the target gun after swapping off of it in a very short period of time (effectively 250ms, times may range from 150ms to 350ms). If there is no free switch then the traditional switch delay is applied. 

Essentially, overclocking is waiting until the free switch (fs) and then doubling the shot at that point. 

As part of FSM, double pump was rendered "impossible" due to the addition of deploy groups. If two guns have an existent deploy group, and their deploy group is identical, then they may not be noslowed between. As of 4/20/2020, pump shotguns (M870, SPAS-12, Hawk 12G) share a deploy group of 1, and the Potato Cannon, Heart Cannon, and M79 share a deploy group of 3. No other gun has a deploy group. Interestingly enough, through the use of the melee or grenade slot, as deploy group is tracked by last gun, double pump is still possible. By interposing fists, the deploy group may be circumvented [https://streamable.com/bzva8o]. This may be done with 2 different pump shotguns as well [https://streamable.com/7ggc0c (done using 1 and 2)] [https://streamable.com/n2ucyt (my preferred method, using switch slot positions)]. On a related note, interposing melee is helpful for when one wants to swap the gun they are overclocking [https://streamable.com/q3txh7].

To overclock, swap to the gun to be overclocked, wait 920ms (I like to visualize this as approximately the time in between 2 M870 shots, traditionally noslowed), shoot, swap off, wait another 100ms (at time 1020ms), swap back, and finally fire again. Beyblade found it more useful to shoot at time 1270ms with a macro, however I prefer to just rapidfire click as it usually produces faster results.

This type of switch can be described as (920, 1020, 1270) with the first number as shoot and swap off gun, the second number for swap back on, and the third as shoot again. Similarly, (999, 1000, 1250) is theoretically possible, but in reality impossible due to the game's netlag.

It's exactly the same with 2 pumps, but one must avoid switching from pump to pump directly, and must switch from pump to melee then pump. Some found it easier to use an initial shot to start off overclocking, however the goal should be no initial shot to reduce telegraph, or a measure of how much startup overclocking takes. Less telegraph is an imperative to reduce predictability and increase usability in an actual fight.

Melee cannot be overclocked, any attempts to do so will result in no damage due to melee's startup. Grenades can be overclocked [https://streamable.com/v3j0a3] by simply swapping off and back to nades, as this eliminates the cooldown time much in the way noslow shortens sniper rifle fire delay. Due to this mechanism, overclocked grenades may not be cooked. Grenade overclock is in reality more similar to traditional noslow than true "overclock."

There are 3 main ways to time a free switch. M-cancelling, short for medcancelling, is using a med for a short time then cancelling it to kill this click [https://streamable.com/9igu8o], as a con it slows you and is by far the most telegraphed way to cc (click cancel).  R-cancelling, or reload cancelling, is reloading then cancelling the reload to cancel the click [https://streamable.com/k9ijx5], requires one non-full secondary gun. And perhaps the least telegraphed of the 3 ccs is G-cancelling, or grenade cancelling [https://streamable.com/7q12q5], simply hotkeying to and from a grenade.

## Glossary
- Overclock: timing shots with a free switch to noslow two weapons in rapid succession.
- Desync: firing both guns at maximum noslowed rate.
- Maxfire: firing both guns at maximum rate, full stop.
- Free Switch: a feature of the new switching system, allowing guns to be shot shortly after switching, under certain conditions (see above).
- Deploy Group: a shared feature of a few guns limiting their noslow ability.
- Telegraph: how much setup and this how predictable an overclock method is.
- M-cancel: med cancel.
- R-cancel: reload cancel.
- G-cancel: grenade cancel.

## Credit
163 Stark, beyblade, Sleepy, n?, 179 Squsodu, 165 Sniper, 173 wxu, 175 SwiPe, OmegaRox, PenguinChungus, Nobody, Ice.

Oh, and 3+ shooting is known, however is to be kept private, at least for now. :163:

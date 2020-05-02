**Switch Delay** (as of 2020-04-20)
```json
{
  "1": [
    "AWM-S", "BLR 81", "Model 94", "Mosin-Nagant", "SV-98", "Scout Elite"
  ],
  "0.9": [
    "Hawk 12G", "Heart Cannon", "M1100", "M134", "M79", "M870", "Potato Cannon"
  ],
  "0.75": [
    "AK-47", "AN-94", "BAR M1918", "CZ-3A1", "DP-28", "FAMAS", "Groza", "Groza-S", "L86A2", "M1 Garand", "M1014", "M1A1", "M249", "M39 EMR", "M416", "M4A1-S", "MAC-10", "MP5", "Mk 12 SPR", "Mk45G", "PKM", "PKP Pecheneg", "QBB-97", "SCAR-H", "SCAR-SSR", "SPAS-12", "SVD-63", "Saiga-12", "Spud Gun", "UMP9", "USAS-12", "VSS", "Vector", "Vector"
  ],
  "0.3": [
    "Bugle", "DEagle 50", "Dual DEagle 50", "Dual Flare Gun", "Dual OT-38", "Dual OTs-38", "Dual P30L", "Dual Peacemaker", "Flare Gun", "MP220", "OT-38", "OTs-38", "Peacemaker", "Rainbow Blaster"
  ],
  "0.25": [
    "Dual G18C", "Dual M1911", "Dual M9", "Dual M93R", "G18C", "M1911", "M9", "M9 Cursed", "M93R", "P30L"
  ]
}
```
Regular Vector and Purple Vector have the same name.

JSONata
```jsonata
$keys($).$lookup($$, $)^(>switchDelay, name){$string(switchDelay): name}
```

**Deploy Groups** (as of 2020-04-20)
```
1: M870, SPAS-12, Hawk 12G
3: Potato Cannon, Heart Cannon, M79
```
No other weapon has a deploy group.

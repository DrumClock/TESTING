**upravit LCD menu :**

```
[menu __main __user __auto_power_off]
type: list
#enable: { 'gcode_macro POWER_DETECT_OFF' in printer }
enable: { 'POWER_DETECT_OFF' in  printer['gcode'].commands }
name: Power supply  
```

 **nefunguje**
 enable: { 'gcode_macro POWER_DETECT_OFF' in printer }

**funkcni**
enable: { 'POWER_DETECT_OFF' in  printer['gcode'].commands }

------------------------------------------------------

**enable: { 'xxx' in  printer['gcode'].commands }**

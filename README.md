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

------------------------------------------------------
**Ignorovat TRIGGER_RUNOUT macro pro první vrstvi tisku**

```
### Disable TRIGGER_RUNOUT macro for the first print layer
{% set enable_runout = true if printer['gcode_move'].gcode_position.z > 0.3 else false %}

{% if  printer['virtual_sdcard'].is_active == true  and  enable_runout  %}
	
	   TRIGGER_RUNOUT .....
 
 {% endif %}
	
```
-------------------------------------------------------
**EMERGENCY STOP tlačítko**

```
[gcode_button emergency_stop]
pin: PA3
press_gcode:  {action_emergency_stop("Press EMERGENCY STOP button !")}
```
------------------------------------------------------
**DUMP_CONFIG S=extruder**

```
[gcode_macro DUMP_CONFIG]
description: Debug: Print all entries of the printer config object
#### DUMP_CONFIG s=extruder
gcode:
 {% set show = params.S|lower %}
 {% set parameters = namespace(output = '') %}
  {% for name1 in printer.configfile.config %}
     {% if name1.split(' ')[0] == show %}        
        {% for name2 in printer.configfile.config[name1] %}
              {% set param = "printer.configfile.config['%s'].%s = %s" % (name1, name2, printer.configfile.config[name1][name2]) %}
        {% set parameters.output = parameters.output +  param + "\n" %}
      {% endfor %}
    {% endif %}
  {% endfor %}
  {action_respond_info(parameters.output)}
```

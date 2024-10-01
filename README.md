**upravit v LCD menu :**

```
[menu __main __user __auto_power_off]
type: list
enable: { 'gcode_macro POWER_DETECT_OFF' in printer }
name: Power supply  
```

 **nefunguje**
 enable: { 'gcode_macro POWER_DETECT_OFF' in printer }

**funkcni**
enable: { 'POWER_DETECT_OFF' in  printer['gcode'].commands }

**nebo**
 enable: { 'gcode_macro POWER_DETECT_OFF' is defined }


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
gcode:
 #### DUMP_CONFIG s=extruder
  {%if 'S' in params %}
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
   {% if  parameters.output|length == 0 %} {% set  parameters.output = "Nothing found for \"DUMP_CONFIG %s\"" % rawparams %} {% endif %}
    {action_respond_info(parameters.output)}
  {% else %}
   {action_respond_info("WARNING: parameter S needed call e.g. DUMP_CONFIG S='printer'")}
  {% endif %} 
```

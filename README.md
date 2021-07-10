# elParaguayo's Qtile Extras

This is a separate repo where I share things that I made for qtile that (probably) won't ever end up in the main repo.

This things were really just made for use by me so your mileage may vary.

Currently available extras:
* Widgets
  * [ALSA Volume Control](#alsa-volume-control-and-widget)
  * [UPower battery indicator](#upower-widget)


## ALSA Volume Control and Widget

This module provides basic volume controls and a simple icon widget showing volume level for Qtile.

### About

The module is very simple and, so far, just allows controls for volume up, down and mute.

Volume control is handled by running the appropriate amixer command. The widget is updated instantly when volume is changed via this code, but will also update on an interval (i.e. it will reflect changes to volume made by other programs).

The widget displays volume level via an icon, bar or both. The icon is permanently visible while the bar only displays when the volume is changed and will hide after a user-defined period.

### Demo

Here is a screenshot from my HTPC showing the widget in the bar. The icon theme currently shown is called "Paper".

_"Icon" mode:_</br>
![Screenshot](images/volumecontrol-icon.gif?raw=true)

_"Bar" mode:_</br>
![Screenshot](images/volumecontrol-bar.gif?raw=true)

_"Both" mode:_</br>
![Screenshot](images/volumecontrol-both.gif?raw=true)


### Configuration

Add the code to your config (`~/.config/qtile/config.py`):

```python
from qtile_extras import widget as extrawidgets

keys = [
...
Key([], "XF86AudioRaiseVolume", lazy.widget["alsawidget"].volume_up()),
Key([], "XF86AudioLowerVolume", lazy.widget["alsawidget"].volume_down()),
Key([], "XF86AudioMute", lazy.widget["alsawidget"].toggle_mute()),
...
]

screens = [
    Screen(
        top=bar.Bar(
            [
                widget.CurrentLayout(),
                widget.GroupBox(),
                widget.Prompt(),
                widget.WindowName(),
                extrawidgets.ALSAWidget(),
                widget.Clock(format='%Y-%m-%d %a %I:%M %p'),
                widget.QuickExit(),
            ],
            24,
        ),
    ),
]
```

### Customising

The volume control assumes the "Master" device is being updated and that volume is changed by 5%. 

The widget can be customised with the following arguments:

<table>
    <tr>
            <td>font</td>
            <td>Default font</td>
    </tr>
    <tr>
            <td>fontsize</td>
            <td>Font size</td>
    </tr>
    <tr>
            <td>mode</td>
            <td>Display mode: 'icon', 'bar', 'both'.</td>
    </tr>
    <tr>
            <td>hide_interval</td>
            <td>Timeout before bar is hidden after update</td>
    </tr>
    <tr>
            <td>text_format</td>
            <td>String format</td>
    </tr>
    <tr>
            <td>bar_width</td>
            <td>Width of display bar</td>
    </tr>
    <tr>
            <td>bar_colour_normal</td>
            <td>Colour of bar in normal range</td>
    </tr>
    <tr>
            <td>bar_colour_high</td>
            <td>Colour of bar if high range</td>
    </tr>
    <tr>
            <td>bar_colour_loud</td>
            <td>Colour of bar in loud range</td>
    </tr>
    <tr>
            <td>bar_colour_mute</td>
            <td>Colour of bar if muted</td>
    </tr>
    <tr>
            <td>limit_normal</td>
            <td>Max percentage for normal range</td>
    </tr>
    <tr>
            <td>limit_high</td>
            <td>Max percentage for high range</td>
    </tr>
    <tr>
            <td>limit_loud</td>
            <td>Max percentage for loud range</td>
    </tr>
    <tr>
            <td>update_interval</td>
            <td>Interval to update widget (e.g. if changes made in other apps).</td>
    </tr>
    <tr>
            <td>theme_path</td>
            <td>Path to theme icons.</td>
    </tr>
    <tr>
            <td>device</td>
            <td>Name of ALSA output (default: "Master").</td>
    </tr>
    <tr>
            <td>tstep</td>
            <td>Amount to change volume by.</td>
    </tr>
</table>

Note: it may be preferable to set the "theme_path" via the "widget_defaults" variable in your config.py so that themes are applied consistently across widgets.



## UPower Widget

This module provides a simple widget showing the status of a laptop battery.

### About

The module uses the UPower DBus interface to obtain information about the current power source.

The widget is drawn by the module rather than using icons from a theme. This allows more customisation of colours.

### Demo

Here is a screenshot showing the widget in the bar.

_Normal:_</br>
![Screenshot](images/battery_normal.png?raw=true)

_Low:_</br>
![Screenshot](images/battery_low.png?raw=true)

_Critical:_</br>
![Screenshot](images/battery_critical.png?raw=true)

_Charging:_</br>
![Screenshot](images/battery_charging.png?raw=true)

_Multiple batteries:_</br>
![Screenshot](images/battery_multiple.png?raw=true)

_Showing text:_</br>
![Screenshot](images/battery_textdisplay.gif?raw=true)

### Configuration

Add the code to your config (`~/.config/qtile/config.py`):

```python
from qtile_extras import widget as extrawidgets
...
screens = [
    Screen(
        top=bar.Bar(
            [
                widget.CurrentLayout(),
                widget.GroupBox(),
                widget.Prompt(),
                widget.WindowName(),
                extrawidgets.UPowerWidget(),
                widget.Clock(format='%Y-%m-%d %a %I:%M %p'),
                widget.QuickExit(),
            ],
            24,
        ),
    ),
]
```

### Customising

The widget allows the battery icon to be resized and to display colours for different states.

The widget can be customised with the following arguments:

<table>
    <tr>
            <td>font</td>
            <td>Default font</td>
    </tr>
    <tr>
            <td>fontsize</td>
            <td>Font size</td>
    </tr>
    <tr>
            <td>font_colour</td>
            <td>Font colour for information text</td>
    </tr>
    <tr>
            <td>battery_height</td>
            <td>Height of battery icon</td>
    </tr>
    <tr>
            <td>battery_width</td>
            <td>Size of battery icon</td>
    </tr>
    <tr>
            <td>battery_name</td>
            <td>Battery name. None = all batteries</td>
    </tr>
    <tr>
            <td>border_charge_colour</td>
            <td>Border colour when charging.</td>
    </tr>
    <tr>
            <td>border_colour</td>
            <td>Border colour when discharging.</td>
    </tr>
    <tr>
            <td>border_critical_colour</td>
            <td>Border colour when battery low.</td>
    </tr>
    <tr>
            <td>fill_normal</td>
            <td>Fill when normal</td>
    </tr>
    <tr>
            <td>fill_low</td>
            <td>Fill colour when battery low</td>
    </tr>
    <tr>
            <td>fill_critical</td>
            <td>Fill when critically low</td>
    </tr>
    <tr>
            <td>margin</td>
            <td>Margin on sides of widget</td>
    </tr>
    <tr>
            <td>spacing</td>
            <td>Space between batteries</td>
    </tr>
    <tr>
            <td>percentage_low</td>
            <td>Low level threshold.</td>
    </tr>
    <tr>
            <td>percentage_critical</td>
            <td>Critical level threshold.</td>
    </tr>
    <tr>
            <td>text_charging</td>
            <td>Text to display when charging.</td>
    </tr>
    <tr>
            <td>text_discharging</td>
            <td>Text to display when on battery.</td>
    </tr>
    <tr>
            <td>text_displaytime</td>
            <td>Time for text to remain before hiding</td>
    </tr>
</table>



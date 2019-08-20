# Using i3 as WM in xfce4 ( without xfdesktop ) on arch

I looked up how to run i3 inside xfce4-DE. If you want to do the same, simply follow these steps.


## Step 1 - Install dependencies
```
pacman -S xfce4 xfce4-goodies human-icon-theme i3
```

## Step 2 - Setup X to start the xfce4 desktop environment
`cp /etc/X11/xinit/xinitrc ~/.xinitrc`

```
// .xinitrc / 
exec startxfce4
````

## Step 3 - Startx 
`startx` - Runs the xfce4 DE - you can immediately logout again. 
This step is required to init the xfce-config files for your user (~/.config/xfce4)

## Step 4 - Modify config files to disable session saving, load i3 as WM and disable xfdesktop
Replace the content of the file `~/.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-session.xml` with this:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<channel name="xfce4-session" version="1.0">
  <property name="general" type="empty">
    <property name="FailsafeSessionName" type="string" value="Failsafe"/>
    <property name="LockCommand" type="string" value=""/>
    <property name="SessionName" type="string" value="Default"/>
    <!-- disable session saving -->
    <property name="SaveOnExit" type="bool" value="false"/>
  </property>
  <property name="sessions" type="empty">
    <property name="Failsafe" type="empty">
      <property name="IsFailsafe" type="bool" value="true"/>
      <property name="Count" type="int" value="5"/>
      <property name="Client0_Command" type="array">
        <!-- use i3 as WM -->
        <value type="string" value="i3"/>
      </property>
      <property name="Client0_Priority" type="int" value="15"/>
      <property name="Client0_PerScreen" type="bool" value="false"/>
      <property name="Client1_Command" type="array">
        <value type="string" value="xfsettingsd"/>
      </property>
      <property name="Client1_Priority" type="int" value="20"/>
      <property name="Client1_PerScreen" type="bool" value="false"/>
      <property name="Client2_Command" type="array">
        <value type="string" value="xfce4-panel"/>
      </property>
      <property name="Client2_Priority" type="int" value="25"/>
      <property name="Client2_PerScreen" type="bool" value="false"/>
      <property name="Client3_Command" type="array">
        <value type="string" value="Thunar"/>
        <value type="string" value="--daemon"/>
      </property>
      <property name="Client3_Priority" type="int" value="30"/>
      <property name="Client3_PerScreen" type="bool" value="false"/>
      <property name="Client4_Command" type="array">
        <!-- empty value, we wont xfdesktop to start -->
        <value type="string" value=""/>
      </property>
      <property name="Client4_Priority" type="int" value="35"/>
      <property name="Client4_PerScreen" type="bool" value="false"/>
    </property>
  </property>
</channel>
```

## Step 5 - kill cache & reboot
- `rm -rf ~/.cache/*`
- Reboot your system

## Step 6 - run startx
`startx` - After a few seconds you should see something like this:
![xfce4 with i3](https://raw.githubusercontent.com/EnricoVogt/xfce4-i3-arch/master/xfce4-i3-preview.PNG)

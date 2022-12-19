---
date: 2022-12-11 13:48:57.256754
description: A tutorial on how to create an Advent Calendar iOS 16 Locks Screen widget.
tags: Lockscreen_Widgets,SwiftUI,Xcode,iOS,tutorial
title: Advent Calendar
---


Let's create an iPhone app that displays an advent calendar Lock Screen widget.

<!--more-->

# Create a new Xcode project

You'll need Xcode 14.0 or later for this project.

Choose "File > New Project", or click on the "Create a new Xcode Project" button.
 
![Welcome to Xcode](/assets/posts/2022-12-11-Advent_Calendar/Welcome_to_Xcode.png)


![Create App](/assets/posts/2022-12-11-Advent_Calendar/Create_App.png)


![Project settings](/assets/posts/2022-12-11-Advent_Calendar/Project_settings.png)


# Add a widget target
### File > Add Target

Use the "File > Add Target" menu item to add a "Widget target".



![Add Widget Target](/assets/posts/2022-12-11-Advent_Calendar/Add_Widget_Target.png)



![Widget Target Options](/assets/posts/2022-12-11-Advent_Calendar/Widget_Target_Options.png)
You don't need a Live Activity. You might want a Configuration intent.





![Activate Widget Scheme](/assets/posts/2022-12-11-Advent_Calendar/Activate_Widget_Scheme.png)




# Change the preview widget family to .accessoryRectangular


![Preview family](/assets/posts/2022-12-11-Advent_Calendar/Preview_family.png)

# Draw the widget

Lock Screen widgets render in xxx mode. This limits you to using alpha and transparency.


![Draw Christmas](/assets/posts/2022-12-11-Advent_Calendar/Draw_Christmas.png)


# Set the Metadata



![Set Widget Metadat](/assets/posts/2022-12-11-Advent_Calendar/Set_Widget_Metadat.png)



# Simulate
### ⌘-R to build and run

Command-R to start running the app on the simulator.

![Running app](/assets/posts/2022-12-11-Advent_Calendar/Running_app.png)


# Lock the screen
### ⌘-L or Device > Lock Screen

![Lock screen](/assets/posts/2022-12-11-Advent_Calendar/Lock_screen.png)


# Long press to customize

![Customize step 1](/assets/posts/2022-12-11-Advent_Calendar/Customize_step_1.png)

# Choose Lock Screen

![Customize step 2](/assets/posts/2022-12-11-Advent_Calendar/Customize_step_2.png)


# Tap Add Widgets

![Customize step 3](/assets/posts/2022-12-11-Advent_Calendar/Customize_step_3.png)

# Choose Advent Calendar

![Add Widgets](/assets/posts/2022-12-11-Advent_Calendar/Add_Widgets.png)

# Choose the Shape

![Choose widget size](/assets/posts/2022-12-11-Advent_Calendar/Choose_widget_size.png)

# Final result

![Final](/assets/posts/2022-12-11-Advent_Calendar/Final.png)

# Next steps

# Improve graphics

### Timeline only needs to update once a year, on Christmas

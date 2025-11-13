---
layout: project
title: Delivery end effector
description: Four-bar EE for the rover arm
technologies: [Autodesk Inventor, Ansys]
image: /assets/images/EE-structure-open.png
---


Cornell Mars Rover end effector for the Extreme Delivery Mission in the University Rover Challenge.
Tasks: open boxes with hinged lids; pick up, carry, and deliver objects up to 5kg in mass - objects may consist of hand tools or instruments, rocks or supply containers.
The end effector features four bar linkages and a geartrain (1:2 gear ratio) driven by a torque servo. It is able to exert up to approximately 100N of force. The silicone molded grips introduce compliance and friction for better grasp on objects with more complex shapes.

![CAD of the Delivery End Effector]({{ "/assets/images/EE-CAD.png" | relative_url }}){: .inline-image-l}

The Ansys below shows the loading of the claws when picking up 5kg parallel to the ground.

![EE End Plate Analysis]({{ "/assets/images/EE-ansys-5kg-load-stress" | relative_url }}){: .inline-image-l}

This is how I solved the problem:

```python
    some code = 10;
    plot();
```


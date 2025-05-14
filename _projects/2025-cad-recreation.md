---
layout: project
title: Foosbot CAD Recreation
description: Class project with CAD
technologies: [Autodesk Fusion 360]
image: /assets/images/foosbot-cad.jpg
---


As part of a class project, I recreated an object in CAD using Autodesk Fusion 360. I chose to recreate the Foosbot, a handheld foosball player that spins like a real foosball player when the arms are pushed in. The Foosbot I modeled is shown below. 

![Photo of Foosbot]({{ "/assets/images/foosbot-photo.jpg" | relative_url }})

I used a ruler to measure the dimensions of the Foosbot. A sketch of the Foosbot with main dimensions is pictured below. 

![Photo of Foosbot Sketch]({{ "/assets/images/foosbot-sketch.jpg" | relative_url }})

To model the inside of the Foosbot, I took it apart. The pieces are shown below. 

![Photo of Foosbot exploded]({{ "/assets/images/foosbot-exploded.jpg" | relative_url }})

To model the Foosbot, I modeled the smaller parts separately and then assembled them. The Foosbot breaks down into 3 main parts: the front of the case, back of the case, and the arms. The arms break down further into many more smaller parts, including the cap, arm case, spring, metal rod, and more.

All of these parts are generally cylindrical, so I sketched circles and extruded them. I used a circular pattern for geometry such as the grooves in the cap, which go all the way around the cap. I used coils for the springs with a smaller pitch on the ends to mimic the real springs. Then, I made a subassembly and arranged all the parts of the arm together, using the align and move tools to put the pieces in the right places. I used a rigid group to make all the parts of the arm move together when I put them in the full assembly. 

For the front of the case, there are 3 main shapes, so those were made first. I used the loft tool to create the curved surface of the head and the slanted edges of the arms. I sketched the shape of the bottom part and then projected it onto a slanted plane above to create the angled surface. I used the shell tool to make the shapes hollow. Then, I added the shapes on the inside by sketching them on a horizontal plane above the inside of the toy and using an offset extrude to put the shapes on the curved inside surface of the case. I used a similar process for the back of the case. 

After making these parts, I added them to an assembly and used the align and move tools to put them in the correct positions. 

If I were to do this again, I would use more parameters and functions to relate dimensions and make it easier to adjust them. Whenever I realized I made a mistake, it was difficult to change. I would also put the parts into a full assembly as I go rather than creating the assembly at the end because it would be easier to catch mistakes with dimensions. 

The finished rendering is at the top of this page. A rendering that has been opened to show the inside of the Foosbot is pictured below. 

![Photo of Foosbot exploded]({{ "/assets/images/foosbot-cad-exploded.jpg" | relative_url }})

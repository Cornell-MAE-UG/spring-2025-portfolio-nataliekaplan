---
layout: project
title: Heat Transfer in Turbine Blades
description: Analyzed the heat transfer through a turbine blade with and without a thermal barrier coating
technologies: [None]
image: /assets/images/heat-transfer-work.jpg
---

In problem 3 of homework 3, I analyzed the heat transfer through a turbine blade with and without a thermal barrier coating (TBC). A diagram of the turbine blade with the TBC is shown below. 

<div style="text-align:center;">
  <img src="{{ "/assets/images/heat-transfer-diagram.png" | relative_url }}" style="max-width:550px;">
</div>

To solve this problem, I used thermal resistance networks. First, I analyzed the system without the TBC. This thermal resistance network includes the resistances from the convection on the outside, the conduction through the superalloy blade, and the convection on the inside. Since they are in series, I added the thermal resistances to get the total thermal resistance. The fact that the resistances are all in series also tells us that the heat transfer through the system $q$ is the same throughout. I then related the thermal resistance, heat transfer, and temperature difference using the equation $\Delta T = qR_{tot}$ and solved for $T_1$. I found that $T_1 = 1236K$, which is greater than the maximum allowable temperature for the superalloy blade $1200K$. To find the temperature of the blade with the TBC, I used a similar process but included the TBC and the bonding agent in the thermal resistance network. I found that $T_3=1104K$, which is less than the maximum allowable temperature of the blade, meaning that with the TBC the superalloy blade can be maintained below $T_max$. 

From this problem, I learned how to use thermal resistance networks to simplify heat transfer systems. The thermal resistance networks served as a good visual for how the heat was flowing through the system and made it easy to find the temperature at any border. This problem also taught me about how important thermal barrier coatings can be on turbine blades and in other machines that operate at high temperatures. The thermal barrier coating allowed the turbine to be operated under the maximum allowable temperature. 

The homework assignment is linked [here]({{ "/assets/heat-transfer-pset.pdf" | relative_url }}). My work is shown below. 

<div style="text-align:center;">
  <img src="{{ "/assets/images/heat-transfer-work.jpg" | relative_url }}" style="max-width:550px;">
</div>
---
layout: project
title: Torque Wrench
description: Designed a torque wrench for Mechanics of Materials class
technologies: [MATLAB, Fusion, Ansys]
image: /assets/images/TR_CAD.png
---

**Overview**

I designed a non-ratcheting, 3/8 inch drive instrumented torque wrench rated for 600 in-lbf. Torque is transduced using strain gauges bonded to the outer surfaces of the wrench at high strain locations. To validate the design, I used hand calculations and finite element analysis. 

**Design Constraints**

Design will include selecting an appropriate material and dimensions to meet or exceed the following requirements:
- attain at least 1.0 mV/V output at the rated torque of 600 in-lbf.
- safety factor of $X_0$ = 4 for yield or brittle failure
- safety factor of $X_K$ = 2 for crack growth from an assumed crack of depth 0.04 inches (1 mm)
- fatigue stress safety factor of $X_S$ = 1.5
- material must be a steel, aluminum, or titanium alloy

**Hand Calculations**

The base design used for hand calculations is shown below. 

<p align="center">
  <img src="{{ '/assets/images/TR_sketch.png' | relative_url }}" width="550">
  <br>
  <em>Figure 1: Sketch of the base design for the torque wrench.</em>
</p>

Given the max torque, length from drive to where load is applied, width, thickness, distance from center of drive to center of strain gauge, Young's modulus, Poisson's ratio, tensile strength, fracture toughness, and fatigue strength for $10^6$ cycles, I calculated the load point deflection, maximum normal stress, safety factor for strength, safety factor for crack growth, safety factor for fatigue, strain at the gauge, and the gauge output. 

In order to iteratively test different dimensions and materials to see what would meet the design requirements, I wrote a MATLAB script to perform these calculations, which is shown below. 

```matlab
    function [defl, sigma_max, X_strength, X_crack, X_fatigue, strain, mVoverV] = TRstressDeflectionAnalysis(M, L, h, b, c, E, nu, su, KIC, sfatigue)
    % TRstressDeflectionAnalysis: 
    %
    %   INPUTS
    %   M = max torque (in-lbf)
    %   L = length from drive to where load applied (inches)
    %   h = width
    %   b = thickness
    %   c = distance from center of drive to center of strain gauge
    %   E = Young's modulus (psi)
    %   nu = Poisson's ratio
    %   su = tensile strength use yield or ultimate depending on material (psi)
    %   KIC = fracture toughness (psi sqrt(in))
    %   sfatigue = fatigue strength from Granta for 10^6 cycles
    %
    %   OUTPUTS
    %       defl = load point deflection
    %       sigma_max = max normal stress
    %       X_strength = safety factor for strength
    %       X_crack = safety factor for crack growth
    %       X_fatigue = safety factor for fatigue
    %       strain = strain at gauge (microstrain)
    %       mVoverV = output at 600 in-lbf using half bridge
    %
    %   Natalie Kaplan

    %titanium alpha-beta alloy, Ti-3Al-5Mo, annealed
    % [defl, sigma_max, X_strength, X_crack, X_fatigue, strain, mVoverV] = TRstressDeflectionAnalysis(600, 16, 0.5, 0.75, 1.0, 14500000, 0.323, 114000, 33700, 60000)  

    I = b*h^3/12;
    a = 0.04; %crack length in inches

    sigma_max = (M * c) / (b * h^2 / 6); % Calculate maximum normal stress
    X_strength = su/sigma_max;
    X_crack = KIC / (1.12*sigma_max*sqrt(pi*a)); %safety factor for crack growth
    X_fatigue = sfatigue / sigma_max; %safety factor for fatigue
    defl = (M * L^2) / (3 * E * I); % Calculate load point deflection
    strain = ((((M/L)*(L-c)*(h/2))/I)/E)*10^6; %microstrain
    mVoverV = strain * 10^-3 * 2; % Calculate output at 600 in-lbf using half bridge

    if mVoverV >= 1 && X_strength >= 4 && X_crack >= 2 && X_fatigue > 1.5
        disp('Design is safe under the given conditions.');
    else
        disp('Design does not meet safety requirements.');

    end
```

The material I chose that met all of the design requirements is Titanium Alpha-Beta alloy (Ti-3Al-5Mo, annealed). According to the Granta Edupack database, for this material Young's Modulus is 14.5 ksi, Poisson's ratio is 0.323, the yield strength is 114 ksi, the fracture toughness is 33.7 $psi\sqrt{in}$, and the fatigue strength for $10^6$ cycles is 60 ksi.

The dimensions I chose are L = 16 in, h = 0.5 in, b = 0.75 in, and c = 1 in. 

**CAD Model**

I used Fusion to create a CAD model of the torque wrench. The model is shown below: 

<p align="center">
  <img src="{{ '/assets/images/TR_CAD.png' | relative_url }}" width="550">
  <br>
  <em>Figure 2: CAD model for the torque wrench. </em>
</p>

Key dimensions of the CAD model are shown below: 


<div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">
  <img src="{{ '/assets/images/TR_b.png' | relative_url }}" width="300">
  <img src="{{ '/assets/images/TR_h.png' | relative_url }}" width="300">
</div>

<p align="center">
  <em>Figure 3: Width and thickness. </em>
</p>

<div style="display: flex; justify-content: center; gap: 20px; flex-wrap: wrap;">
  <img src="{{ '/assets/images/TR_length_plus_1.png' | relative_url }}" width="350">
  <img src="{{ '/assets/images/TR_params.png' | relative_url }}" width="200">
</div>

<p align="center">
  <em>Figure 4: Length and document parameters. Length from far end to the center of the drive is 16 inches. One more inch is added on the end for stability. </em>
</p>
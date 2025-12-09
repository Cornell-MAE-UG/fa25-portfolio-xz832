---
layout: project
title: Torque Wrench Analysis and Potential Design
description: Analysis of the performance of a torque wrench based on given parameters and improvements to satisfy factors of safety and strain gauge sensitivity
technologies: [Autodesk Inventor, Ansys, MATLAB]
image: /assets/images/EE-structure-open.png
---

Our goal for this project is to design a non-ratcheting, 3/8 inch drive instrumented torque wrench rated for 600 in-lbf. Torque will be transduced using strain gauges bonded to the outer surfaces of the wrench at high strain locations. The design of the torque wrench will have to satisfy the following requirements:
1. attain at least 1.0 mV/V output at the rated torque of 600 in-lbf
2. safety factor of Xo = 4 for yield failure as it is a ductile material
3. safety factor of Xk = 2 for crack growth from an assumed crack of depth 0.04 inches (1 mm)
4. fatigue stress safety factor of Xs = 1.5
5. material must be a steel, aluminum or titanium alloy.

We evaluated the torque wrench performance both with hand calculations, scripted in the MATLAB code below:
```
M = 600; % max torque (in-lbf)
L = 15; % length from drive to where load applied (inches)
h = 0.55; % width
b = 1.3; % thickness
c = 1; % distance from center of drive to center of strain gauge
E = 966.E4; % Young's modulus (psi)
nu = 0.33; % Poisson's ratio
su = 377.E2; % tensile strength use yield or ultimate depending on material (psi)
KIC = 3005.E1; % fracture toughness (psi sqrt(in))
sfatigue = 205.E2; % fatigue strength from Granta for 10^6 cycles
name = '6061 Aluminum'; % material name
GF = 155;
k = 1;
crack_depth = 0.04;
Force = M/L;
I = (b*(h^3)) / 12;
max_norm_stress = (M * (h/2)) / I;
sig1 = max_norm_stress;
sig2 = 0;
sig3 = 0;
sig_a = max_norm_stress;
sig_m = 0;
sig_arhat = sqrt(sig_a * (sig_m + sig_a));
Sg = (6 * M) / (h^2 * b);
K = 1.12 * Sg * sqrt(pi * crack_depth);
%sig_ar = sfatigue * (2 * N)^b_fatigue;
ss = sig1;
%Factor of Safeties
X_k = KIC / K;
X_o = su / ss;
X_s = sfatigue / max_norm_stress;
%point load
point_load_deflection = (Force * (L^3)) / (3 * E * I);
%guage
sig_at_c = ((Force * (L)) * (h/2)) / I;
%strain = sig_at_c/E;
%strainguage = strain * 1e6 ;
FEM_strain = 8.8566e-4*1e6;
output = (GF * k * FEM_strain) / 1e3;
%results
fprintf('\nStress and deflection analysis\n');
fprintf('load point deflection = %.3f in\n', point_load_deflection);
fprintf('max normal stress = %.2f ksi\n', max_norm_stress/1e3);
fprintf('\nSafety factor results:\n');
fprintf('safety factor for strength = %.2f\n', X_o);
fprintf('safety factor for crack growth = %.2f\n', X_k);
fprintf('safety factor for fatigue = %.2f\n', X_s);
fprintf('\nStrain gauge results:\n');
fprintf('strain at gauge = %.0f microstrain\n', FEM_strain);
fprintf('output = %.2f mV/V at %d in-lbf using full bridge\n', output, M);
```
as well as FEM analysis using Ansys. Our results are presented below.

![CAD of the Delivery End Effector]({{ "/assets/images/EE-CAD.png" | relative_url }}){: .inline-image-l}

The Ansys below shows the loading of the claws when picking up 5kg parallel to the ground.

![EE End Plate Analysis]({{ "/assets/images/EE-ansys-5kg-load-stress.png" | relative_url }}){: .inline-image-l}

This is how I solved the problem:

```python
    some code = 10;
    plot();
```


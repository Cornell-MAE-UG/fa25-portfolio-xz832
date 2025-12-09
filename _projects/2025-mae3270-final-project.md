---
layout: project
title: Torque Wrench Analysis and Potential Design
description: Analysis of the performance of a torque wrench based on given parameters and improvements to satisfy factors of safety and strain gauge sensitivity
technologies: [Autodesk Inventor, Ansys, MATLAB]
image: /assets/images/DesignDef.png
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

![Dimensions of the torque wrench]({{ "/assets/images/dimensions.png" | relative_url | width = 100}})

L = 15 in

h = 0.55 in

b = 1.3 in

c = 1 in

Images of our CAD model:
![CAD of the updated torque wrench design]({{ "/assets/images/designCAD.png" | relative_url | width = 100}})

Part drawing showing all key dimensions:
![Key dimensions]({{ "/assets/images/designPartDrawing.png" | relative_url | width = 100}})

We used Aluminum 6061-T6. It is a ductile material and an industry standard. It has a Young’s modulus of 9.66 x 10<sup>6</sup> psi, Poisson’s ratio of 0.33, yield tensile strength of 3.77 x 10<sup>4</sup> psi, fracture toughness of 30050 psi sqrt(in), and fatigue strength for 10<sup>6</sup> cycles of 20500 psi.

How loads and boundary conditions were applied to our FEM model:
![Ansys loads and boundary conditions]({{ "/assets/images/Diagram for BC.jpg" | relative_url | width = 100}})

Normal strain contours (in the strain gauge direction) from FEM
![Ansys normal strain contours]({{ "/assets/images/NormalStrainAlongZ.png" | relative_url | width = 100}})

Contour plot of maximum principal stress from FEM
![Ansys stress contour plot]({{ "/assets/images/MaxPrincipalStress.png" | relative_url | width = 100}})

Maximum normal stress is 53811 psi (at where the drive meets the handle), strain at strain gauge location is 886 microstrain, and deflection of load point is 0.41648in.

Strain at gauge from FEM = 886 microstrain, equivalent to 137.28 mV/V at 600 in-lbf using full bridge

We are using a semiconductor bar-shaped strain gauge from Haptica, model SS-027-013-500 P. We chose this strain gauge due to its incredible high sensitivity, since we do not have a strict budget constraint. We are using a full-bridge strain gauge configuration, which we can comfortably fit onto our torque wrench design given the following dimensions:

Total Length mm (inch): 0.686 (0.027)

Active Length mm (inch): 0.330 (0.013)

Width mm (inch): 0.229 (0.009)

Thickness mm (inch): 0.010 (0.0004)

Gauge Factor: 155±10

Source: [Haptica Sensing Semiconductor Strain Gauges](https://www.hapticasensing.com/products/semiconductor-strain-gauges/bar-gauges/)

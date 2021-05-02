---
layout: post
title: Plaxis-Python Connection - Seismic Deformation
---

For underground structures, a rough but reasonable simplification is pseudo-static deformation method. In this method, we apply seismic strain which can be calculated as the ratio of effective PGV (Peak Ground Velocity) to effective shear wave velocity.

$$ \gamma = \frac{PGV_e}{VS_e}$$

Effective PGV can be multiplied with depth dependent reduction factors (see FHWA-NHI-10-034) and maximum shear wave velocity obtained from geophysical tests with almost zero strain can be converted to effective shear wave velocity based on recommendations of FHWA or Eurocode 8.

A simple Python code can be written to implement lateral deformation profile in Plaxis to simulate seismic loading. If you locate this Python file inside the Bentley folder (< PLAXIS installation folder >\pytools\input) this can be directly called from Plaxis Input.

<script src="https://gist.github.com/berkdemir/599f578444b4aa51ddd931b998eb5704.js?file=PLX_SeismicDeformations_BD.py"></script>

To determine the effective shear wave velocity, I have developed a simple methodology and will present this as a part of a paper in the near future (hopefully in WTC 2022). I will not go into detail too much, but you can understand from the code that it is a iterative process.

- Calculate seismic shear strain using reduced PGV based on the depth of tunnel and maximum shear wave velocity.
- Ignoring any static strain, calculate the G/Gmax based on either one of the presented approaches.
- Calculate shear modulus reduction ratio as square root of the G/Gmax.
- Using the calculated ratio, calculate effective shear wave velocity.
- Calculate new seismic shear strain using reduced PGV and new effective shear wave velocity.
- Continue until reasonable difference is obtained between Vsi+1 and Vsi.

Here is the simple code that calculates the effective shear wave velocity:

<script src="https://gist.github.com/berkdemir/599f578444b4aa51ddd931b998eb5704.js?file=EffectiveShearWave.py"></script>


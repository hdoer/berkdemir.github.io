---
layout: post
title: Hoek-Brown Parameters Database
---

If you will use Hoek-Brown in your Python code, you may want to recommend some constants based on rock type. There is a widely used table in literature by Hoek and others that we use to select Modulus Ratio and material constant (mi) in the absence of high quality laboratory tests. 

I have done the manual labour, don't write it all again. A dictionary called `RockDict` is given in the following Gist. Rock types are given as keys of dict and a sub-dictionary with:

* MR: Modulus Ratio
* MRSTD: Modulus Ratio Deviation
* Type: Rock Type (Igneous, Metamorphic, Sedimentary)
* mi: Material Constant
* miSDT: Material Constant Deviation

<script src="https://gist.github.com/berkdemir/175fb9536cee53c57525dd1781736ce2.js"> </script>


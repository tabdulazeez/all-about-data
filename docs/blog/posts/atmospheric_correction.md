---
date: 2023-08-13
authors: [tabdulazeez]
description: >
    Landsat Atmospheric Correction Using R
categories:
  - Remote Sensing 
---

# Landsat Atmospheric Correction Using R 
The package we will be using is `library(RStoolbox)`. Free and open source.

<!-- more -->

Read the meta 
```R
meta2011 = readMeta('LT05_L1TP_130045_20060424_20161122_01_T1_MTL.txt')
summary(meta2011)

```

Stack individual landsat bands in a stack
```R
p22_2011 = stackMeta(meta2011) # stack individual landsat bands in a stack

dn2_rad=meta2011$CALRAD # extract offset gain data

dn2_rad


```

Convert dn to top of the atmospheric radiant
```R
p22_2011_rad= radCor(p22_2011,metaData = meta2011, method = 'rad')
p22_2011_rad


```


Top of atmosphere reflectance (Obtain TOA)

```R 
p22_2011_ref = radCor(p22_2011, metaData = meta2011, method = 'apref')
p22_2011_ref

```

Haze correction

```R 
haze = estimateHaze(p22_2011,darkProp = 0.01, hazeBands = c("B1_dn", "B2_dn","B3_dn","B4_dn"))
haze

```

Dark object extraction ( DOS )

```R
p22_2011_dos = radCor(p22_2011, metaData = meta2011,darkProp = 0.01, method = 'dos')
p22_2011_dos 

```

 CALCULATE NDVI

```R
ndvi = spectralIndices(p22_2011, red = "B3_dn", nir = 'B4_dn', indices = 'NDVI')

```

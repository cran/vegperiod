# vegperiod

[![Travis-CI Build Status](https://travis-ci.org/rnuske/vegperiod.svg?branch=master)](https://travis-ci.org/rnuske/vegperiod) 
[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/rnuske/vegperiod?branch=master&svg=true)](https://ci.appveyor.com/project/rnuske/vegperiod) 
[![Package-License](https://img.shields.io/badge/license-GPL--3-brightgreen.svg?style=flat)](http://www.gnu.org/licenses/gpl-3.0.html) 
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.1466541.svg)](https://doi.org/10.5281/zenodo.1466541)

The vegetation period, or growing season, is the period of the year when the weather conditions are sufficient for plants to grow. This package collects climatological or thermal growing seasons that can be calculated from daily mean temperatures and the day of the year (DOY). Because of their simplicity, they are commonly used in plant growth models and climate change impact assessments.

The concept of a temperature driven vegetation period holds mostly for the temperate climate zone. At lower latitudes, other factors such as precipitation and evaporation can be more decisive. Some methods such as GSL of `ETCCDI` are employed globally (with a half year shift in the southern hemisphere). Others have a smaller area of application as they have been parameterized with local to regional observations. `Menzel` and `vonWilpert` are used throughout Germany.

The package also includes functions for downloading open meteo data from Germany's National Meteorological Service (Deutscher Wetterdienst, DWD).


## Installation
A development version of the package `vegperiod` can be installed from Github using the package devtools.

```r
# install.packages("devtools")
devtools::install_github("rnuske/vegperiod")
```

## Usage
Vegetation periods a calculated using the function `vegperiod()`.  One has to choose at least a start and end method. Some methods, such as 'Menzel', require additional arguments.

```r
data(goe)
vegperiod(dates=goe$date, Tavg=goe$t, 
          start.method="Menzel", end.method="vonWilpert", 
          species="Picea abies (frueh)", est.prev=5)
```

### Implemented start and end methods
Some common methods for calculating the onset and end of vegetation periods are already implemented. Popular choices with regard to forest trees in Germany are 'Menzel' and 'vonWilpert'.

#### Start methods
* **Menzel** implemented as described in Menzel (1997). Parameterized for 10 common tree species. Needs previous years chill days.
* **StdMeteo** / **ETCCDI** a simple threshold based procedure as defined by the Expert Team on Climate Change Detection and Indices (cf. ETCCDI 2009 and Frich et al. 2002). Leading to quite early vegetation starts.
* **Ribes uva-crispa** using leaf out of gooseberry as indicator. Developed by Germany's National Meteorological Service (Deutscher Wetterdienst, DWD). Presented in the section forestry of DWD's 'Climate Atlas'. It is more robust against early starts than common simple meteorological procedures.

#### End methods
* **vonWilpert** based on von Wilpert (1990). Originally developed for "Picea abies" in the Black Forest but currently used for all tree species throughout Germany.
* **LWF-BROOK90** a reimplementation of the LWF-BROOK90 VBA version of vonWilpert (Hammel and Kennel 2001).
* **NuskeAlbert** a very simple method inspired by standard climatological practices.
* **StdMeteo** / **ETCCDI** a simple threshold based procedure as defined by the Expert Team on Climate Change Detection and Indices (cf. ETCCDI 2009 and Frich et al., 2002). Leading to quite late vegetation ends.

### Downloading data from DWD
Germany's National Meteorological Service offers open meteo data in its [Climate Data Center](https://www.dwd.de/EN/climate_environment/cdc/cdc.html).
The files are organized in deep folder structures and end with an arcane/legacy EOF character. 
The Function `read.DWDdata()`deals with all of that and returns a data.frame. Beware there might be missing values and inhomogeneities.

Note: Downloading 'historical' data from DWD with `read.DWDdata()` requires the package 'curl'.


## Contributions
Further start and end methods or download functions are more than welcome! 

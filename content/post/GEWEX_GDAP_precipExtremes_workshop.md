+++
title = "GEWEX/GDAP Workshop on Precipitation Extremes"
date = 2018-07-09T09:37:44+02:00
draft = true

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = ""
preview = true


math = true
+++

# DAY 1

## Remy Roca

- [IPWG website with list of precipitation datasets](http://www.isac.cnr.it/~ipwg/data/datasets.html)  
- Reanalyses: MERRA, MERRA2, ERA interim, JRA55, CFSR 


Precipitation assessment: extreme events chapter

- What metrics?
- Evolution in time?
- how to define uncertainty?

## Lisa Alexander

- critical gaps in the amount, quality and availabiltiy of data for precip extremes
- we are at a point where satellite records are long enough that we would like to get the two communities together (satellite and in situ and reanalyses)
- For the guidance document
	- a maturity index for each dataset

## Sonia Seneveratne

### Overview of IPCC AR6
- Dec 2019: papers submitted deadline
- Sep 2020: papers accepted deadline
- 12 chapters
- CH11: Weather and extremes in a changing climate
	- extreme types
	- obs for extremes and limitations
	- mechanisms, drivers and feedbacks
	- ability of mdels to simulate extrems
	- some case studies
- Hawkins and sutton 2009: BAMS; 2011 Clym Dyn
- Mueller et al 2011 GRL; 2013  HESS
- Meuller and senerveratne 2014; GRL
- models generally exhibit opposite sign of SM-P feedbacks both for spatial and temporal relationships compared with obs (satellite)

## Andreas Becker

- long term global radar systems: Heistermann et al 2013
- radar data is also affected by other things (like birds insects plans etc) and so is ok for real time assessment but now climate assessment
- but there is hope. a number of organisations have looked into this and in the future this should be possible
- Saltikoff 2018 ERAD; Potential use of radar data for climate monitoring

## Robert Dunn

### HADEX3
- Coverage of extremes has not changed much between AR4 and AR5
- HADEX data came from ECAD, SACAD and LACAD
- Structural Uncertainty
	- weighting functions
	- stations within a gridbox
	- longterm stations
	- gridding method
	- gridding network
		- sub sample 25%, 50% sations etc... and then calculate indices and interpolate (monte carlo analysis) 
- Station network is important!
- Still missing lots of data
- Next steps
	- QC tests on indices
	- interpolate anomalies?

## Hirohiko Masunaga - Satellite Precipitation - Status of GEWEX precip assessment with a focus on extremes

- previous assessment (GRP/GDAP) took a decade to complete. It is desired to address the urgent needs of the broad science community in a timely manner
- Foci of assessment
	- glb and reg clim
	- time series anlasis
	- extremes
	- frozen ex
	- uncertainties
- 20% variance among datasets in the equatorial zonal mean precipitation over land and ocean
- mean rainfall differences are smaller than heavy ex differences in radar and microwave imager
- extreme precip has its own sources of biases that are qualitatively different from climatological biases

## AFTERNOON DISCUSSION
__Gaps__

- uncertainty
- innovative methods for defining extremes from satellite/event def
	- compound events (consistency across Variables)
	- Tmax products - processing satellite products tailored for extremes, order of operations
	- Max rain rates e.g. growing decaying of storms
	- Process-based analyses - casue atmospheric rivers, synoptic scale, regime dependence, convective vs large scale, raw measurements (reflectivity, heating profile etc...)
	- Timing - onset of monsoon, lifecycle of storm
	- Diurnal cycle
- effective grids s nominal grids
- temp scaling
- thermodynamic scaling
- extreme snow (any experts?)
- homogeneity of datasets for trend analysis
- practices for intercomparing


- Satellites are very good at detecting than estimating so can be much better at detecting droughts/dry spells.

# DAY 2

## HAYLEY FOWLER

- motivation for INTENSE: convection permitting models in the future
- hourly data for ~25,000 stations
	- 16,000 with > 1yr
	- 11,000 with > 10 yr
- can get indices but not raw guage data
- Barbero et al. GRL 2017

## Part3: Current problems: clouds and extr precip

P_e = -\epsilon{\omega_e\frac{dq_s}{dp} at \theta^*}

- Muller et al.

## New analysis to do for intercomparison project

- Redo ts plots with 
	1. high quality REGEN mask
	2. regions where data density is high such as America, Europe and Aus
	3. based on the topography mask
- Investigate extremes in GSMAP vs reanalyses vs guage datasets by
	1. plotting maps for a particular year (for tomorrow)
	2. plotting pdfs of grrids for a particular year and see if 
		- the entire distribution is shifted which implies a systematic bias
		- or if the tail is extended which means its because of the changes in extremes
- create maps of interproduct spread (by dividing the interproduct std dev by multimodel mean) and multimodel mean over a common period (perhaps 1998-2013) for 
	1. cwd
	2. rx1day
	3. prcptot/sdii
- do the above for all products, only reanalysis products, only satellite products and only guage products
- fig1 from Nick's JGR paper for different types of products (all, satellite, guage, reanalyses)

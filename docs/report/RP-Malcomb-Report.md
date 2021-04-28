---
layout: page
title: RP- Vulnerability modeling for sub-Saharan Africa
---


**Replication of**
# Vulnerability modeling for sub-Saharan Africa

Original study *by* Malcomb, D. W., E. A. Weaver, and A. R. Krakowka. 2014. Vulnerability modeling for sub-Saharan Africa: An operationalized approach in Malawi. *Applied Geography* 48:17–30. DOI:[10.1016/j.apgeog.2014.01.004](https://doi.org/10.1016/j.apgeog.2014.01.004)

Replication Authors:
Sanjana Roy, Joseph Holler, Kufre Udoh, Open Source GIScience students of fall 2019 and Spring 2021

Replication Materials Available at: [github repository name](github repository link)

Created: `25 April 2021`
Revised: `27 April 2021`

## Abstract

The original study is a multi-criteria analysis of vulnerability to Climate Change in Malawi, and is one of the earliest sub-national geographic models of climate change vulnerability for an African country. The study aims to be replicable, and had 40 citations in Google Scholar as of April 8, 2021.

## Original Study Information

The study region is the country of Malawi. The spatial support of input data includes DHS survey points, Traditional Authority boundaries, and raster grids of flood risk (0.833 degree resolution) and drought exposure (0.416 degree resolution).

The original study was published without data or code, but has detailed narrative description of the methodology. The methods used are feasible for undergraduate students to implement following completion of one introductory GIS course. The study states that its data is available for replication in 23 African countries.


### Data Description and Variables

Outline the data used in the study, including:

- sources of each data layer and
- the variable(s) used from each data source
- transformations applied to the variables (e.g. rescaling variables, calculating derived variables, aggregating to different geographic units, etc.)

This part may be compiled collaboratively as a group!

The study by Malcomb et al. (2014) attempts to create a multi-criteria analysis model of vulnerability in Malawi. The authors use three sources of data that investigate four different criteria or "metathemes": access, assets, physical exposure, livelihood sensitivity. Data from the Demographic Health Survey (DHS) from 2004-2010, conducted by the U.S. agency for International Development (USAID) was used to to model household dynamics and socio-economic data, including the 'access' and 'assets' criteria.

**Assets** are important coping strategies to understand vulnerability, adaptive capacity, and resilience. The following indicators were determined to increase or decrease household resilience in the context of climate-induced disasters or shocks:
1. Number of Livestock units - None/More than 95/Unknown
2. Number of Household members sick in the past 12 months - member very sick for 3+ months No/Yes/Don't Know
3. Arables land (hectares) - 95/95 or more/unknown
4. Wealth index score - Poorest/Poorer/Middle/Richer/Richest
5. Number of Orphans in a household - Number of orphans and vulnerable children

**Access** indicators covered a wide range of areas, including resources, healthcare, education, markets, insurance, infrastructure, water, and sanitation:
6. Time to water source - On premises/Don't know
7. Electricity - Has electricity Yes/No
8. Type of Cooking Fuel - Electricity, LPG/Natural gas, Natural gas, Biogas, Kerosene, Coal/lignite, charcoal, wood, straw/shrubs/grass, agricultural crop, animal dung, no food cooked, other
9. Sex of Head of Household - Male/Female
10. Own a Cell Phone - Yes/No
11. Own a Radio - Yes/No
12. House Setting - Urban/Rural


**Livelihood data** was acquired through the Famine Early Warnings Systems Network (FEWSNET) who conducted interviews in Malawi in collaboration with the Malawi Vulnerability Assessment Committee (MVAC) and the USAID. Livelihood zones were established based on households sharing similar options for obtaining food and income and divided into wealth groups of 'Poor', 'Middle', and 'Better-Off'. Using this data, four variables from livelihood zones were developed to evaluate the sensitivity of livelihoods. Data from the different Livelihood zones were extracted based on these indicators. These were then converted into percentages based on the respective formulas:
13. Percent of food poor households obtained from their own farms = crops as source of food (%)
14. Percent of income that poor households obtain from wage labour = (labor/total) * 100
15. Percent of income from cash crops - percent labor income that is susceptible to market shocks = (crops/total) * 100
16. Disaster Coping Strategy - Ecological destruction associated with livelihood coping strategies during times of crisis in each zone. Function of baseline access, possible hazards, and response strategies. Response strategies broken down into expanding existing strategies and distress strategies. Hazards broken down into periodic and chronic. = % income from practices that would lead to ecological destruction. Determining these practices was vague and there was much uncertainty in which practices were considered in the Malcomb et al. (2014) study.

For the above three metathemes (Assets, Access, and Livelihood Sensitivity), indicators were converted to a 1-5 scale to match the Malcomb et al. (2014) study. There scores were then used to calculate capacity scores based on table 2 in the paper by weighting them with percentages outlined in the model. These percentages were then converted to a 0-20 scale, by multiplying by 20, to resemble the range of the original study.

**Physical exposure** data was acquired through the United Nations Environmental Program (UNEP) Global Disaster Risk Platform, which provides easily interpretable data on the risk of flood and drought exposure, two variables used under this metatheme. UNEP's Global Resource Information Database (GRID)- Europe designed this information for the global interpretation of these two variables in terms of risk evaluation, vulnerability, and information and early-warning.
17. Estimated Risk for Flood Hazard - Modeled using global data using estimated index 1 - 5 (extreme)
18. Estimated Risk to Drought Events - based on Standardized Precipitation Index. Unit as expected average annual population exposed

Layers from UNEP were resampled and 

-- ADD WHAT DATA WAS UNCLEAR

1. How were LH added to DHS?
2. Why did we multiply by 20?
3. We're just using 2010 whereas original study did 2004 and 2010
4. What are we doing with flood and drought layers? Why do we multiply flood layers by 4?




### Analytical Specification

The original study was conducted using ArcGIS and STATA, but does not state which versions of these software were used.
The replication study will use R.

## Materials and Procedure

1. Data Preprocessing:
    - Download traditional authorities: MWI_adm2.shp
2. Adding Traditional Authorities (TA) and Livelihood Zone (LZ) ids to DHS clusters
3. Removing Household (HH) entries with invalid or unknown values
4. Aggregating HH data to DHA clusters, and then joining to traditional authorities to get: ta_capacity_2010
5. Removing index and livestock values that were NA
6. Sum of Livestock by HH
7. Scale adaptive capacity fields (from DHS data) on scale of 1 - 5 to match Malcomb et al.
8. Weight capacity based on table 2 in Malcomb et al.
    - Calculate capacity by summing all weighted capacity fields
9. Summarize capacity from households to traditional authorities
10. Joining mean capacities to TA polygon layer
11. Making capacity score resemble Malcomb et al s work (scores on range of 0-20) by multiplying capacity score by 20
12. Categorizing capacities using natural jenks methods
13. Creating blank raster and setting extent of Malawi - CRS: 4326
14. Reproject, clip and resampling flood risk and drought exposure rasters to new extent and cell size
    - Uses bilinear resampling for drought to average continuous population exposure values
    - Uses nearest neighbor resampling for flood risk to preserve integer values
    - Removing factors from flood layer and recasting them as integers
    - Clipping TAs with LZs to remove lake
    - Rasterizing final TA capacity layer
15. Masking flood and drought layers
16. Reclassify drought raster into quantiles
17. Add all RASTERs together to calculate final output:  final = (40 - geo) * 0.40 + drought * 0.20 + flood * 0.20 + LHZ * 0.20
18. Using zonal statistics to aggregate raster to TA geometry for final calculation of vulnerability in each traditional authority

## Replication Results

For each output from the original study (mainly figure 4 and figure 5), present separately the results of the replication attempt.

2.	State whether the original study was or was not supported by the replication
3.	State whether any hypothesis linked to a planned deviation from the original study was supported. Provide key statistics and related reasoning.

Figures to Include:
- map of resilience by traditional authority in 2010, analagous to figure 4 of the original study
- map of vulnerability in Malawi, analagous to figure 5 of the original study
- map of difference between your figure 4 and the original figure 4
- map of difference between your figure 5 and the original figure 5

## Unplanned Deviations from the Protocol

Summarize changes and uncertainties between
- your interpretation and plan for the workflow based on reading the paper
- your final workflow after accessing the data and code and completing the code

Replication and reproduction of scientific methods required a sound description of methodology. From what we understood in the Malcomb et al. (2014) study


1. Rescaling from 0-5 to 1-5
2. Rescale to fit the Malcomb et al. (2014) * 20
3. Cleaning the data and preprocessing
4. Adding in the Livelihood zone data - arbitrary decisions made about the ecological damage - had to decide what data went into this as it was not outlined in Malcomb -


## Discussion

Provide a summary and interpretation of the key findings of the replication *vis-a-vis* the original study results. If the attempt was a failure, discuss possible causes of the failure. In this replication, any failure is probably due to practical causes, which may include:
- lack of data
- lack of code
- lack of details in the original analysis
- uncertainties due to manner in which data has been used








## Conclusion

Restate the key findings and discuss their broader societal implications or contributions to theory.
Do the research findings suggest a need for any future research?

## References

Include any referenced studies or materials in the [AAG Style of author-date referencing](https://www.tandf.co.uk//journals/authors/style/reference/tf_USChicagoB.pdf).

####  Report Template References & License

This template was developed by Peter Kedron and Joseph Holler with funding support from HEGS-2049837. This template is an adaptation of the ReScience Article Template Developed by N.P Rougier, released under a GPL version 3 license and available here: https://github.com/ReScience/template. Copyright © Nicolas Rougier and coauthors. It also draws inspiration from the pre-registration protocol of the Open Science Framework and the replication studies of Camerer et al. (2016, 2018). See https://osf.io/pfdyw/ and https://osf.io/bzm54/

Camerer, C. F., A. Dreber, E. Forsell, T.-H. Ho, J. Huber, M. Johannesson, M. Kirchler, J. Almenberg, A. Altmejd, T. Chan, E. Heikensten, F. Holzmeister, T. Imai, S. Isaksson, G. Nave, T. Pfeiffer, M. Razen, and H. Wu. 2016. Evaluating replicability of laboratory experiments in economics. Science 351 (6280):1433–1436. https://www.sciencemag.org/lookup/doi/10.1126/science.aaf0918.

Camerer, C. F., A. Dreber, F. Holzmeister, T.-H. Ho, J. Huber, M. Johannesson, M. Kirchler, G. Nave, B. A. Nosek, T. Pfeiffer, A. Altmejd, N. Buttrick, T. Chan, Y. Chen, E. Forsell, A. Gampa, E. Heikensten, L. Hummer, T. Imai, S. Isaksson, D. Manfredi, J. Rose, E.-J. Wagenmakers, and H. Wu. 2018. Evaluating the replicability of social science experiments in Nature and Science between 2010 and 2015. Nature Human Behaviour 2 (9):637–644. http://www.nature.com/articles/s41562-018-0399-z.

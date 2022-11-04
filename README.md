# Investigating invasive *Neogobius melanostomus* (Round Goby) and its Impact on Native Fishes in Lake Ontario

## Abstract/At a Glance
The Neogobius melanostomus, or the round goby, is a bottom-dwelling, small, saltwater species native to the Caspian Sea in Eastern Europe. A very aggressive and 
prolific breeder, this alien species is thought to have lead to the decline of native species that occupy a similar niche[^1]. Due to the changing climate of the Great Lakes, as the temperatures of the Great Lakes begin to match those of the Caspian, we want to see/predict if there will be any changes to species abundance within the Great Lakes.

This project is based upon a extended study conducted in various site in Lake Ontario since 1978, where species were collected from the bed, measured, and tallied[^2]. We aim to compare several variables. To study the abundances of native species and the round goby, variables of importance include the abundance of the Round Goby (an invasive species in the great lakes region), the water temperature, and the abundances of native fish species (slimy sculpin, three-spine stickleback, etc) that occupy a similar benthic niche. We want to find out if there is any correlation between changing temperature and the abundance of native species(slimy sculpin, three-spine stickleback, etc). Furthermore, we hope to determine if the presence of the round goby is influencing the native species.  Besides abundance, we want to see if the introduction of the Round Goby has exhibted any morphological and/or behavioural changes in native species; to accomplish this, we  will measure native fish body traits such as body length, average mass, life stage and fishing depth prior to the introduction of the round goby and after the introduction and increasing abundance of the round goby. We hope this will help understand the threats faced by native species and whether they are more threatened by the round goby or by the temperature, or both. The data was collected from various points in Lake Ontario, and we hope to also find any differences between sites.

## Null Hypothesis

1.	Temperature does not have a different effect on the abundance of native species (sculpin, logperch, etc.) compared to the round goby because both native species and the round goby are equally affected by temperature changes.
2.	The abundance of the round goby has no effect on the abundance of native species 
3. The prescence of the round goby has not resulted in any significant morphological and behavioural changes in native fish 

## Alternative Hypothesis 

1.	Temperature has a different effect on the abundance of native species (lake trout, smallmouth bass, etc.) compared to the round goby because the round goby and native species are not equally affected by temperature changes. 
2.	The abundance of the round goby negatively influences the abundance of native species because it outcompetes native species.
3. The prescence of the round goby has resulted in significant morphological and behavioural changes in native fish. 

## Predictions

1.	If the temperature increases then the round goby will be positively affected, and the native species will be negatively affected. 
a.	The round goby’s native range is warmer than the average temperature of the great lakes region [^3]. Thus, we suspect that as temperature increases the round goby will feed more aggressively on other abundant species like zebra mussels or small invertebrates. As the round goby breeds continuously throughout the year, increased temperatures may extend their spawning season and the number of spawning per year, which would rapidly increase their population [^3].
2.	If the round goby abundance increases, then the native species abundance will decrease.
a.	When the round goby populations increase, they prey on the same species as native fishes and can outcompete them for food [^4]. They also consume the eggs and young of native species which could cause a decrease in their populations [^4].
3. The prescence of the round goby results in captured native fish being larger and in deeper sections of the lake, as smaller native fish in shallower are directly preyed on by the Round Goby 

## Description of datasets and Data Exploration 
 (TrawlCatch_SpringPreyfishBottomTrawl.csv) Lake Ontario April Prey Fish Bottom Trawl Survey, 1978-2022
TrawlCatch_SpringPreyFishBottomTrawl is a part of a larger study on the Great Lakes Region for conservation and fisheries. Within this dataset, it contains specimens from research vessels from the US Geological Survey and the Ontario Department of Natural Resources and Fisheries. The speciments were collected from 1978-2022 in various sites across Lake Ontario using trawl fishing(bottom fishing) during April of each year. The Round Goby is a benthic fish so we expect trawling to provide an accurate representation of their relative abundance. Columns of importance include water temperature, species name, latitude, longitude, life stage, n(number of this species collected), mass(individual), and average mass. Using this dataset, relative population densities of various species can be determined. We will use this to assess the relative abundance of the round goby in different locations as well as the native species and we will see how they change with temperature. We will also consider the size of the fishes measured, and their life stage. 
[https://www.sciencebase.gov/catalog/item/62f4fd46d34eacf53973a841](https://www.sciencebase.gov/catalog/item/62f4fd46d34eacf53973a841) 

To get our data ready for general analysis(for studying both abundances and morphology), we first removed columns from the dataset that were not applicable to our analysis, such as towing time, speed of the research vessel, etc. Furthermore, we mutated the saved dataset to label the species caught as either native or non-native("exotic"). This makes it easier for us to group the species for analysis. We then tallied up the total number of individuals by species; keeping the species with more than 200 observations. Species with less than 200 indivdiduals studied probably indicates that said species did not occupy a niche within the benthic layer of water, and thus would have less interaction with the round goby. As the dataset covers a large amount of data over a extremely long period of time(40+ years of data), species with less than 200 observations would potentially skew the analysis, as there are not enough individuals to get a more accurate representation of that species. After filtering out our desired species that met this requirement ("Slimy Sculpin," "deepwater sculpin," "yellow perch," "trout-perch," "johnny darter," "lake trout," "round goby," "threespine stickleback"), we are now able to perform our desired analysis on the data. 

To see a detailed breakdown of our code and plots that we created, go to the folder "data exploration," which contains our code. [GroupC/DataExploration/](https://github.com/EEB313-2022/GroupC/tree/main/DataExploration)

## Analysis 

There are several analyses we plan to complete.

1. In order to reject/accept the hypothesis about temperature and its influence of species abundance, we will perform linear regressions/linear models(using the "lm" function), between the temperatures of different sites and the abundances of each species(Native species and the Round Goby). In this case, the dependent variable would be species abundance(native or round goby), and the independent variable would be temperature. This model will display if temperature has an effect on abundance. To validate the assumptions on our linear model, we need check for homogenity of variances at each X and normality. By using the plot() function and analysising the Residuals vs fitted plot and the Q-Q plot, we can determine if the model assumptions are met, in order to determine if our linear model is of any significance. 

2. To determine the interaction between the Round Goby on the abundance of native species, but accounting for other variables such as site(location) and temperature, we will use a mixed effects model to see if we can discover what factors are most correlated with the abundances of native species, and whether round goby abundance has any effect. Prior to setting up our model, we would need to test if the site/location of capture is nested(not independent) or not; we can test for this by using xtabs. If non-independent, when we measure native species abundance, we can treat goby abundance as an fixed effect, and treat the location/site as a random effect. We would also like to know if different sites at which the data was collected have an effect on the abundances of the fish species of interest (e.g. if the data is spatially autocorrelated). This should help us see if the round goby is having an effect on native species and/or vice versa. 

3. To determine whether the prescence of the round goby has impacted the morphology and/or behaviour of native species, we will conduct a PCA on fish length, mass, and fishing depth, see whether these traits are negatively correlated with an increase in goby population. Before conducting the PCA analysis, we will use the “pairs” function to determine any relationships between the variables(body length, mass, depth of water caught at). To determine which PC axises would be of use(and to help us accept/reject the null hypothesis), we would then plot a scree plot to determine which axises are meaningful. In all, conducting a PCA analysis will tell us whether or not the prescence of the round goby has affected native species in anyway.     



## Sources and Citations:
[^1]: Corkum, Lynda D., et al. “The Round Goby, Neogobius Melanostomus, a Fish Invader on Both Sides of the Atlantic Ocean.” Biological Invasions, vol. 6, no. 2, 2004, pp. 173–81.  https://doi.org/10.1023/B:BINV.0000022136.43502.db.
[^2]: Weidel, B.C., Holden, J.P., Goretzke, J., Connerton, M., and Gutowsky, L., 2022, Lake Ontario April Prey Fish Bottom Trawl Survey, 1978-2022: U.S. Geological Survey data release, https://doi.org/10.5066/P97DZ1AS.
[^3]: Reid, H. B., & Ricciardi, A., 2022, Ecological responses to elevated water temperatures across invasive populations of the round goby ( Neogobius melanostomus ) in the Great Lakes basin. Canadian Journal of Fisheries and Aquatic Sciences, 79(2), 277–288. https://doi.org/10.1139/cjfas-2021-0141
[^4]: Yves, P, 2018, Aquatic invasive species in the freshwater section of the St. Lawrence River. Gouvernement du Québec,.


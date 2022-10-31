# Investigating invasive *Neogobius melanostomus* (Round Goby) and its Impact on Native Fishes in Lake Ontario

## Abstract/At a Glance
The Neogobius melanostomus, or the round goby, is a bottom-dwelling, small, saltwater species native to the Caspian Sea in Eastern Europe. A very aggressive and 
prolific breeder, this alien species is thought to have lead to the decline of native species that occupy a similar niche[^1]. Due to the changing climate of the Great Lakes, as the temperatures of the Great Lakes begin to match those of the Caspian, we want to see/predict if there will be any changes to species abundance within the Great Lakes.

This project is based upon a extended study conducted in various site in Lake Ontario since 1978, where species were collected from the bed, measured, and tallied[^2]. We aim to compare several variables. The main 3 are the abundance of the Round Goby (an invasive species in the great lakes region), the water temperature, and the abundance of native fish species (sculpin, and logperch but we’d love to look at more if we have time) that occupy a similar niche. We want to find out if there is any correlation between changing temperature and the abundance of these 3 species. Furthermore, we hope to determine if the presence of the round goby is influencing the native species. We hope this will help understand the threats faced by native species and whether they are more threatened by the round goby or by the temperature, or both. The data was collected from various points in Lake Ontario, and we hope to also find any differences between sites.



## Null Hypothesis

1.	Temperature does not have a different effect on the abundance of native species (sculpin, logperch, etc.) compared to the round goby because both native species and the round goby are equally affected by temperature changes.
2.	The abundance of the round goby no effect on the abundance of native species because it does not outcompete native species.


## Alternative Hypothesis 

1.	Temperature has a different effect on the abundance of native species (lake trout, smallmouth bass, etc.) compared to the round goby because the round goby and native species are not equally affected by temperature changes. 
2.	The abundance of the round goby negatively influences the abundance of native species because it outcompetes native species.


## Predictions

1.	If the temperature increases then the round goby will be positively affected, and the native species will be negatively affected. 
a.	The round goby’s native range is warmer than the average temperature of the great lakes region [^3]. Thus, we suspect that as temperature increases the round goby will feed more aggressively on other abundant species like zebra mussels or small invertebrates. Also, because the round goby breeds continuously throughout the year, increased temperatures may increase the number of spawning per year which would rapidly increase their population [^3].
2.	If the round goby abundance increases, then the native species abundance will decrease.
a.	When the round goby populations increase, they prey on the same species as native fishes and can outcompete them for food [^4]. They also consume the eggs and young of native species which could cause a decrease in their populations [^4].

## Description of datasets
 (TrawlCatch_SpringPreyfishBottomTrawl.csv) Lake Ontario April Prey Fish Bottom Trawl Survey, 1997-2022
TrawlCatch_SpringPreyFishBottomTrawl contains the specimens collected from 1997-2022 from Lake Ontario using trawl fishing. Round Goby is a benthic fish so we expect trawling to provide an accurate representation of their relative abundance. Columns of importance include water temperature, species name, latitude, longitude, life stage, n(number of species collected), and weight. Using this dataset, relative population densities of various species can be determined. We will use this to assess the relative abundance of the round goby in different locations as well as the native species and we will see how they change with temperature. If we have extra time, we may consider the size of the fishes measured, and their life stage. 
Link:  https://www.sciencebase.gov/catalog/item/62f4fd46d34eacf53973a841 



## Data Exploration 

*Note: Don't forget to set your own working directory*
```{r Data Cleaning}
fish.data.raw <- read.csv("TrawlCatch_SpringPreyfishBottomTrawl.csv")
head(fish.data.raw)
fish.data.raw <- fish.data.raw %>% 
  mutate(year=as.factor(year)) #didn't want year to be a continuous variable, I wanted them as discrete values.
fish.data <- fish.data.raw %>% 
  filter(!is.na(fishingTemperature_C), !is.na(latitude), !is.na(longitude), species==c("Lake trout", ""))#I am removing any datapoints where the temperature, latitude, or longitude is not collected because they will not be useful for the questions we are asking unless we have those data
```
```{r Temperature by date}
#This is to set up a graph for the temperature values collected at each date 
unique(fish.data$year) #Checking the years to make sure we have the right dataset - we have data from 1997 to 2022
#M: the dates should be converted into a more readable format. I just don't know how to do that so I need to ask for help.
ggplot(fish.data, aes(x=opDate, y=fishingTemperature_C)) + geom_point(alpha=0.1) + geom_smooth() + labs(title="Temperature values by date", x="Date (YYYYMMDD)", y="Temperature (Degrees C)") + theme(plot.title = element_text(hjust = 0.5)) #I did smooth just to see it, but it's not a very meaningful line 
```

## Analysis 

There are several analyses we plan to complete.

1. First, using linear regressions (M: is this the right term for it?) we will attempt to see if there is any relationship between the temperatures of different sites and the abundances of each species. 
2. Then, we would like to use mixed effects models (M: correct term?) to see if we can discover what factors are most correlated with the abundances of each individual species. The biological relevance of each variable will be considered. Currently we plan to look at location, temperature, and abundance of each of native/invasive species. For example, we might test a model of round goby abundance with the variables temperature, site, and presence of our native species of interest (listed above) and then we would also test a model of the abundance of each native species using the same variables except with the presence of the round goby instead. We would also like to know if different sites at which the data was collected have an effect on the abundances of the fish species of interest (e.g. if the data is spatially autocorrelated). This should help us see if the round goby is having an effect on native species and/or vice versa. 
3. We are cautious of the amount of time we have to complete this project. If we have time we would find it interesting to do a PCA using the biologically relevant variables in the dataset (M: what are these variables? what might we test in a PCA?). We would also like to create some maps of the locations examined in this data so that we can present our data in our final project in a visually interesting and clear way. 


## Sources and Citations:
[^1]: https://link.springer.com/content/pdf/10.1023/B:BINV.0000022136.43502.db.pdf
[^2]: Weidel, B.C., Holden, J.P., Goretzke, J., Connerton, M., and Gutowsky, L., 2022, Lake Ontario April Prey Fish Bottom Trawl Survey, 1978-2022: U.S. Geological Survey data release, https://doi.org/10.5066/P97DZ1AS.
[^3]: Reid, H. B., & Ricciardi, A., 2022, Ecological responses to elevated water temperatures across invasive populations of the round goby ( Neogobius melanostomus ) in the Great Lakes basin. Canadian Journal of Fisheries and Aquatic Sciences, 79(2), 277–288. https://doi.org/10.1139/cjfas-2021-0141
[^4]: Yves, P, 2018, Aquatic invasive species in the freshwater section of the St. Lawrence River. Gouvernement du Québec,.


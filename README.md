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

###Setup
```{r Setup}
library(ggplot2)
library(dplyr)
library(ggfortify)
library(tidyverse)
```
```{r Cleaning Data}
fish.data.raw <- read.csv("TrawlCatch_SpringPreyfishBottomTrawl.csv")
head(fish.data.raw)
fish.data.raw <- fish.data.raw %>% 
  mutate(year=as.factor(year)) #didn't want year to be a continuous variable, I wanted them as discrete values not continuous.
fish.data <- fish.data.raw %>% 
  filter(!is.na(fishingTemperature_C), !is.na(latitude), !is.na(longitude), commonName!= "No fish caught", commonName!= "Miscellaneous or unidentified species", commonName!=  "Unidentified coregonid", commonName!= "Unidentified minnows",  commonName!= "Uninidentified redhorse")#I am removing any datapoints where the temperature, latitude, or longitude is not collected because they will not be useful for the questions we are asking unless we have those data
#I am also removing unidentified or misc. fishes
```
###Choosing focal species
```{r Choosing Native Species}
fish.data %>% 
  group_by(commonName) %>% 
  tally() %>% 
  arrange(desc(n)) #this helped us see how many observations we had per species 
fish.list <- as.data.frame(unique(fish.data$commonName)) #this just made a data frame of the list so we could check if each one was exotic or native
fish.data.exonat <- fish.data %>% #Here we made a new column that marks each fish species as exotic or native. This was based on a few papers listed in our methods. We did not bother doing this for any fishes where there were less than 200 observations because they would not be used for this analysis anyway. We also did not do this for non-fish species as they are not the focus of this analysis. 
  mutate(inv.status = case_when(
    endsWith(commonName, "Alewife") ~ "exotic",
    endsWith(commonName, "Sea lamprey") ~ "exotic",
    endsWith(commonName, "Chinook salmon") ~ "exotic",
    endsWith(commonName, "Rainbow trout (Steelhead)") ~ "exotic",
    endsWith(commonName, "Carp") ~ "exotic",
    endsWith(commonName, "Brown trout") ~ "exotic",
    endsWith(commonName, "Rainbow smelt") ~ "exotic",
    endsWith(commonName, "Coho salmon") ~ "exotic",
    endsWith(commonName, "White perch") ~ "exotic",
    endsWith(commonName, "Blueback herring") ~ "exotic", 
    endsWith(commonName, "Chain pickerel") ~ "exotic",
    endsWith(commonName, "Round goby") ~ "exotic",
    endsWith(commonName, "Tubenose goby") ~ "exotic",
    endsWith(commonName, "Threespine stickleback") ~ "native",
    endsWith(commonName, "Emerald shiner") ~ "native",
    endsWith(commonName, "Lake whitefish") ~ "native",
    endsWith(commonName, "Deepwater sculpin") ~ "native",
    endsWith(commonName, "Lake trout") ~ "native",
    endsWith(commonName, "Burbot") ~ "native",
    endsWith(commonName, "Slimy sculpin") ~ "native",
    endsWith(commonName, "Emerald shiner") ~ "native",
    endsWith(commonName, "Cisco (lake herring)") ~ "native",
    endsWith(commonName, "Whitefishes") ~ "native",
    endsWith(commonName, "Johnny darter") ~ "native",  
    endsWith(commonName, "Trout-perch") ~ "native", 
    endsWith(commonName, "Yellow perch") ~ "native", 
    endsWith(commonName, "Spottail shiner") ~ "native"
    ))
fish.data.exonat %>% 
  filter(is.na(inv.status)) %>% 
  group_by(commonName) %>% 
  tally() %>% 
  arrange(desc(n))
#Checking to see which ones I hadn't researched yet to make sure I did not miss any important ones.
#Dreissena are mussels and we are only focused on fishes so we will be cutting those out anyway
#We ignore everything below 200 observations on this list because they do not have enough observations to be included in our data
fish.data.exonat %>%  #now that we have labeled each species, we can display our native species of interest. 
  filter(inv.status=="native") %>% 
  group_by(commonName) %>% 
  tally() %>% 
  arrange(desc(n))
#based on this, we can choose only species with more than 300 observations. In this case that means Yellow perch, Threespine stickleback, Deepwater sculpin, Trout-perch, Johnny darter, Lake trout, and Slimy sculpin.
```

Our data for which species were exotic came from here: https://www-sciencedirect-com.myaccess.library.utoronto.ca/science/article/pii/S0380133019301637
http://www.glfc.org/pubs/TechReports/Tr67.pdf 
https://librarysearch.library.utoronto.ca/permalink/01UTORONTO_INST/fedca1/cdi_gale_infotracacademiconefile_A484511028 

###Creating cleaned data set
```{r Cleaning data 2}
fish.data.clean <- fish.data.exonat %>% #this is now the data we are interested in, including only the species that we are able to look at.
  filter(commonName=="Yellow perch" | commonName=="Threespine stickleback" | commonName=="Deepwater sculpin" | commonName=="Trout-perch" | commonName=="Johnny darter" | commonName=="Lake trout" | commonName=="Slimy sculpin" | commonName=="Round goby")
```

###Cleaning: Change the format of date column to YYYY-MM-DD
```{r}
fish.data.clean2 <- fish.data.clean
```

```{r Cleaning data 3}
library(dplyr)
fish.data.clean <- fish.data.clean2 %>%
  group_by(opDate) %>%
  mutate(opDate = paste0(substr(opDate,1,4), "-", substr(opDate,5,6), "-", substr(opDate,7,8))) %>% 
  as.data.frame(mutate(opDate = as.character(opDate)))

```

#Plotting temperature
```{r Temperature Plot}
unique(fish.data$year) #we have data from 1997 to 2022
#the dates should be converted into a more readable format. I just don't know how to do that so I need to ask for help.

ggplot(fish.data, aes(x=opDate, y=fishingTemperature_C)) + geom_point(alpha=0.1) + geom_smooth() + labs(title="Temperature values by date", x="Date (YYYYMMDD)", y="Temperature (Degrees C)") + theme(plot.title = element_text(hjust = 0.5)) 
```

![](DataExploration/Plots/Temperature%20Values%20by%20date.png)

This graph just shows temperature changes over time. We don't expect them to vary much, but its good to take a look regardless to see if there is variation in temperature at all. There does seem to be variation which will be important for our analyses. Also, I used the clean data because we needed to confirm that there was variation in the data we will actually use, not just in the original data. If, for example, all the temperatures were the same, we likely wouldn't be able to make any conclusions about the temperature ranges of these fishes. 
```{r Checking temperature effects visually}
ggplot(fish.data.clean, aes(x=fishingTemperature_C, fill=inv.status)) + geom_histogram(bins=15) + facet_wrap(~commonName) + labs(title="Observed temperature by species", x="Temperature (Degrees C)", y="Count of individuals observed") + theme(plot.title = element_text(hjust = 0.5)) + scale_fill_discrete(name = "Status")
#plotting the count of observations of each species depending on the temperature.
```
![](DataExploration/Plots/DataExploration/Plots/Observed%20Temperature%20by%20species.png)

Just based on the visual that this graph provides it looks like there's a variety of temperature tolerances among our focal species. It seems like the round goby does not exist at much higher temperatures than the other species. We will still have to run our analysis to confirm this, but there appears to be little difference in temperature tolerance of the round goby in comparison to the other native species. Interestingly, some of these species (like the yellow perch, trout perch, and lake trout) have an even wider temperature range than the round goby. Others, like the slimy sculpin, have a narrower range and seem to be mostly found at one location. It should be noted, as well, that these data are counts so it simply be that the slimy sculpin is just more commonly observed which is why it would have such a narrow peak.

#Plotting proportion of the catch by Species 
```{r plotting proportion of catch by species }
# Proportion of the total catch from the first occurance of the Round Goby in 1997 
fish.data.clean %>% 
  group_by(commonName, year) %>% 
  # filtering out year based on first time a goby was sighted --> in 1997
  filter(year %in% seq(1997, 2022)) %>% 
  tally(n) %>%  # tallying up occurances of each species
  ggplot(aes(x=year, y=n, fill=commonName)) + geom_bar(position="fill", stat="identity") + labs(title="Proportion of Catch By Species", x="Year of Study", y="Proportion of catch") + theme(axis.text.x = element_text(angle=90, hjust=1)) 
```
![](DataExploration/Plots/Proportion%20of%20Catch%20By%20Species.png)
By plotting the proportion of the yearly catch by species, we can see some pretty intesting details. At the very beginning of the introduction of the Round Goby, 
we can see that the majority of the catch was either the Threespine Stickleback or the Slimy Sculpin; both benthic species that occupy a similar niche to the Round goby. As the study progressed, the Round Goby became a greater and greater proporition of the catch, while catches of native fish including the Threespine Stickleback and the Slimy Sculpin were observed less and less. 

## Analysis 

There are several analyses we plan to complete.

1. First, using linear regressions (M: is this the right term for it?) we will attempt to see if there is any relationship between the temperatures of different sites and the abundances of each species. 
2. Then, we would like to use mixed effects models (M: correct term?) to see if we can discover what factors are most correlated with the abundances of each individual species. The biological relevance of each variable will be considered. Currently we plan to look at location, temperature, and abundance of each of native/invasive species. For example, we might test a model of round goby abundance with the variables temperature, site, and presence of our native species of interest (listed above) and then we would also test a model of the abundance of each native species using the same variables except with the presence of the round goby instead. We would also like to know if different sites at which the data was collected have an effect on the abundances of the fish species of interest (e.g. if the data is spatially autocorrelated). This should help us see if the round goby is having an effect on native species and/or vice versa. 
3. We are cautious of the amount of time we have to complete this project. If we have time we would find it interesting to do a PCA using the biologically relevant variables in the dataset (M: what are these variables? what might we test in a PCA?). We would also like to create some maps of the locations examined in this data so that we can present our data in our final project in a visually interesting and clear way. 


## Sources and Citations:
[^1]: Corkum, Lynda D., et al. “The Round Goby, Neogobius Melanostomus, a Fish Invader on Both Sides of the Atlantic Ocean.” Biological Invasions, vol. 6, no. 2, 2004, pp. 173–81.  https://doi.org/10.1023/B:BINV.0000022136.43502.db.
[^2]: Weidel, B.C., Holden, J.P., Goretzke, J., Connerton, M., and Gutowsky, L., 2022, Lake Ontario April Prey Fish Bottom Trawl Survey, 1978-2022: U.S. Geological Survey data release, https://doi.org/10.5066/P97DZ1AS.
[^3]: Reid, H. B., & Ricciardi, A., 2022, Ecological responses to elevated water temperatures across invasive populations of the round goby ( Neogobius melanostomus ) in the Great Lakes basin. Canadian Journal of Fisheries and Aquatic Sciences, 79(2), 277–288. https://doi.org/10.1139/cjfas-2021-0141
[^4]: Yves, P, 2018, Aquatic invasive species in the freshwater section of the St. Lawrence River. Gouvernement du Québec,.


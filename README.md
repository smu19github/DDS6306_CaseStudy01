## DDS6306_CaseStudy01
Git repository project entitled "Case Study 1." Members of this project include Heindel Adu, Gabriel Gonzales, and Amy Markum. 

## Introduction: This presentation provides an overview of exactly what type of manipulations were performed on two combined data sets (Beer by State and Breweries by State). Client has requested answers to the five following questions:

  ### Number of Breweries per state
  ### Number of NA values returned for each data set
  ### Median ABV and IBU counts for each state, and provide a comparative bar chart
  ### States with maximum ABV and IBU, respectively
  ### Determination of any relationship between ABV and IBU, and provide comparative scatter plot

## First, it is necessary to merge the tables using a specific code in r
  ### Brew_Beer_Table <- right_join(Breweries, Beers, by="Brew_ID")

## Second, it's a good idea to save your file before manipulating the data, in case you need to return to the original data
  ### write.csv(Brew_Beer_Table, file = "Breweries_Beer.csv")

## Third, you should rename all columns to be easily human readable
  ### names(Brew_Beer_Table) <- c("Brewery_ID", "Brewery_Name", "City", "State", "Beer_Name", "Beer_ID", "ABV", "IBU", "Beer_Style", "Ounces")

## Any data should be sorted to assist in understanding the data set, in this case sorted first by Brewery ID, then by ABV (ascending)
  ### attach(Brew_Beer_Table)
  ### SortedBreweryTable <- Brew_Beer_Table[order(Brewery_ID, ABV), ]
  ### write.csv(SortedBreweryTable, file = "Breweries_Beer.csv")

## It's always a good idea to review your dataset to ensure everything is as it should be
  ### head(SortedBreweryTable, 10)
  ### tail(SortedBreweryTable, 10)

## Next we begin to answer the client's questions: Identify all of the NA's: (62 in the ABV column, 1005 in IBU)
  ### sapply(SortedBreweryTable, function(x) sum(is.na(x)))

## Then identify how many breweries are in each state
  ### StateBreweryTotals <- SortedBreweryTable %>% group_by(State) %>% summarize(count=n())

## Any data that is separated out and may be needed for later mainpulation should be stored in its own file
  ###write.csv(StateBreweryTotals, file = "Breweries-State.csv")

## We provide a barplot indicating the number of breweries per state
  ### ggplot(StateBreweryTotals, aes(x=State, y=count)) + geom_bar(stat="identity")


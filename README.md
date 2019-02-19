## DDS6306_CaseStudy01
Git repository for the case study projects. Members of this project include Amy, Gabriel and Heindel.

I did a bit of work today. Here's the reproducible code for what I've done:

### joining both tables into one, joined under the Brew_ID
Brew_Beer_Table <- right_join(Breweries, Beers, by="Brew_ID")

### write file to save before working on the data
write.csv(Brew_Beer_Table, file = "Breweries_Beer.csv")

### rename columns to be easily human readable
names(Brew_Beer_Table) <- c("Brewery_ID", "Brewery_Name", "City", "State", "Beer_Name", "Beer_ID", "ABV", "IBU", "Beer_Style", "Ounces")

### begin sorting, in this case sorted first by Brewery ID, then by ABV (ascending)
attach(Brew_Beer_Table)
SortedBreweryTable <- Brew_Beer_Table[order(Brewery_ID, ABV), ]
write.csv(SortedBreweryTable, file = "Breweries_Beer.csv")

### take a look at the file
head(SortedBreweryTable, 10)
tail(SortedBreweryTable, 10)

### identify all of the NA's in each column (62 in the ABV column, 1005 in IBU)
sapply(SortedBreweryTable, function(x) sum(is.na(x)))

### identify how many breweries are in each state
StateBreweryTotals <- SortedBreweryTable %>% group_by(State) %>% summarize(count=n())

### and write that to its own file
write.csv(StateBreweryTotals, file = "Breweries-State.csv")

### plot your breweries per state
ggplot(StateBreweryTotals, aes(x=State, y=count)) + geom_bar(stat="identity")


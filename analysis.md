---
title: "Palmer Archipelago (Antarctica) Penguin Data Analysis"
author: "A N M Jubaer"
date: '2022-06-03'
output:
  word_document: default
  html_document: default
  pdf_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE)
```


![](https://allisonhorst.github.io/palmerpenguins/logo.png)

Palmer Archipelago (Antarctica) Penguin is a dataset of ***Allison Horst, Alison Hill and Kristen Gorman***. Follow this [link](https://allisonhorst.github.io/palmerpenguins/) to navigate to dataset's github repo.



## Loading palmerpenguin and required packages

```{r loading_packages}
if(!require("tidyverse")) install.packages("tidyverse")
if(!require("palmerpenguins")) install.packages("palmerpenguins")
if(!require("ggplot2")) install.packages("ggplot2")
library("ggplot2")
library("tidyverse")
library("palmerpenguins")
data("penguins")
```
## Let's view the dataset

```{r viewing_data001}
head(penguins)
```

head() function gives the first 6 rows of data from the datasetas as shown in the up.

colnames() function gives list of all columns in dataset.
```{r viewing_data002}
colnames(penguins)
```

str() and glimpse() lets see the more details about columns from dataset

```{r viewing_data003}
str(penguins)
glimpse(penguins)
```

There are 8 columns and 344 rows in the penguin dataset.

```{r viewing_data004}
unique(penguins$species)
unique(penguins$island)
```

The penguins are of 3 species from 3 different islands. We can see the count of penguins of different species and over the islands present in the dataset below:

```{r}
table(penguins$species)
table(penguins$island)

```


## Analysing the dataset from differnt angle

Plot for different species' penguins:

```{r analysis001}
ggplot(data = penguins, aes(x = species, fill = species)) + geom_bar()
```

Plot for number of penguins at different islands:


```{r analysis002}
ggplot(data = penguins, aes(x = island, fill = island)) + geom_bar()

```

Plot for number of different penguins species at different islands:

```{r}
ggplot(data = penguins, aes(x = species, fill = island)) + geom_bar()
```


Plot for Flipper length (mm) vs Body mass (g):

```{r analysis003}
labs001 <- labs(title = "Flipper length (mm) vs Body mass (g)", x = "Flipper length (mm)", y = "Body mass (g)")
ggplot(data = penguins, aes(x = flipper_length_mm, y = body_mass_g, color = species, shape = species)) +
  geom_point() + labs001 +
  annotate("text", x = 220, y = 3500, label = "Gentoos are the largest", angle = 40, fontface = "bold", size = 4.5, color = "#619CFF")
```


Plot for common trend in Flipper length (mm) vs Body mass (g):

```{r}
ggplot(data = penguins, aes(x = flipper_length_mm, y = body_mass_g)) + geom_smooth(method = "loess", color = "yellow") + geom_point(aes(color = species)) + labs001
```


Plot for common trend in Flipper length (mm) vs Body mass (g) over different species:

```{r}
ggplot(data = penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_point(aes(color = species, shape = species)) + geom_smooth(method = "loess", color = "yellow") +
  facet_wrap(~species)
```

Plot for Species vs Sex for Flipper length (mm) vs Body mass (g):

```{r}
ggplot(data = penguins, aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_jitter(aes(color = species)) +
  facet_grid(species~sex) + labs(title = "Penguins: Flipper length vs Body mass", subtitle = "Species vs Sex", caption = "Data collected by Dr. Kristen Gorman") +
  theme(axis.text.x = element_text(angle = 90))
```


Plot for Bill Length, Bill depth, Flipper length, Body mass based on sex of different species:

First extracting the required data from data frame:

```{r}
df <- penguins %>% 
  group_by(sex, species) %>% 
  drop_na() %>% 
  summarise(mean_bill_length = mean(bill_length_mm), mean_bill_depth = mean(bill_depth_mm), mean_flipper_length = mean(flipper_length_mm), mean_body_mass = mean(body_mass_g))
df
```

Then, converting the wide format to long format

```{r}
df_long <- pivot_longer(df, -species & -sex, names_to="variables", values_to="values")
df_long

```

After that, plotting the long data:

```{r}
ggplot(df_long,aes(x = variables,y = values)) + 
  geom_bar(aes(fill = variables),stat = "identity",position = "dodge") + 
  scale_y_log10() + facet_grid(sex~species) +
  theme(axis.text.x = element_text(angle = 90))


```

# Thank You




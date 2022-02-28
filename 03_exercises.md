---
title: 'Weekly Exercises #3'
author: "Audrey Smyczek"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday dog breed data
breed_traits <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_traits.csv')
trait_description <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/trait_description.csv')
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_rank.csv')

# Tidy Tuesday data for challenge problem
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 


## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.
  

```r
garden_harvest %>% 
  select(`vegetable`, `weight`, `date`) %>% 
  mutate(day = wday(date, label = TRUE)) %>% 
  group_by(`day`, `vegetable`) %>% 
  summarise(wgt = max(cumsum(weight))) %>% 
  pivot_wider(id_cols = `vegetable`,
              names_from = `day`,
              values_from = `wgt`)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["Sun"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["Mon"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["Tue"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["Wed"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["Thu"],"name":[6],"type":["dbl"],"align":["right"]},{"label":["Fri"],"name":[7],"type":["dbl"],"align":["right"]},{"label":["Sat"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"beans","2":"868","3":"2952","4":"1990","5":"1852","6":"1539","7":"692","8":"2136"},{"1":"beets","2":"146","3":"305","4":"72","5":"83","6":"5394","7":"11","8":"172"},{"1":"broccoli","2":"571","3":"372","4":"NA","5":"321","6":"NA","7":"75","8":"NA"},{"1":"carrots","2":"1332","3":"395","4":"160","5":"2523","6":"1213","7":"970","8":"1057"},{"1":"corn","2":"661","3":"344","4":"330","5":"2405","6":"NA","7":"1564","8":"597"},{"1":"cucumbers","2":"1408","3":"2166","4":"4557","5":"2407","6":"1500","7":"3370","8":"4373"},{"1":"jalapeño","2":"119","3":"2519","4":"249","5":"218","6":"102","7":"587","8":"684"},{"1":"kale","2":"375","3":"938","4":"128","5":"280","6":"127","7":"173","8":"676"},{"1":"lettuce","2":"665","3":"1115","4":"416","5":"538","6":"1112","7":"817","8":"597"},{"1":"onions","2":"118","3":"231","4":"321","5":"NA","6":"273","7":"33","8":"868"},{"1":"peas","2":"933","3":"2102","4":"938","5":"490","6":"1541","7":"425","8":"1294"},{"1":"peppers","2":"228","3":"1146","4":"655","5":"1108","6":"322","7":"152","8":"627"},{"1":"radish","2":"37","3":"89","4":"43","5":"NA","6":"67","7":"88","8":"105"},{"1":"rutabaga","2":"8738","3":"NA","4":"NA","5":"NA","6":"NA","7":"1623","8":"3129"},{"1":"spinach","2":"221","3":"67","4":"225","5":"97","6":"106","7":"89","8":"118"},{"1":"strawberries","2":"37","3":"217","4":"NA","5":"NA","6":"40","7":"221","8":"77"},{"1":"Swiss chard","2":"566","3":"487","4":"32","5":"412","6":"1012","7":"280","8":"333"},{"1":"tomatoes","2":"34296","3":"5213","4":"22113","5":"26429","6":"15657","7":"38590","8":"15933"},{"1":"zucchini","2":"5550","3":"5532","4":"7470","5":"926","6":"15708","7":"8492","8":"1549"},{"1":"basil","2":"NA","3":"30","4":"50","5":"NA","6":"12","7":"212","8":"186"},{"1":"hot peppers","2":"NA","3":"571","4":"64","5":"31","6":"NA","7":"NA","8":"NA"},{"1":"potatoes","2":"NA","3":"440","4":"NA","5":"2073","6":"5376","7":"1697","8":"1271"},{"1":"pumpkins","2":"NA","3":"13662","4":"14450","5":"NA","6":"NA","7":"NA","8":"42043"},{"1":"raspberries","2":"NA","3":"59","4":"152","5":"NA","6":"131","7":"259","8":"242"},{"1":"squash","2":"NA","3":"11038","4":"8377","5":"NA","6":"NA","7":"NA","8":"25502"},{"1":"cilantro","2":"NA","3":"NA","4":"2","5":"NA","6":"NA","7":"33","8":"17"},{"1":"edamame","2":"NA","3":"NA","4":"636","5":"NA","6":"NA","7":"NA","8":"2127"},{"1":"chives","2":"NA","3":"NA","4":"NA","5":"8","6":"NA","7":"NA","8":"NA"},{"1":"kohlrabi","2":"NA","3":"NA","4":"NA","5":"NA","6":"191","7":"NA","8":"NA"},{"1":"apple","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"156"},{"1":"asparagus","2":"NA","3":"NA","4":"NA","5":"NA","6":"NA","7":"NA","8":"20"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?

The problem is that not all of the vegetables there were planted were harvested, so for some of the vegetable varieties, there is NA in cumulative weight. If the user only want to see the vegetable varieties that were harvested, then the data could be filtered for `cum_lbs` for all values not equal to NA.


```r
sum_harvest <-garden_harvest %>% 
  group_by(`variety`, `vegetable`) %>% 
  summarise(cum_lbs = max(cumsum(weight) * 0.00220462))

garden_planting %>% 
  left_join(sum_harvest)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["plot"],"name":[1],"type":["chr"],"align":["left"]},{"label":["vegetable"],"name":[2],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[3],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[5],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[6],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[7],"type":["chr"],"align":["left"]},{"label":["cum_lbs"],"name":[8],"type":["dbl"],"align":["right"]}],"data":[{"1":"A","2":"peas","3":"Super Sugar Snap","4":"22","5":"2020-04-19","6":"TRUE","7":"NA","8":"9.5680508"},{"1":"B","2":"peas","3":"Magnolia Blossom","4":"24","5":"2020-04-19","6":"TRUE","7":"NA","8":"7.4582295"},{"1":"H","2":"onions","3":"Long Keeping Rainbow","4":"40","5":"2020-04-26","6":"FALSE","7":"NA","8":"3.3113392"},{"1":"P","2":"onions","3":"Delicious Duo","4":"25","5":"2020-04-26","6":"FALSE","7":"NA","8":"0.7539800"},{"1":"C","2":"lettuce","3":"Farmer's Market Blend","4":"60","5":"2020-05-02","6":"FALSE","7":"NA","8":"3.8029695"},{"1":"C","2":"radish","3":"Garden Party Mix","4":"20","5":"2020-05-02","6":"FALSE","7":"NA","8":"0.9457820"},{"1":"D","2":"potatoes","3":"purple","4":"5","5":"2020-05-02","6":"FALSE","7":"NA","8":"3.0093063"},{"1":"D","2":"potatoes","3":"Russet","4":"8","5":"2020-05-02","6":"FALSE","7":"NA","8":"9.0918529"},{"1":"I","2":"potatoes","3":"yellow","4":"10","5":"2020-05-02","6":"FALSE","7":"NA","8":"7.4009093"},{"1":"P","2":"kale","3":"Heirloom Lacinto","4":"30","5":"2020-05-02","6":"FALSE","7":"NA","8":"5.9458601"},{"1":"G","2":"radish","3":"Garden Party Mix","4":"30","5":"2020-05-02","6":"FALSE","7":"NA","8":"0.9457820"},{"1":"H","2":"carrots","3":"Bolero","4":"50","5":"2020-05-02","6":"FALSE","7":"NA","8":"8.2915758"},{"1":"H","2":"carrots","3":"King Midas","4":"50","5":"2020-05-02","6":"FALSE","7":"NA","8":"4.0961840"},{"1":"H","2":"carrots","3":"Dragon","4":"40","5":"2020-05-02","6":"FALSE","7":"NA","8":"4.1050024"},{"1":"H","2":"beets","3":"Gourmet Golden","4":"40","5":"2020-05-02","6":"FALSE","7":"NA","8":"7.0217147"},{"1":"H","2":"beets","3":"Sweet Merlin","4":"40","5":"2020-05-02","6":"FALSE","7":"NA","8":"6.3867841"},{"1":"P","2":"lettuce","3":"Tatsoi","4":"25","5":"2020-05-02","6":"FALSE","7":"NA","8":"2.8946661"},{"1":"M","2":"Swiss chard","3":"Neon Glow","4":"25","5":"2020-05-02","6":"FALSE","7":"NA","8":"6.8828236"},{"1":"H","2":"radish","3":"Garden Party Mix","4":"15","5":"2020-05-02","6":"FALSE","7":"NA","8":"0.9457820"},{"1":"O","2":"edamame","3":"edamame","4":"25","5":"2020-05-16","6":"FALSE","7":"NA","8":"6.0913651"},{"1":"H","2":"spinach","3":"Catalina","4":"50","5":"2020-05-16","6":"FALSE","7":"NA","8":"2.0348643"},{"1":"P","2":"broccoli","3":"Yod Fah","4":"25","5":"2020-05-16","6":"FALSE","7":"NA","8":"0.8201186"},{"1":"M","2":"beans","3":"Bush Bush Slender","4":"30","5":"2020-05-16","6":"FALSE","7":"NA","8":"22.1299756"},{"1":"L","2":"lettuce","3":"Farmer's Market Blend","4":"60","5":"2020-05-16","6":"FALSE","7":"NA","8":"3.8029695"},{"1":"A","2":"squash","3":"Butternut (saved)","4":"8","5":"2020-05-20","6":"TRUE","7":"NA","8":"NA"},{"1":"A","2":"squash","3":"Red Kuri","4":"4","5":"2020-05-20","6":"TRUE","7":"NA","8":"22.7318368"},{"1":"A","2":"squash","3":"Blue (saved)","4":"4","5":"2020-05-20","6":"TRUE","7":"NA","8":"41.5240177"},{"1":"A","2":"squash","3":"Waltham Butternut","4":"4","5":"2020-05-20","6":"TRUE","7":"NA","8":"24.2706616"},{"1":"A","2":"pumpkins","3":"Cinderalla's Carraige","4":"3","5":"2020-05-20","6":"TRUE","7":"NA","8":"NA"},{"1":"B","2":"squash","3":"Red Kuri","4":"4","5":"2020-05-20","6":"TRUE","7":"NA","8":"22.7318368"},{"1":"B","2":"squash","3":"Blue (saved)","4":"8","5":"2020-05-20","6":"TRUE","7":"NA","8":"41.5240177"},{"1":"B","2":"pumpkins","3":"Cinderella's Carraige","4":"3","5":"2020-05-20","6":"TRUE","7":"NA","8":"32.8730888"},{"1":"B","2":"pumpkins","3":"saved","4":"8","5":"2020-05-20","6":"TRUE","7":"NA","8":"76.9324195"},{"1":"J","2":"tomatoes","3":"Mortgage Lifter","4":"1","5":"2020-05-20","6":"TRUE","7":"died","8":"26.3253674"},{"1":"J","2":"tomatoes","3":"Old German","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"26.7177898"},{"1":"J","2":"tomatoes","3":"Better Boy","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"34.0084681"},{"1":"J","2":"tomatoes","3":"Amish Paste","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"65.6734252"},{"1":"J","2":"tomatoes","3":"Cherokee Purple","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"15.7123267"},{"1":"J","2":"tomatoes","3":"Brandywine","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"15.6461881"},{"1":"J","2":"tomatoes","3":"Bonny Best","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"24.9232291"},{"1":"N","2":"tomatoes","3":"Amish Paste","4":"2","5":"2020-05-20","6":"TRUE","7":"NA","8":"65.6734252"},{"1":"N","2":"tomatoes","3":"Big Beef","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"24.9937769"},{"1":"N","2":"tomatoes","3":"Mortgage Lifter","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"26.3253674"},{"1":"N","2":"tomatoes","3":"Black Krim","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"15.8071254"},{"1":"N","2":"tomatoes","3":"Jet Star","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"15.0244853"},{"1":"N","2":"tomatoes","3":"Better Boy","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"34.0084681"},{"1":"O","2":"tomatoes","3":"grape","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"32.3946863"},{"1":"K","2":"peppers","3":"green","4":"12","5":"2020-05-21","6":"TRUE","7":"NA","8":"5.6923288"},{"1":"O","2":"peppers","3":"green","4":"5","5":"2020-05-21","6":"TRUE","7":"NA","8":"5.6923288"},{"1":"potA","2":"peppers","3":"variety","4":"3","5":"2020-05-21","6":"TRUE","7":"NA","8":"3.6508507"},{"1":"potA","2":"peppers","3":"variety","4":"3","5":"2020-05-21","6":"TRUE","7":"NA","8":"3.6508507"},{"1":"potB","2":"hot peppers","3":"thai","4":"1","5":"2020-05-21","6":"TRUE","7":"NA","8":"0.1477095"},{"1":"potC","2":"hot peppers","3":"variety","4":"6","5":"2020-05-21","6":"TRUE","7":"NA","8":"1.3205674"},{"1":"potD","2":"peppers","3":"variety","4":"1","5":"2020-05-21","6":"TRUE","7":"NA","8":"3.6508507"},{"1":"potB","2":"basil","3":"Isle of Naxos","4":"40","5":"2020-05-16","6":"FALSE","7":"NA","8":"1.0802638"},{"1":"wagon","2":"dill","3":"Grandma Einck's","4":"40","5":"2020-05-16","6":"FALSE","7":"NA","8":"NA"},{"1":"potD","2":"cilantro","3":"cilantro","4":"15","5":"2020-05-16","6":"FALSE","7":"NA","8":"0.1146402"},{"1":"D","2":"beans","3":"Bush Bush Slender","4":"10","5":"2020-05-21","6":"TRUE","7":"NA","8":"22.1299756"},{"1":"D","2":"zucchini","3":"Romanesco","4":"3","5":"2020-05-21","6":"TRUE","7":"NA","8":"99.7083487"},{"1":"D","2":"brussels sprouts","3":"Long Island","4":"13","5":"2020-05-21","6":"TRUE","7":"NA","8":"NA"},{"1":"L","2":"jalapeño","3":"giant","4":"4","5":"2020-05-21","6":"TRUE","7":"NA","8":"9.8722884"},{"1":"D","2":"broccoli","3":"Main Crop Bravado","4":"7","5":"2020-05-22","6":"TRUE","7":"NA","8":"2.1318675"},{"1":"I","2":"broccoli","3":"Main Crop Bravado","4":"7","5":"2020-05-22","6":"TRUE","7":"NA","8":"2.1318675"},{"1":"I","2":"potatoes","3":"yellow","4":"8","5":"2020-05-22","6":"TRUE","7":"NA","8":"7.4009093"},{"1":"I","2":"potatoes","3":"red","4":"3","5":"2020-05-22","6":"FALSE","7":"NA","8":"4.4334908"},{"1":"side","2":"pumpkins","3":"Big Max","4":"6","5":"2020-05-24","6":"TRUE","7":"NA","8":"NA"},{"1":"side","2":"pumpkins","3":"Cinderalla's Carraige","4":"6","5":"2020-05-24","6":"TRUE","7":"NA","8":"NA"},{"1":"side","2":"watermelon","3":"Doll Babies","4":"8","5":"2020-05-24","6":"TRUE","7":"NA","8":"NA"},{"1":"side","2":"melon","3":"honeydew","4":"5","5":"2020-05-24","6":"TRUE","7":"NA","8":"NA"},{"1":"K","2":"squash","3":"Waltham Butternut","4":"6","5":"2020-05-25","6":"TRUE","7":"NA","8":"24.2706616"},{"1":"K","2":"squash","3":"delicata","4":"8","5":"2020-05-25","6":"TRUE","7":"NA","8":"10.4984004"},{"1":"K","2":"pumpkins","3":"New England Sugar","4":"4","5":"2020-05-25","6":"TRUE","7":"NA","8":"44.8596078"},{"1":"K","2":"beans","3":"Chinese Red Noodle","4":"5","5":"2020-05-25","6":"TRUE","7":"NA","8":"0.7848447"},{"1":"L","2":"beans","3":"Chinese Red Noodle","4":"5","5":"2020-05-25","6":"TRUE","7":"NA","8":"0.7848447"},{"1":"L","2":"carrots","3":"Bolero","4":"50","5":"2020-05-25","6":"FALSE","7":"NA","8":"8.2915758"},{"1":"L","2":"carrots","3":"King Midas","4":"50","5":"2020-05-25","6":"FALSE","7":"NA","8":"4.0961840"},{"1":"L","2":"carrots","3":"Dragon","4":"50","5":"2020-05-25","6":"FALSE","7":"NA","8":"4.1050024"},{"1":"E","2":"rudabaga","3":"Improved Helenor","4":"30","5":"2020-05-25","6":"FALSE","7":"NA","8":"NA"},{"1":"L","2":"cucumbers","3":"pickling","4":"20","5":"2020-05-25","6":"FALSE","7":"NA","8":"43.6095882"},{"1":"F","2":"strawberries","3":"perennial","4":"NA","5":"<NA>","6":"NA","7":"NA","8":"NA"},{"1":"N","2":"tomatoes","3":"volunteers","4":"1","5":"2020-06-03","6":"TRUE","7":"NA","8":"51.6123588"},{"1":"J","2":"tomatoes","3":"volunteers","4":"1","5":"2020-06-03","6":"TRUE","7":"NA","8":"51.6123588"},{"1":"side","2":"squash","3":"Red Kuri","4":"1","5":"2020-05-20","6":"TRUE","7":"NA","8":"22.7318368"},{"1":"front","2":"tomatoes","3":"volunteers","4":"5","5":"2020-06-03","6":"TRUE","7":"NA","8":"51.6123588"},{"1":"O","2":"tomatoes","3":"volunteers","4":"2","5":"2020-06-03","6":"TRUE","7":"NA","8":"51.6123588"},{"1":"A","2":"corn","3":"Dorinny Sweet","4":"20","5":"2020-05-25","6":"FALSE","7":"NA","8":"11.4067039"},{"1":"B","2":"corn","3":"Golden Bantam","4":"20","5":"2020-05-25","6":"FALSE","7":"NA","8":"1.6027587"},{"1":"front","2":"kohlrabi","3":"Crispy Colors Duo","4":"10","5":"2020-05-20","6":"FALSE","7":"NA","8":"0.4210824"},{"1":"E","2":"spinach","3":"Catalina","4":"100","5":"2020-06-20","6":"FALSE","7":"NA","8":"2.0348643"},{"1":"E","2":"beans","3":"Classic Slenderette","4":"29","5":"2020-06-20","6":"TRUE","7":"NA","8":"3.6045537"},{"1":"G","2":"lettuce","3":"Lettuce Mixture","4":"200","5":"2020-06-20","6":"FALSE","7":"NA","8":"4.7487515"},{"1":"E","2":"cilantro","3":"cilantro","4":"20","5":"2020-06-20","6":"FALSE","7":"NA","8":"0.1146402"},{"1":"front","2":"kale","3":"Heirloom Lacinto","4":"30","5":"2020-06-20","6":"FALSE","7":"NA","8":"5.9458601"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.
  
Understanding how much money is saved from gardening is possible by first summing up the weight in pounds of each of the varieties of vegetables that were harvested. Then it would be helpful to join the garden_spending to the garden_harvest table by variety because then the cost of each of the varieties of vegetables can be used with the summed weight of the vegetable varieties. Then it would be helpful to join an outside data set from a store which shows the price in pound of the vegetables that were harvested in the garden. Next the cost of the seeds for vegetable varieties can be divided by the summed weight of that variety, giving us price of vegetable per pound. That cost can then be compared to the cost from the outside data set, which tends to hold the cost of item by pounds. The costs can be compared by multiplying the outside cost of a vegetable variety by the number of pounds of the item harvested, giving us the comparative outside cost. That cost can then subtract the cost spent for the garden, giving the dollar amount saved.

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order. CHALLENGE: add the date near the end of the bar. (This is probably not a super useful graph because it's difficult to read. This is more an exercise in using some of the functions you just learned.)
  

```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(`variety`) %>% 
  summarise(date = min(`date`),
            sum_weight = sum(weight) * 0.00220462) %>% 
  ggplot(aes(x=`sum_weight`, y=`variety`, fill = `variety`))+
  geom_col()
```

![](03_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>% 
  mutate(lowercase = str_to_lower(variety),
         len = str_length(variety)) %>% 
  distinct(`variety`, `vegetable`, `len`) %>% 
  arrange(`len`, `vegetable`)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["len"],"name":[3],"type":["int"],"align":["right"]}],"data":[{"1":"potatoes","2":"red","3":"3"},{"1":"hot peppers","2":"thai","3":"4"},{"1":"jalapeño","2":"giant","3":"5"},{"1":"peppers","2":"green","3":"5"},{"1":"pumpkins","2":"saved","3":"5"},{"1":"tomatoes","2":"grape","3":"5"},{"1":"beets","2":"leaves","3":"6"},{"1":"carrots","2":"Dragon","3":"6"},{"1":"carrots","2":"Bolero","3":"6"},{"1":"carrots","2":"greens","3":"6"},{"1":"lettuce","2":"reseed","3":"6"},{"1":"lettuce","2":"Tatsoi","3":"6"},{"1":"potatoes","2":"purple","3":"6"},{"1":"potatoes","2":"yellow","3":"6"},{"1":"potatoes","2":"Russet","3":"6"},{"1":"apple","2":"unknown","3":"7"},{"1":"broccoli","2":"Yod Fah","3":"7"},{"1":"edamame","2":"edamame","3":"7"},{"1":"hot peppers","2":"variety","3":"7"},{"1":"peppers","2":"variety","3":"7"},{"1":"cilantro","2":"cilantro","3":"8"},{"1":"cucumbers","2":"pickling","3":"8"},{"1":"spinach","2":"Catalina","3":"8"},{"1":"squash","2":"delicata","3":"8"},{"1":"squash","2":"Red Kuri","3":"8"},{"1":"tomatoes","2":"Big Beef","3":"8"},{"1":"tomatoes","2":"Jet Star","3":"8"},{"1":"asparagus","2":"asparagus","3":"9"},{"1":"chives","2":"perrenial","3":"9"},{"1":"raspberries","2":"perrenial","3":"9"},{"1":"strawberries","2":"perrenial","3":"9"},{"1":"Swiss chard","2":"Neon Glow","3":"9"},{"1":"zucchini","2":"Romanesco","3":"9"},{"1":"carrots","2":"King Midas","3":"10"},{"1":"tomatoes","2":"Bonny Best","3":"10"},{"1":"tomatoes","2":"Better Boy","3":"10"},{"1":"tomatoes","2":"Old German","3":"10"},{"1":"tomatoes","2":"Brandywine","3":"10"},{"1":"tomatoes","2":"Black Krim","3":"10"},{"1":"tomatoes","2":"volunteers","3":"10"},{"1":"tomatoes","2":"Amish Paste","3":"11"},{"1":"beets","2":"Sweet Merlin","3":"12"},{"1":"squash","2":"Blue (saved)","3":"12"},{"1":"basil","2":"Isle of Naxos","3":"13"},{"1":"corn","2":"Dorinny Sweet","3":"13"},{"1":"corn","2":"Golden Bantam","3":"13"},{"1":"onions","2":"Delicious Duo","3":"13"},{"1":"beets","2":"Gourmet Golden","3":"14"},{"1":"lettuce","2":"mustard greens","3":"14"},{"1":"lettuce","2":"Lettuce Mixture","3":"15"},{"1":"tomatoes","2":"Cherokee Purple","3":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"15"},{"1":"kale","2":"Heirloom Lacinto","3":"16"},{"1":"peas","2":"Magnolia Blossom","3":"16"},{"1":"peas","2":"Super Sugar Snap","3":"16"},{"1":"radish","2":"Garden Party Mix","3":"16"},{"1":"rutabaga","2":"Improved Helenor","3":"16"},{"1":"beans","2":"Bush Bush Slender","3":"17"},{"1":"broccoli","2":"Main Crop Bravado","3":"17"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"17"},{"1":"pumpkins","2":"New England Sugar","3":"17"},{"1":"squash","2":"Waltham Butternut","3":"17"},{"1":"beans","2":"Chinese Red Noodle","3":"18"},{"1":"beans","2":"Classic Slenderette","3":"19"},{"1":"onions","2":"Long Keeping Rainbow","3":"20"},{"1":"lettuce","2":"Farmer's Market Blend","3":"21"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"21"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>% 
  distinct(vegetable, variety) %>% 
  mutate(er_or_ar = str_detect(variety, "er|ar")) %>% 
  filter(er_or_ar)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["er_or_ar"],"name":[3],"type":["lgl"],"align":["right"]}],"data":[{"1":"radish","2":"Garden Party Mix","3":"TRUE"},{"1":"lettuce","2":"Farmer's Market Blend","3":"TRUE"},{"1":"peas","2":"Super Sugar Snap","3":"TRUE"},{"1":"chives","2":"perrenial","3":"TRUE"},{"1":"strawberries","2":"perrenial","3":"TRUE"},{"1":"asparagus","2":"asparagus","3":"TRUE"},{"1":"lettuce","2":"mustard greens","3":"TRUE"},{"1":"raspberries","2":"perrenial","3":"TRUE"},{"1":"beans","2":"Bush Bush Slender","3":"TRUE"},{"1":"beets","2":"Sweet Merlin","3":"TRUE"},{"1":"hot peppers","2":"variety","3":"TRUE"},{"1":"tomatoes","2":"Cherokee Purple","3":"TRUE"},{"1":"tomatoes","2":"Better Boy","3":"TRUE"},{"1":"peppers","2":"variety","3":"TRUE"},{"1":"tomatoes","2":"Mortgage Lifter","3":"TRUE"},{"1":"tomatoes","2":"Old German","3":"TRUE"},{"1":"tomatoes","2":"Jet Star","3":"TRUE"},{"1":"carrots","2":"Bolero","3":"TRUE"},{"1":"tomatoes","2":"volunteers","3":"TRUE"},{"1":"beans","2":"Classic Slenderette","3":"TRUE"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"TRUE"},{"1":"squash","2":"Waltham Butternut","3":"TRUE"},{"1":"pumpkins","2":"New England Sugar","3":"TRUE"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){width="30%"}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){width="30%"}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usual, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>% 
  ggplot(aes(x = sdate))+
  geom_density()+
  labs(title = "Date vs. Density of Events",
       x = "Date in Days",
       y = "Events")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>% 
  mutate(timeOfDay = hour(sdate) + (minute(sdate) * (1/60))) %>% 
  ggplot()+
  geom_density(aes(x = timeOfDay))+
  labs(title = "Time Of Day vs. Density of Events",
       x = "Time Of Day in 24 hour",
       y = NULL)+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>% 
  mutate(day = wday(sdate, label = TRUE)) %>% 
  ggplot(aes(y=day,
             fill = day))+
  geom_bar()+
  labs(title = "Days of Week vs. Number of Events")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?

There is a pattern, during the typical corporate work week, there is a spike in density of events at the start and end of the work day. On Saturdays and Sundays, there is an increase in the afternoon however the change in density is much more gradual compared to the changes during the work week.


```r
Trips %>% 
  mutate(timeOfDay = hour(sdate) + (minute(sdate) * (1/60)),
         dayOfWeek = wday(sdate, label = TRUE)) %>% 
  ggplot()+
  geom_density(aes(x = timeOfDay))+
  facet_wrap(~dayOfWeek)+
  labs(title = "Day of Week vs. Density of Events",
       x = "Time Of Day in 24 hour",
       y = NULL)+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.


```r
Trips %>% 
  mutate(timeOfDay = hour(sdate) + (minute(sdate) * (1/60)),
         dayOfWeek = wday(sdate, label = TRUE)) %>% 
  ggplot()+
  geom_density(aes(x = timeOfDay, fill = client), alpha = .5, color = NA)+
  facet_wrap(~dayOfWeek)+
  labs(title = "Day of Week vs. Density of Events",
       x = "Time Of Day in 24 hour",
       y = NULL)+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  
I think this graph is better in terms of telling a story. The only real difference between the graph is 11 and this graph is the markings on the y-axis. This graph has different y-axis labels which are scaled differently so while these graphs might not be direct interpretations of the data, they are able to interpreted with more ease. While these graphs are easier to interpret, they are less helpful if looking from the data set to the graphs, that would be confusing and not translate easily.


```r
Trips %>% 
  mutate(timeOfDay = hour(sdate) + (minute(sdate) * (1/60)),
         dayOfWeek = wday(sdate, label = TRUE)) %>% 
  ggplot()+
  geom_density(aes(x = timeOfDay, 
                   fill = client), 
               alpha = .5, 
               color = NA,
               position = position_stack())+
  facet_wrap(~dayOfWeek)+
  labs(title = "Day of Week vs. Density of Events",
       x = "Time Of Day in 24 hour",
       y = NULL)+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>% 
  mutate(timeOfDay = hour(sdate) + (minute(sdate) * (1/60)),
         dayOfWeek = wday(sdate, label = TRUE),
         weekend = ifelse(wday(sdate) == 1 | wday(sdate) == 7, "weekend", "weekday")) %>% 
  ggplot()+
  geom_density(aes(x = timeOfDay, fill = client), alpha = .5, color = NA)+
  facet_wrap(~weekend)+
  labs(title = NULL,
       x = "Time Of Day in 24 hour",
       y = NULL)
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  
This graph doesn't tell us any information that the previous graph didn't however it is formatted differently which allows the reader to compare the information differently. I don't believe that one graph is better than another, it really depends on the information and comparisons that the reader is hoping to make. If the reader wants to compare casual and registered users this graph is more helpful however if the reader wants to compare weekday and weekend rides, the previous graph is better.
  

```r
Trips %>% 
  mutate(timeOfDay = hour(sdate) + (minute(sdate) * (1/60)),
         dayOfWeek = wday(sdate, label = TRUE),
         weekend = ifelse(wday(sdate) == 1 | wday(sdate) == 7, "weekend", "weekday")) %>% 
  ggplot()+
  geom_density(aes(x = timeOfDay, fill = weekend), alpha = .5, color = NA)+
  facet_wrap(~client)+
  labs(title = NULL,
       x = "Time Of Day in 24 hour",
       y = NULL)
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Small_Trips <- Trips %>% 
  group_by(`sstation`) %>% 
  summarise(num_departures = n()) %>% 
  ungroup()

Small_Trips %>% 
  left_join(Stations,
            by = c("sstation"="name")) %>% 
  select(`sstation`, `lat`, `long`, `num_departures`) %>% 
  ggplot()+
  geom_point(mapping = aes(x = `lat`, 
                           y = `long`,
                           color = `num_departures`))+
    labs(title = "Map of Stations based on Longitude and Latitude Positioning",
       x = "Longitude",
       y = "Latitude")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  


```r
Trips %>% 
  group_by(`sstation`) %>% 
  summarise()
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["sstation"],"name":[1],"type":["chr"],"align":["left"]}],"data":[{"1":"10th & E St NW"},{"1":"10th & Florida Ave NW"},{"1":"10th & Monroe St NE"},{"1":"10th & U St NW"},{"1":"10th St & Constitution Ave NW"},{"1":"11th & F St NW"},{"1":"11th & H St NE"},{"1":"11th & K St NW"},{"1":"11th & Kenyon St NW"},{"1":"11th & M St NW"},{"1":"12th & Army Navy Dr"},{"1":"12th & Irving St NE"},{"1":"12th & L St NW"},{"1":"12th & Newton St NE"},{"1":"12th & U St NW"},{"1":"13th & D St NE"},{"1":"13th & H St NE"},{"1":"13th St & Eastern Ave"},{"1":"13th St & New York Ave NW"},{"1":"14th & Belmont St NW"},{"1":"14th & D St NW / Ronald Reagan Building"},{"1":"14th & D St SE"},{"1":"14th & G St NW"},{"1":"14th & Harvard St NW"},{"1":"14th & R St NW"},{"1":"14th & Rhode Island Ave NW"},{"1":"14th & Upshur St NW"},{"1":"14th & V St NW"},{"1":"14th St & Colorado Ave NW"},{"1":"14th St & New York Ave NW"},{"1":"14th St & Spring Rd NW"},{"1":"14th St Heights / 14th & Crittenden St NW"},{"1":"15th & Crystal Dr"},{"1":"15th & East Capitol St NE"},{"1":"15th & Euclid St  NW"},{"1":"15th & F St NE"},{"1":"15th & K St NW"},{"1":"15th & L St NW"},{"1":"15th & N Scott St"},{"1":"15th & P St NW"},{"1":"15th St & Massachusetts Ave SE"},{"1":"16th & Harvard St NW"},{"1":"17th & Corcoran St NW"},{"1":"17th & G St NW"},{"1":"17th & K St NW"},{"1":"17th & K St NW / Farragut Square"},{"1":"17th & Rhode Island Ave NW"},{"1":"17th St & Massachusetts Ave NW"},{"1":"18th & Eads St."},{"1":"18th & M St NW"},{"1":"18th & R St NW"},{"1":"18th St & Pennsylvania Ave NW"},{"1":"18th St & Rhode Island Ave NE"},{"1":"18th St & Wyoming Ave NW"},{"1":"19th & E Street NW"},{"1":"19th & East Capitol St SE"},{"1":"19th & K St NW"},{"1":"19th & L St NW"},{"1":"19th St & Constitution Ave NW"},{"1":"19th St & Pennsylvania Ave NW"},{"1":"1st & H St NW"},{"1":"1st & K St SE"},{"1":"1st & M St NE"},{"1":"1st & N St  SE"},{"1":"1st & Rhode Island Ave NW"},{"1":"1st & Washington Hospital Center NW"},{"1":"20th & Bell St"},{"1":"20th & Crystal Dr"},{"1":"20th & E St NW"},{"1":"20th & L St NW"},{"1":"20th & O St NW / Dupont South"},{"1":"20th St & Florida Ave NW"},{"1":"20th St & Virginia Ave NW"},{"1":"21st & I St NW"},{"1":"21st & M St NW"},{"1":"21st St & Constitution Ave NW"},{"1":"21st St & Pennsylvania Ave NW"},{"1":"21st St N & N Pierce St"},{"1":"22nd & Eads St"},{"1":"22nd & I St NW / Foggy Bottom"},{"1":"23rd & Crystal Dr"},{"1":"23rd & E St NW"},{"1":"24th & N St NW"},{"1":"25th St & Pennsylvania Ave NW"},{"1":"26th & S Clark St"},{"1":"27th & Crystal Dr"},{"1":"28th St S & S Meade St"},{"1":"3000 Connecticut Ave NW / National Zoo"},{"1":"31st & Woodrow St S"},{"1":"34th & Water St NW"},{"1":"34th St & Minnesota Ave SE"},{"1":"34th St & Wisconsin Ave NW"},{"1":"36th & Calvert St NW / Glover Park"},{"1":"37th & O St NW / Georgetown University"},{"1":"39th & Calvert St NW / Stoddert"},{"1":"39th & Veazey St NW"},{"1":"3rd & D St SE"},{"1":"3rd & Elm St NW"},{"1":"3rd & G St SE"},{"1":"3rd & H St NE"},{"1":"3rd & H St NW"},{"1":"3rd & Tingey St SE"},{"1":"3rd St & Pennsylvania Ave SE"},{"1":"47th & Elm St"},{"1":"4th & D St NW / Judiciary Square"},{"1":"4th & E St SW"},{"1":"4th & East Capitol St NE"},{"1":"4th & M St SW"},{"1":"4th & W St NE"},{"1":"5th & F St NW"},{"1":"5th & K St NW"},{"1":"5th & Kennedy St NW"},{"1":"5th St & Massachusetts Ave NW"},{"1":"6th & H St NE"},{"1":"6th & S Ball St"},{"1":"6th & Water St SW / SW Waterfront"},{"1":"6th St & Indiana Ave NW"},{"1":"7th & F St NW / National Portrait Gallery"},{"1":"7th & R St NW / Shaw Library"},{"1":"7th & T St NW"},{"1":"8th & D St NW"},{"1":"8th & East Capitol St NE"},{"1":"8th & Eye St SE / Barracks Row"},{"1":"8th & F St NE"},{"1":"8th & F St NW / National Portrait Gallery"},{"1":"8th & H St NW"},{"1":"9th & Upshur St NW"},{"1":"Adams Mill & Columbia Rd NW"},{"1":"Alabama & MLK Ave SE"},{"1":"Anacostia Ave & Benning Rd NE / River Terrace"},{"1":"Anacostia Library"},{"1":"Anacostia Metro"},{"1":"Arlington Blvd & Fillmore St"},{"1":"Arlington Blvd & N Queen St"},{"1":"Arlington Blvd & S George Mason Dr/NFATC"},{"1":"Aurora Hills Community Ctr/18th & Hayes St"},{"1":"Ballenger Ave & Dulaney St"},{"1":"Ballston Metro / N Stuart & 9th St N"},{"1":"Barton St & 10th St N"},{"1":"Battery Ln & Trolley Trail"},{"1":"Benning Branch Library"},{"1":"Benning Rd & East Capitol St NE / Benning Rd Metro"},{"1":"Bethesda Ave & Arlington Rd"},{"1":"Bethesda Metro"},{"1":"Bladensburg Rd & Benning Rd NE"},{"1":"Braddock Rd Metro"},{"1":"Branch & Pennsylvania Ave SE"},{"1":"Broschart & Blackwell Rd"},{"1":"C & O Canal & Wisconsin Ave NW"},{"1":"California St & Florida Ave NW"},{"1":"Calvert & Biltmore St NW"},{"1":"Calvert St & Woodley Pl NW"},{"1":"Carroll & Ethan Allen Ave"},{"1":"Carroll & Westmoreland Ave"},{"1":"Central Library / N Quincy St & 10th St N"},{"1":"Clarendon Blvd & N Fillmore St"},{"1":"Clarendon Blvd & Pierce St"},{"1":"Clarendon Metro / Wilson Blvd & N Highland St"},{"1":"Columbia Pike & S Courthouse Rd"},{"1":"Columbia Pike & S Dinwiddie St / Arlington Mill Community Center"},{"1":"Columbia Pike & S Orme St"},{"1":"Columbia Pike & S Walter Reed Dr"},{"1":"Columbia Rd & Belmont St NW"},{"1":"Columbia Rd & Georgia Ave NW"},{"1":"Columbus Circle / Union Station"},{"1":"Commerce St & Fayette St"},{"1":"Congress Heights Metro"},{"1":"Connecticut & Nebraska Ave NW"},{"1":"Connecticut Ave & Newark St NW / Cleveland Park"},{"1":"Connecticut Ave & Tilden St NW"},{"1":"Constitution Ave & 2nd St NW/DOL"},{"1":"Convention Center / 7th & M St NW"},{"1":"Cordell & Norfolk Ave"},{"1":"Court House Metro / 15th & N Uhle St"},{"1":"Crabbs Branch Way & Calhoun Pl"},{"1":"Crabbs Branch Way & Redland Rd"},{"1":"Crystal City Metro / 18th & Bell St"},{"1":"D St & Maryland Ave NE"},{"1":"Deanwood Rec Center"},{"1":"Duke St & John Carlyle St"},{"1":"E Montgomery Ave & Maryland Ave"},{"1":"East West Hwy & Blair Mill Rd"},{"1":"Eastern Market / 7th & North Carolina Ave SE"},{"1":"Eastern Market Metro / Pennsylvania Ave & 7th St SE"},{"1":"Eckington Pl & Q St NE"},{"1":"Eisenhower Ave & Mill Race Ln"},{"1":"Fairfax Dr & Kenmore St"},{"1":"Fairfax Dr & Wilson Blvd"},{"1":"Fairfax Village"},{"1":"Fallsgrove Blvd & Fallsgrove Dr"},{"1":"Fallsgrove Dr & W Montgomery Ave"},{"1":"Fenton St & Ellsworth Dr"},{"1":"Fenton St & Gist Ave"},{"1":"Fenton St & New York Ave"},{"1":"Fessenden St & Wisconsin Ave NW"},{"1":"Fleet St & Ritchie Pkwy"},{"1":"Florida Ave & R St NW"},{"1":"Fort Totten Metro"},{"1":"Frederick Ave & Horners Ln"},{"1":"Friendship Blvd & Willard Ave"},{"1":"Friendship Hts Metro/Wisconsin Ave & Wisconsin Cir"},{"1":"Gallaudet / 8th St & Florida Ave NE"},{"1":"Garland Ave & Walden Rd"},{"1":"George Mason Dr & Wilson Blvd"},{"1":"Georgetown Harbor / 30th St NW"},{"1":"Georgia & New Hampshire Ave NW"},{"1":"Georgia Ave & Emerson St NW"},{"1":"Georgia Ave & Spring St"},{"1":"Georgia Ave and Fairmont St NW"},{"1":"Glebe Rd & 11th St N"},{"1":"Good Hope & Naylor Rd SE"},{"1":"Good Hope Rd & 14th St SE"},{"1":"Good Hope Rd & MLK Ave SE"},{"1":"Hains Point/Buckeye & Ohio Dr SW"},{"1":"Hamlin & 7th St NE"},{"1":"Harvard St & Adams Mill Rd NW"},{"1":"Henry St & Pendleton St"},{"1":"Idaho Ave & Newark St NW [on 2nd District patio]"},{"1":"Independence Ave & L'Enfant Plaza SW/DOE"},{"1":"Iwo Jima Memorial/N Meade & 14th St N"},{"1":"Jefferson Dr & 14th St SW"},{"1":"Jefferson Memorial"},{"1":"John McCormack Dr & Michigan Ave NE"},{"1":"Kennedy Center"},{"1":"Key Blvd & N Quinn St"},{"1":"King Farm Blvd & Piccard Dr"},{"1":"King Farm Blvd & Pleasant Dr"},{"1":"King St & Patrick St"},{"1":"King St Metro"},{"1":"L'Enfant Plaza / 7th & C St SW"},{"1":"Lamont & Mt Pleasant NW"},{"1":"Lee Hwy & N Cleveland St"},{"1":"Lee Hwy & N Scott St"},{"1":"Lincoln Memorial"},{"1":"Lincoln Park / 13th & East Capitol St NE"},{"1":"Long Bridge Park/Long Bridge Dr & 6th St S"},{"1":"Lynn & 19th St North"},{"1":"M St & Delaware Ave NE"},{"1":"M St & New Jersey Ave SE"},{"1":"M St & Pennsylvania Ave NW"},{"1":"Maple & Ritchie Ave"},{"1":"Market Square / King St & Royal St"},{"1":"Maryland & Independence Ave SW"},{"1":"Massachusetts Ave & Dupont Circle NW"},{"1":"McKinley St & Connecticut Ave NW"},{"1":"Medical Center Dr & Key West Ave"},{"1":"Metro Center / 12th & G St NW"},{"1":"Minnesota Ave Metro/DOES"},{"1":"MLK Library/9th & G St NW"},{"1":"Monroe Ave & Leslie Ave"},{"1":"Monroe St & Monroe Pl"},{"1":"Montgomery & East Ln"},{"1":"Montgomery Ave & Waverly St"},{"1":"Montgomery College/W Campus Dr & Mannakee St"},{"1":"Mount Vernon Ave & E Del Ray Ave"},{"1":"Mount Vernon Ave & E Nelson Ave"},{"1":"Mount Vernon Ave & Kennedy St"},{"1":"N Adams St & Lee Hwy"},{"1":"N Nelson St & Lee Hwy"},{"1":"N Pershing Dr & Wayne St"},{"1":"N Quincy St & Glebe Rd"},{"1":"N Quincy St & Wilson Blvd"},{"1":"N Randolph St & Fairfax Dr"},{"1":"N Rhodes & 16th St N"},{"1":"N Veitch  & 20th St N"},{"1":"N Veitch & Key Blvd"},{"1":"Nannie Helen Burroughs & Minnesota Ave NE"},{"1":"Nannie Helen Burroughs Ave & 49th St NE"},{"1":"Neal St & Trinidad Ave NE"},{"1":"Needwood Rd & Eagles Head Ct"},{"1":"New Hampshire Ave & 24th St NW"},{"1":"New Hampshire Ave & T St NW"},{"1":"New Jersey Ave & N St NW/Dunbar HS"},{"1":"New Jersey Ave & R St NW"},{"1":"New York Ave & 15th St NW"},{"1":"Norfolk & Rugby Ave"},{"1":"Norfolk Ave & Fairmont St"},{"1":"North Capitol St & F St NW"},{"1":"North Capitol St & G Pl NE"},{"1":"Offutt Ln & Chevy Chase Dr"},{"1":"Ohio Dr & West Basin Dr SW / MLK & FDR Memorials"},{"1":"Old Georgetown Rd & Southwick St"},{"1":"Park Rd & Holmead Pl NW"},{"1":"Pennsylvania & Minnesota Ave SE"},{"1":"Pentagon City Metro / 12th & S Hayes St"},{"1":"Pershing & N George Mason Dr"},{"1":"Philadelphia & Maple Ave"},{"1":"Piccard & W Gude Dr"},{"1":"Pleasant St & MLK Ave SE"},{"1":"Potomac & Pennsylvania Ave SE"},{"1":"Potomac Ave & 35th St S"},{"1":"Potomac Ave & 8th St SE"},{"1":"Potomac Greens Dr & Slaters Ln"},{"1":"Prince St & Union St"},{"1":"Randle Circle & Minnesota Ave SE"},{"1":"Rhode Island Ave & V St NE"},{"1":"Rhode Island Ave Metro"},{"1":"Ripley & Bonifant St"},{"1":"River Rd & Landy Ln"},{"1":"Rockville Metro East"},{"1":"Rockville Metro West"},{"1":"Rolfe St & 9th St S"},{"1":"Rosslyn Metro / Wilson Blvd & Ft Myer Dr"},{"1":"S Abingdon St & 36th St S"},{"1":"S Arlington Mill Dr & Campbell Ave"},{"1":"S Four Mile Run Dr & S Shirlington Rd"},{"1":"S George Mason Dr & 13th St S"},{"1":"S George Mason Dr & Four Mile Run"},{"1":"S Glebe & Potomac Ave"},{"1":"S Joyce & 16th St S"},{"1":"S Joyce & Army Navy Dr"},{"1":"S Kenmore & 24th St S"},{"1":"S Oakland St & Columbia Pike"},{"1":"S Stafford & 34th St S"},{"1":"S Troy St & 26th St S"},{"1":"Saint Asaph St & Pendleton  St"},{"1":"Shady Grove Hospital"},{"1":"Shady Grove Metro West"},{"1":"Shirlington Transit Center / S Quincy & Randolph St"},{"1":"Silver Spring Metro/Colesville Rd & Wayne Ave"},{"1":"Smithsonian / Jefferson Dr & 12th St SW"},{"1":"Spring St & Second Ave"},{"1":"Taft St & E Gude Dr"},{"1":"Takoma Metro"},{"1":"Tenleytown / Wisconsin Ave & Albemarle St NW"},{"1":"Thomas Circle"},{"1":"TJ Cmty Ctr / 2nd St & S Old Glebe Rd"},{"1":"Traville Gateway Dr & Gudelsky Dr"},{"1":"Union Market/6th St & Neal Pl NE"},{"1":"US Dept of State / Virginia Ave & 21st St NW"},{"1":"USDA / 12th & Independence Ave SW"},{"1":"Utah St & 11th St N"},{"1":"Van Ness Metro / UDC"},{"1":"Veterans Pl & Pershing Dr"},{"1":"Virginia Square Metro / N Monroe St & 9th St N"},{"1":"Walter Reed Community Center / Walter Reed Dr & 16th St S"},{"1":"Ward Circle / American University"},{"1":"Washington & Independence Ave SW/HHS"},{"1":"Washington Adventist U / Flower Ave & Division St"},{"1":"Washington Blvd & 10th St N"},{"1":"Washington Blvd & 7th St N"},{"1":"Washington Blvd & Walter Reed Dr"},{"1":"Wilson Blvd & Franklin Rd"},{"1":"Wilson Blvd & N Edgewood St"},{"1":"Wilson Blvd & N Oakland St"},{"1":"Wilson Blvd & N Uhle St"},{"1":"Wisconsin Ave & O St NW"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>
  

## Dogs!

In this section, we'll use the data from 2022-02-01 Tidy Tuesday. If you didn't use that data or need a little refresher on it, see the [website](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-02-01/readme.md).

  17. The final product of this exercise will be a graph that has breed on the y-axis and the sum of the numeric ratings in the `breed_traits` dataset on the x-axis, with a dot for each rating. First, create a new dataset called `breed_traits_total` that has two variables -- `Breed` and `total_rating`. The `total_rating` variable is the sum of the numeric ratings in the `breed_traits` dataset (we'll use this dataset again in the next problem). Then, create the graph just described. Omit Breeds with a `total_rating` of 0 and order the Breeds from highest to lowest ranked. You may want to adjust the `fig.height` and `fig.width` arguments inside the code chunk options (eg. `{r, fig.height=8, fig.width=4}`) so you can see things more clearly - check this after you knit the file to assure it looks like what you expected.


```r
breed_traits_total <- breed_traits %>% 
  mutate(total_rating = (`Affectionate With Family`  + `Good With Young Children` + `Good With Other Dogs` + `Shedding Level` + `Coat Grooming Frequency` + `Drooling Level`+ `Openness To Strangers`+ `Playfulness Level` + `Watchdog/Protective Nature` + `Adaptability Level` + `Trainability Level` + `Mental Stimulation Needs` + `Energy Level` + `Barking Level`)/14) %>% 
  select(`Breed`, `total_rating`) %>% 
  filter(`total_rating` != 0)

breed_traits_total %>%
  ggplot()+
  geom_point(mapping = aes(x = `total_rating`, 
                           y = fct_reorder(`Breed`, `total_rating`, max)),
             color = "red")+
  labs(title = "Total Ranking of Dog Traits vs. Breeds",
       x = "Total Ranking",
       y = NULL)+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

  18. The final product of this exercise will be a graph with the top-20 dogs in total ratings (from previous problem) on the y-axis, year on the x-axis, and points colored by each breed's ranking for that year (from the `breed_rank_all` dataset). The points within each breed will be connected by a line, and the breeds should be arranged from the highest median rank to lowest median rank ("highest" is actually the smallest number, eg. 1 = best). After you're finished, think of AT LEAST one thing you could you do to make this graph better. HINTS: 1. Start with the `breed_rank_all` dataset and pivot it so year is a variable. 2. Use the `separate()` function to get year alone, and there's an extra argument in that function that can make it numeric. 3. For both datasets used, you'll need to `str_squish()` Breed before joining. 
  
I added a graph title and changed the axis-labels to make the graph more understandable.


```r
breed_rank_all %>% 
  left_join(breed_traits_total) %>% 
  arrange(desc(`total_rating`)) %>% 
  slice(1:20) %>% 
  pivot_longer(cols= starts_with("20"),
               names_to = "year",
               values_to = "ranking") %>% 
  separate(year,
           c("year"),
           convert = TRUE) %>% 
  group_by(`Breed`) %>% 
  mutate(median_rank = median(`ranking`, na.rm = TRUE)) %>% 
  select(`Breed`, `total_rating`, `year`, `ranking`, `median_rank`) %>% 
  arrange(`median_rank`) %>% 
  ggplot()+
  geom_point(aes(x=`year`, y=fct_reorder(`Breed`, `median_rank`, .desc=TRUE), color=`ranking`))+
  geom_line(mapping = aes(x=`year`, y=`Breed`))+
  labs(title = "Rank by Year vs. Dog Breed",
       x = "Year",
       y = "Dog Breed, ordered by Median Rank from High to Low")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-18-1.png)<!-- -->
  
  19. Create your own! Requirements: use a `join` or `pivot` function (or both, if you'd like), a `str_XXX()` function, and a `fct_XXX()` function to create a graph using any of the dog datasets. One suggestion is to try to improve the graph you created for the Tidy Tuesday assignment. If you want an extra challenge, find a way to use the dog images in the `breed_rank_all` file - check out the `ggimage` library and [this resource](https://wilkelab.org/ggtext/) for putting images as labels.

  

```r
breed_rank_all %>% 
  left_join(`breed_traits_total`) %>% 
  mutate(breed_len = str_length(`Breed`)) %>% 
  arrange(`breed_len`) %>% 
  slice(1:50) %>% 
  ggplot()+
    geom_point(mapping = aes(x=`total_rating`,
                             y=fct_reorder(`Breed`, 
                                           `breed_len`)),
               color = "blue")+
  labs(x="Average Rating of Dog Breeds",
       y = "Breed Names",
       title="Average Rating vs. Dog Breeds")+
  theme(plot.title = element_text(hjust = 0.5))
```

![](03_exercises_files/figure-html/unnamed-chunk-19-1.png)<!-- -->
  
## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.
  
https://github.com/Audrey-Smyczek/smyczek_exercises_repo/blob/59631e0887819920a9c84dfbd31a182fb35c96f9/03_exercises.md

https://github.com/Audrey-Smyczek/smyczek_exercises_repo/blob/59631e0887819920a9c84dfbd31a182fb35c96f9/03_exercises.Rmd


## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.


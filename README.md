# Final-Project
CSVs and Images
Week Seven – Final Project
Introduction

For my final project, I chose to use the Petra Great Temple Excavations project run by Brown University in Jordan from 1993 – 2006 from Open Context, and the “Neutron Activation Analysis of Ceramics from Israel and the Palestinian Territories” project available from The Digital Archaeological Record (tDAR). The pottery that is covered by the Petra Great Temple Excavations is primarily tied to the Roman occupation of the region from the 2nd century BCE up to about 360 CE. The goal is to compare this data with other known Roman sites from the same time period, to see if we can produce any interesting comparisons. The benefit of the tDAR project is that it covers a large variety of sites and has a much more expansive time period. However, not every object has the same amount of data information. Additionally, it can be seen that these are objects that were clearly sent for testing given the additional data provided, but it is unclear whether these are just diagnostic pieces, or how many other pieces may have been excluded from this record. This is a limitation that is inherent in the current data collection and reuse practices, but despite this, there are still worthwhile correlations we can find within the data, and that is what I will be discussing.  Below several images and CSV files are referenced – for full image quality you can find them on Github: https://github.com/anastasia-tesfaye/Final-Project .

Data Sourcing and Cleaning

Given the multiple sources, the first objective was to filter the time periods from the tDAR project to match the Petra files. The issue with a theoretically simple feat was that this data was collected from several sites in the region. Though the headers of the data were the same, the input was very different. For example, Early, Middle, Late Roman, 8th century AD, A.D. 800, or sometimes a combination of them were all used under the column ‘era’. Obviously, things like Middle or Late are a bit subjective, without the further description of even Republic or Empire. Especially as my specialty isn’t during this time period, these were incredibly difficult to parse. Eventually, I understood 4th century CE and onward as Late, Middle as 2nd and 3rd century C.E. and Early as 1st century CE from the file as one grouping listed both the century and early, middle, late in combination. This was frankly a stroke of luck, which guided my decisions on what should and should not be included. Had that group not utilized this combination it would have been much more complicated to parse what those meant. I used Google’s Open Refine to clean both groups of data which lead to this discovery when processing this file. It enabled me to select all the data that fell under this range, without manually sorting due to their grouping feature, and it left me with 63 rows for my tDAR file after excluding all the data that was the wrong time period or had no era context which was, unfortunately, a substantial portion.

As a note, perhaps a skilled classicist or someone who has spent time researching this area would be able to utilize the other information in order to make educated assumptions on those ceramics that did not have this data. However, I do not fall under that category. Therefore, in the interest of having the most correct data possible, I excluded those from my project. The rest of the tDAR data was in great condition and did not require any additional cleaning. I suspect that this data may have been looked over or edited before submission as a group in order to make the data so consistent.

The Open Context data was also relatively clean (in terms of consistency) with only some merging needed on the ‘Pottery Form’ column, however, the issue is that they did not include thickness, diameter, or color in separate columns. This information, along with other location information was included under the ‘description’ section. This was extremely unfortunate as parsing that data is extremely difficult given that it wasn’t separated by commas or any identifying marker to make parsing the cell in python or some other programming language easy. This is a great example of creating unusable or at least extremely difficult to use data. 

Luckily, I do have some background in programming to get some of this data out. The first step was to remove commas that were included in the description. When working with CSVs the commas in the description can lead to issues as them being read as separate columns instead of part of the description column. Therefore, I did a basic find and replace to switch out commas for three dashes. Dashes didn’t appear in the sheet in this way, therefore I could tell where I made changes. Next, I used RegEx (Regular Expressions) in order to write an expression to tell the code what I was looking for and to pull it out. The easiest way to do this and check my work was to use https://regexr.com/. Regexr is an open-source project with  GPLv3 license. (You can find its’ GitHub repository here for more information: https://github.com/gskinner/regexr/.) Using this, I pulled out thickness as almost every object included that variable and converted those listed in centimeters to millimeters for consistency.  

The expression I used was:

(Thickness: )(\d*(\.\d+)?(-\d*(\.\d+)?)* \w+).
While it seems complicated, it is actually pretty self-explanatory. I searched for the word and colon “Thickness:” which is how it was set up within the file, ‘d’ represents a digit, ‘w’ represents a word, ‘+’ represents more than one of whatever proceeded it (a token) in this case it represents more than one digit or word,  ‘*’ represents that more than one of the preceding token is possible but not necessary, a ‘.’ is just looking for a period, a ‘-’ is a dash, and a ‘?’ means that the search can include everything before the ‘?’ or not, which essentially makes that part of the expression optional.  Finally, a set of ‘()’ represents a group. 

In this way, I was able to capture all the different methods of expressing thickness such as 0.15 mm, 1-4 mm, or 10 mm. Question marks were key in finding thickness expressed as a range in select objects. When putting the expression in RegExr, hovering over it will provide explanations, as well as a sample of the final result before you submit. Therefore anyone can use my code above and replicate my results using the Open Context data.

Now that I had a workable expression I was able to use Visual Studio Code to execute. Visual Studio is just my preferred code editor given that it is free and has a lot of add-ons, however, any code editor capable of find and replace using RegEx should work or using Python on Jupyter Notebooks would also be capable of replicating this.  Finally, after saving and exporting, I renamed the various pottery form columns to ‘specific form’ and ‘form’ so they may be compared more easily. As I had the data in a state that was manipulable and back in a CSV, I could begin my analysis. 

Data Visualization and Analysis

To begin, I wanted a general overview of the story each dataset could tell us. There are a lot of fields that were not consistent between the two files, and therefore it was worthwhile to look at each before combining them and limiting the factors I could analyze.  The data visualization program I will be using is RAWGraphs (found here:https://rawgraphs.io/.) This is the program that we have used as a class analyzing other archaeological data. I chose this program because of the ease of use and the ability to chose between several types of graphs quickly.  

Open Context – Petra Great Temple Excavations

First, I took a look at the types of pottery that were discovered at the site.  In the image below, we can see a list of the specific and general forms as listed by the dataset.



The graphs show us that the largest category is Bowls followed by Jugs. Given the alphabetization that is inherent in the graph, it also makes it easy to see related objects such as cooking pot and cooking pot lid or chalice and chalice base. The date information, unfortunately, is not useful to include in a graph. Every object just includes the date range listed as -200 to 360. This essentially makes it impossible to use any of the graphs that would take advantage of patterns of change over time. Luckily, the tDAR documents do have this additional element.  

What we do have is thickness which I extracted from the method mentioned above. Below we can see a comparison of the specific forms and the thickness. We can see that these values are not exactly consistent with each other. Many of the objects have ranges – this can indicate objects that have tapered walls, thereby having a variety of thicknesses. Rather than averaging out these numbers, I left them as is, in order to keep the data as unchanged as possible. The fact that it does not hamper analysis (given the low count of such objects)  also contributed to this decision.

Here is a graph of the thickness in comparison to all the specific form types within the data set.



We can see a few outliers within this range – an amphora base with a 16 mm thickness as well as an amphora handle with a range of 9-10 mm thickness.

tDAR – Neutron Activation Analysis of Ceramics from Israel and the Palestinian Territories

The tDAR data is a compilation of several sites that have been reduced down to the ones that fall under the same or similar time period covered by the Open Context data. Just like our previous graphs, below we can see the distribution of pottery types.



 

Bowls is again the largest grouping followed by jugs.  By adding in the sites, we can also see a breakdown of finds per site – one of the added benefits of using a dataset that already includes multiple locations. In the image below we can see each of the sites listed as well a the general forms indicated.



We can see that every single site had at least a bowl or a jar represented, with the exception of Tel Ashdod. Since our data includes null or empty cells, we can see that it is not that Tel Ashdod did not have those types of pottery but rather no form information was included within the dataset. This is also true of several objects from the other sites but only Tel Ashdod completely ignored that field. It is not immediately clear why this is, however, there are a few possibilities.  It may have been an older excavation where the data was collected differently, that data may have been included in another column that was excluded when they were condensing all the objects, or it is possible that there was just an element of human error that resulted in the loss of the information. 

Comparison

Next, I had to compare them to each other. I combined the data within one sheet and ran it through Open Refine once more. This was to catch any inconsistencies between the two sheets, for example, Cooking Pots versus Cooking pots. Once I did this, it was time to look at the additional challenges of comparing two different datasets.  First, there is an issue of scale – Open Context has about 350 separate entries while the tDAR file has about 64. So I could not compare them purely by the number of vessels. Instead, I took an approach of analyzing the ratio of certain vessel types to their collection. In the beginning, the idea was to include both specific form and group as part of the analysis. However, that method was skewing the data substantially. It seemed that the tDAR data was less varied than Open Context. In the image below you can see that blue indicates the tDAR data and red indicates the Open Context.



The inner circle is the more general form information, and the outer ring is the specific form. From this visualization, it looks as if there are only 5 different general forms for the tDAR data and much more for the Open Context. However, if you look closely you can see this is a matter of subjective categorizing between the two groups of data. For the blue “Bowl” category, we can see that it encompasses things such as cooking pots, however for the Open Context data, we can see that cooking pot is its own general category.  Because of this issue, the only option was to remove the general form data and only analyze specific form data for a clearer picture.  As a note, while this inconsistency skews the data for the ceramics themselves, it does give an interesting insight into how the categorizing of pottery is inconsistent even across sites in the same general area and the same general time period. A possible future project might be to analyze the differences in how archaeologists categorize ceramics in the field through their raw data. 

However, based on these (along with the individual graphs) we can see that bowls followed by jugs were the most common types of pottery discovered in both locations.  This is interesting since we have to consider that the tDAR files may be diagnostic pieces only. This could suggest that bowls and jugs are the most plentiful artifacts found in these types of sites. It could also suggest that bowl and jar are the most generic terminology which encompasses many more artifacts and other categories such as a cooking pot or candle holder are much more narrow. To test this theory I took a look at the thicknesses within the pots and jugs.  I wanted to see if there was a wide range of thicknesses within these two categories which could indicate a significant variety when compared to the other categories.  Below you can see the pivot table I created of the variables as well as a chart showing the correlation. 



 



 We can see that bowls typically had a thickness range from about 2-3mm on average and with only a few outliers. Therefore, this variable shows that there is no significant variation in terms of thickness at least. However, we can see that in the category Jugs there is a much larger range from 1.5mm – 4 mm which may indicate a too broad category. Further statistical analysis is needed to expand upon this query in addition to further analysis of the possible uses of these objects. It may also indicate that a variety of jugs thickness was needed for various purposes. 

Conclusion

Ultimately, we can see that there is quite a bit of information that can be discovered through this type of archaeological analysis. While I did not come to any conclusive result, I did gain a significant amount of information that could lead me to more research. For example, I might want to continue the research into jug thickness, across many other datasets in order to get a more decisive result or in a more methodological approach, I could further analyze how categories are created and used.  I think this is why this type of data is important to keep and use because it can lead to insights that the original creators never thought of or intended and further contributing to academic knowledge. 

 

 

Bibliography

Neutron Activation Analysis of Ceramics from Israel and the Palestinian Territories. ( tDAR id: 371565) ; doi:10.6067/XCV8KP83MX

Martha Sharp Joukowsky. “Petra Great Temple Excavations”. (2007) Martha Sharp Joukowsky (Ed.) . Released: 2007-11-11. Open Context. <http://opencontext.org/projects/A5DDBEA2-B3C8-43F9-8151-33343CBDC857> ARK (Archive): https://n2t.net/ark:/28722/k20r9w91w

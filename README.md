<img src="./Resources/LogoSMM.png" width="400">

# Swiss Music Map Project
> Project for the course Applied Data Analysis, taught by Michele Catasta
EPFL, November 2016

**What?**<br>
• Study upon the evolution of musical genres in Switzerland throughout the years.
<br>
**How?**<br>
• Represent the main musical genres of the Switzerland scene, in a very graphical and interactive way, on a map, with a possibility to browse musical events by genre, by artist, and select the timeframe.

##Team
Virgile Neu (@virgileNeu), IN  
Alexandre Connat (@AlexConnat), SC  
Simon Narduzzi (@Narduzzi), IN

##Final Deliverable
###Pipeline
<img src="./Resources/pipeline.png" width="400">
####Scraping
First we scraped the data from three different websites : <a href="http://events.ch">events.ch</a>, <a href="http://residentadvisor.net">residentadvisor.net</a> and <a href="http://routesdesfestivals.com">routedesfestivals.com</a>.
We gathered like this over 40'000 musical events in Switzerland. The format of each website was different so we had to do specific scraping scripts for each of these websites. They also had different way of representing information, and no consistency. Which has created some problems we had to resolve before the merge, for instance the format of the date, we agreed to use the YYYY-MM-DD format (with dashes and not spaces or colons). Some data sources also had more information than others, events.ch sometimes contains the genre of the event, and resident advisor displays club names.

Then we merge this three dataframes into one big datagrame, making sure they all have the right columns and columns names.

Then we extracted the geolocation information, the (latitude, longitude) tuple to be able to plot it on the map. For events.ch and routesdesfestivals.com we only had the city name (location), for resident advisor we had the address of the event, more or less complete and well written.

After this, we want to get the musical genre of the events. For this purpose, we made use of Spotify and Wikipedia, but we needed the correct name of the artists. For that, we clean the artists column to make sure nothing phony is in it (some data from RA had the hours and other special characters in it). Then we can finally extract the genre per artists, then we compute the main genre per events.

Now the Events Dataframe is ready.

We also want to have a desaggregated artists, that is every line of the dataframe is a unique artists/events/date tuple, with also the genre of the artist. For this we use the desaggregate script.

We now have the two dataframe we want and need for the visualisation in Tableau.

###Visualization
We first wrote a app using matplotlib, it was ugly and a lot of code but worked (see `testVisualisation.py` in `Scrapping/events.ch/.old_visualisation/testVisualisation.py`). We also tried to draw the music map in Folium, and display it in a Web Browser, but it wasn't "smooth" enough, and kind of slow. Then we heard about the Tableau software for data visualiztion. We tested and adopted it because it was much faster, cleaner and easier to adapt than the matplotlib + basemap script we wrote before.

The Tableau projet files are in the `Visualization` folder.

<img src="./Visualization/Screenshots/1.png" width="400">
<img src="./Visualization/Screenshots/3.png" width="400">

The public Tableau is accessible on <a href="https://public.tableau.com/profile/publish/ADA_Project/Histoire1">this link</a> !

___
Below is the old description/README we made before starting the project : <br>
(to see what were our expectations)
___

<img src="./Resources/LogoADA.png" width="300">


##Abstract
> Music is an art form and cultural activity whose medium is sound and silence, which exist in time. - Wikipedia  

Music is a way to express and share. It brings people together around a common focus. In ancient Greece, Music is the most beautiful, and respected of all Art forms. Along with Science, it is the object of high philosophical speculation. The Greeks are indeed the first people who have established real concerts, which contributed to the developpement of a socially conscious public actively participating in the hearing.

Through the ages, Music has greatly evolved, some styles of music fell into disuse, while other genres emerged. However, the manner we celebrate and listen to music has remained the same : Concerts. <br>

The goal of this project is to study the dynamics of music in Switzerland in recent decades, by analysing the evolution of genres and audience population in concerts and major musical events.


##Data Description
###List of concert places

A first approach will be to use existing event websites to grab informations about the musical scene in Switzerland. We plan to get a list of all locations were we can attend a concert. Using the plateform Resident Advisor, we can a good list of clubs and concerts hall in Switzerland. The website contains all line-ups of the concerts that have been announced through RA since 2006, giving us approximately 10 years of data. As the plateform is mainly electronic music oriented, the data will be mostly related to Techno scenes and Electro clubs.

###Line-ups of Festivals / Concerts

We need to find several website of festival in Switzerland. We will build a dataset of all festival since 1980 *(Montreux Jazz, Verbier Festival, Paleo Festival, ...)*, and try to get the line-up for each of them. Of course, it will not be possible to get data for 36 years, as some festival just got created 10 or even 5 years ago. But we will try to get as much data as we can in order to perform analysis on genre and attendance of the festival. We will grab the line-up from official websites of festivals, and maybe other plateform, such as Facebook public events.

###Genres Analysis

Once we have the line-up of the concerts and festival, we can link the name of the artist to genre of music. There is a lot of genre possible, we need to select "main genres" that we can link to the data. We will perform a "merge" of subgenres, *i.e : Hard Techno and Hardstyle = Electronic Music*. If we have the time, we will try to divide our analysis in subgenres, but we will maybe not have enough data to have a viable statistical basis. The goal is to be able to detect rise and fall of music genre in Switzerland across the years.

We will mainly use a Wikipedia bot that will extract genre from artist name. The main challenge will be to try to get the genre of an artist if it does not have a Wikipedia biography. We might want to take a look on the Million Song dataset to extract missing information.

###Artists Analysis

Using Wikipedia (and other biography pages), we can get the "profile" of an artiste. We can analyse its discography to observe genre change, and try to compare the popularity of an artist with the "trend" at that time in Swtizerland (hit parade, artist reputation).

Using the list of artist, we can also perform an analysis of the "foreign" rate of music : Has Switzerland always invited foreign musicians ? Do we observe a rise of local artists in big festivals ? <br>
An other question being : What is the success of Swiss artists abroad ?

##Feasability and Risks

We already know that some data exists for some location, for instance with the new Montreux Jazz Museum in Art Lab we have access to all the artists that performed in the Montreux Jazz Festival for the last 50 years. We should be able to scrap enough data to at least cover the Vaud canton. The risks are the lack of other data or the time it will take to scrap it all the from various festival and concert room websites.  
We also plan to use the Twitter database or the Facebook API to grab some more data, tweets or comments on musical events to have more inputs in case we don't find as much information as required to have a precise and accurate representation.
We can also try to contact online tickets sellers (like ticketcorner or fnac) to see if they agree to give us some data. This would be a last attempt if we are desparatly in lack of data.  
We will focus on the tools learned in the first labs and homeworks using Python, numpy and Panda's dataframes, and using Javascript for visualizing data on interactive maps. The major goal is to obtain a big set of data, and we will use all the tools we can to have it. We will to merge the data from different website and plateform in a unique, comparable dataset on which we will perform our analysis. Data Scrapping and Vizualisations will be the two main aspect we will focus on during this project.

##Deliverables

The project with be released with vizualisations using Javascript Vizualisation libraries like Bokeh or D3JS. We will create multiple interactive maps that will contain the following information :

###Attendance Map
The attendance map is a heat-map containing the medium attendance of concert place for each year. The idea is to display the evolution of genre and popularity of festivals in Switzerland (Montreux Jazz, Paléo, Frauenfeld..), as well as the evolution of concert places : number of public institutions per city, size, etc.

The resulting map should look like this :

![Image of Crime Data form Alastaira](https://alastaira.files.wordpress.com/2011/02/image24.png)

###Genre evolution
We can use attendance map to display the number of events of a certain genre in Switzerland. A heat map of colors symbolizing the genre of music will be displayed, and a slider under the map will allow the user to observe the evolution of music in Switzerland across time. Some side options will be available : select only one genre to display (observing the evolution of only one genre), compare two genres, and a "play" button to automatically observe the dynamic of the genres.

This map should look like the following picture, each color representing a genre. We can observe the evolution of popularity of a genre in each region. 

![](http://www.zw3b.fr/var/contents/links/59/imgs/internet-census-2012.gif)
>IPv4 usage observed using ICMP ping requests, source : Carna Botnet

###Artist popularity / Artist genre evolution
Using data from the line-up, wikipedia or even twitter, we will be able to observe the popularity of a certain artiste. A search bar will be available to the user to perform search on the artist he/she wants. Using the interactive map, we can observe the evolution of an artiste across the time, if he/she came to Switzerland several times in a year, etc.

More ideas can be explored, such as displaying an piechart of genres of an artist.

##Timeplan

We hope to achieve the end of scraping by the checkpoint mid december or at most by the end of december. We will first need to get all the website we need, and then create bots that will automatically grab the data for us. Storing all data in CSV or only parsing HTML, we will next create dataframes that will resume all data. 
By the end of december we should have got all data from RA, festivals and wikipedia artist pages.

The next step will be to focus on the visualization (maps). That point shouldn't take more than two weeks. We will try to spare time in order to grab more data or offer more detailed visualisation or more different views on the maps. 

This project is consequent and a planning is difficult to predict, as we are not very aware of the data that are available at the moment. Maybe we will discover interesting informations constructing our dataframe, and discovering more possible comparison. But displaying the result can take a lot of time, that's why we will first develop the two points mentionned above before trying techniques such as Machine Learning on our data to discover new features.

- Week 1 (7.11.2016) : Data Search / Data Scrapping for RA  
- Week 2 (14.11.2016) : Data Search / Data Scrapping for other plateform  
- Week 3 (21.11.2016) : Data Search / Data Scrapping for other plateform  
- Week 4 (28.11.2016) : Data Search / Data Scrapping for other plateform (Twitter, Facebook)  
- Week 5 (5.11.2016) : Wikipedia artist extractor development / Data Analysis  
- Week 6 (12.11.2016) : Wikipedia artist extractor development / Data Analysis  
- Week 7 (19.11.2016) : Wikipedia artist extractor development  
- Week 8 (26.11.2016) : Vizualisation developpement  
- Week 9 (2.11.2016) : Vizualisation developpement  
- Week 10 (9.11.2016) : Vizualisation developpement  
- Week 10 (16.11.2016) : Vizualisation developpement  

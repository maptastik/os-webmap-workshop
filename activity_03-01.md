#Part 3, Activity 1
##Make an interactive choropleth map

###Introduction
In this activity you'll be using CartoCSS and TileMill to make a choropleth map of the Hungarian descended population in Lucas County Ohio by census tract. Your map will also include a legend and interactivity that informs a visitor to your map about the total and percentage of the Hungarian descended population. At the end you will export your map to mbtiles and upload it to Mapbox.

###What you will need
- [TileMill](https://www.mapbox.com/tilemill/)
- [Mapbox account](https://www.mapbox.com/signup/) (the free plan will be sufficient)
- Census tract level data for Lucas County, OH on Hungarian ancestry. (If you cloned the os-webmap-workshop repo you can find i your local copy at **/data/prepared/lucasCo_hungarian_tracts.geojson**. Otherwise, you get it [here](https://raw.githubusercontent.com/maptastik/os-webmap-workshop/gh-pages/data/prepared/lucasCo_hungarian_tracts.geojson))

###Let's make a map!
1. Open TileMill
2. You'll see the *Projects* window. Click  **+ New Project**<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-projects1.png" width=50% />
3. You should see the *New project* window. Fill out the **Filename** field. Also uncheck **Default data**. We don't need the world layers that TileMill provides. Still, it's good to know they're there! If you want to fill in **Name** or **Description** fields you may, but it's not necessary. When you're finished, click **Add**.<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-projects2.png" width=50% />
4. In the *Projects* window, select the project you just made.
5. TileMill will open up your project. It has by default added a light blue background to your project. That's not a layer of data! It's just a style for the space you'll be putting your data into!<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map1.png" width=50% />
6. Go ahead and just delete the default CartoCSS:<br/>`Map {
  background-color: #b8dee6;
}`
7. You should see a gridded background. That means you have no actual background anymore. That's good! It will allow us to eventually add our map on top of other maps.
8. Let's add our data. In the bottom-left corner of your window you should see a vertical stack of four buttons. Each one opens up a lot of TileMill's functionality, but we're most concerned with getting our data into TileMill. As such, click the bottom-most button.<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map2.PNG" width=10%/>
9. This will open up a little *Layers* window. Since we haven't added anything to our project, there aren't any layers shown. Let's change that! Click **+ Add Layer** <br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map3.PNG" width=100% />
10. You should see the *Add Layer* window. Fill in the **ID** field with a short name for the layer. You can name it whatever you want, but it's helpful to name your layer something that describes what it is. (I opted for *tracts*). Select lucasCo&#95;hungarian&#95;tracts.geojson dataset wherever you have it saved. Leave everything else as it is and click **Save &amp; Style**<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map4.PNG" width=100% />
11. Alright! You've loaded in your data. You'll see some default cartoCSS for our tracts layers has been loaded into the style.mss pane on the right. Also note that in the bottom-left in the *Layers* window, we now have our tracts layer listed. You may initially not be able to the actual map of our tracts. In the *Layers* window, to the right of #tracts is a magnifying glass icon. Click that and the view will zoom to our tracts<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map5.png" width=100% />
12. Let's take a look at the data? What are we going to map? In the *Layers* window, to the right of #tracts is a table icon. Click it and you should see the attribute table of the tracts data. We've got too attribute fields that were joined from ACS Census data. **t_hung** is total population in the tract claiming Hungarian ancestry. **p_hung** is the percentage of the population in the tract that claims Hungarian ancestry. Because we're making a choropleth map, it's best to use normalized data. We'll be working with the **p_hung** field for this map<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map6.png" width=100% />
13. In the stylesheet pane, we have our default styling for our tracts layer:<br /><br />`#tracts {
  line-color:#594;
  line-width:0.5;
  polygon-opacity:1;
  polygon-fill:#ae8;
}`<br /><br />These style description give the map it's current look, but there are many properties of the polygon's fill and outline that can be edited. You can access the built-in CartoCSS reference by click the curly-brace button on that vertical stack of buttons mentioned earlier. Additionally, Mapbox includes the [reference](https://github.com/mapbox/carto/blob/master/docs/latest.md) on their GitHub page along with [several examples](https://www.mapbox.com/tilemill/docs/crashcourse/styling/) in the TileMill documentation on their website.<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-ref1.png" width=100% />
14. Let's change some properties. Change:<br /><br />`line-color:#594;` to `line-color:#000000;`<br />and<br />`polygon-fill:#ae8;` to `polygon-fill:#2980b9;`.<br /><br />Click **Save**. If all is correct, you should have a map of census tracts in Lucas County, OH with a blue fill and black borders.<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map7.png" width=100% />

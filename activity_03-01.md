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
5. TileMill will open up your project. It has by default added a light blue background to your project. That's not a layer of data! It's just a style for the space you'll be putting your data into!<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map2.png" width=50% />
6. Go ahead and just delete the default CartoCSS:

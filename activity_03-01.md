# Part 3, Activity 1
## Make an interactive choropleth map

### Introduction
In this activity you'll be using CartoCSS and TileMill to make a choropleth map of the Hungarian descended population in Lucas County Ohio by census tract. Your map will also include a legend and interactivity that informs a visitor to your map about the total and percentage of the Hungarian descended population. At the end you will export your map to mbtiles and upload it to Mapbox.

### What you will need
- [TileMill](https://www.mapbox.com/tilemill/)
- [Mapbox account](https://www.mapbox.com/signup/) (the free plan will be sufficient)
- Census tract level data for Lucas County, OH on Hungarian ancestry. (If you cloned the os-webmap-workshop repo you can find i your local copy at `/data/prepared/lucasCo_hungarian_tracts.geojson`. Otherwise, you get it [here](https://raw.githubusercontent.com/maptastik/os-webmap-workshop/gh-pages/data/prepared/lucasCo_hungarian_tracts.geojson))

### Let's make a map!

#### Getting started

Open TileMill

You'll see the *Projects* window. Click  **+ New Project**<br /><img src="https://maptastik.github.com/os-webmap-workshop/images/tm-projects1.png" width=50% />

You should see the *New project* window. Fill out the `Filename` field. Also uncheck `Default data`. We don't need the world layers that TileMill provides. 

When you're finished, click **Add**.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-projects2.png" width=50% />

In the *Projects* window, select the project you just made.

#### Setting up your workspace

TileMill will open up your project. It has, by default, added a light blue background to your project. That's no layer of data! It's just a style for the space you'll be putting your data into.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map1.png" width=50% />

In the right hand pane is a text area. There's a little tab denoting that this is `style.mss`. This is where we'll be adding CartoCSS to style our map.

Go ahead and just delete the default CartoCSS:

````
Map {
  background-color: #b8dee6;
}
````
Click **Save**.

You should see a gridded background. That means you have no actual background anymore. That's good! It will allow us to eventually add our map on top of other maps.

#### Adding data

In the bottom-left corner of your window you should see a vertical stack of four buttons. Each one opens up a lot of TileMill's functionality, but we're most concerned with getting some data into TileMill. As such, click the bottom-most button.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map2.PNG" width=10%/>

This will open up a little *Layers* window. Since we haven't added anything to our project, there aren't any layers shown. Let's change that! Click **+ Add Layer**

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map3.PNG" width=100% />

You should see the *Add Layer* window. Fill in the `ID` field with a short name for the layer. It can be whatever you want, but it's helpful to name your layer something that describes what it is. (I opted for `tracts`). 

Select the `lucasCo_hungarian_tracts.geojson` dataset wherever you have it saved. Leave everything else as it is and click **Save &amp; Style**

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map4.PNG" width=100% />

Alright! You've loaded in your data. You'll see some default cartoCSS for our tracts layers has been loaded into the style.mss pane on the right. Also note that in the bottom-left in the *Layers* window, we now have our tracts layer listed. You may initially not be able to the actual map of our tracts. In the *Layers* window, to the right of #tracts is a magnifying glass icon. Click that and the view will zoom to our tracts.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map5.png" width=100% />

#### Examining the data

Let's take a look at the data. What are we going to map? In the *Layers* window, to the right of `#tracts` is a table icon. Click it and you should see the attribute table of the tracts data. We've got two attribute fields that were joined from ACS Census data. 

- `t_hung` is total population in the tract claiming Hungarian ancestry. 

- `p_hung` is the percentage of the population in the tract that claims Hungarian ancestry. 

#### Some simple styling

Because we're making a choropleth map, it's best to use normalized data. We'll be working with the `p_hung` field for this map.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map6.png" width=100% />

In the stylesheet pane, we have our default styling for our tracts layer:

`#tracts {`
  <br>&nbsp;&nbsp;&nbsp;&nbsp;`line-color:#594;`
  <br>&nbsp;&nbsp;&nbsp;&nbsp;`line-width:0.5;`
  <br>&nbsp;&nbsp;&nbsp;&nbsp;`polygon-opacity:1;`
  <br>&nbsp;&nbsp;&nbsp;&nbsp;`polygon-fill:#ae8;`
<br>`}`

These style description give the map it's current look, but there are many properties of the polygon's fill and outline that can be edited. You can access the built-in CartoCSS reference by click the curly-brace (`{}`) button on that vertical stack of buttons mentioned earlier. Additionally, Mapbox includes the [reference](https://github.com/mapbox/carto/blob/master/docs/latest.md) on their GitHub page and [several examples](https://www.mapbox.com/tilemill/docs/crashcourse/styling/) in the TileMill documentation on their website.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-ref1.png" width=100% />

Let's change some properties. Change:

- `line-color:#594;` to `line-color:#000000;`
<br />and<br />
- `polygon-fill:#ae8;` to `polygon-fill:#2980b9;`
 
Click **Save**.

If all is correct, you should have a map of census tracts in Lucas County, OH with a blue fill and black borders.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map7.png" width=100% />

Not too shabby! Way better than clicking through tons of dialog boxes. But this is a pretty boring map. Let's map those Hungarians!

#### Styling based on data
With CartoCSS we can assign styles to features that meet certain criteria. This is called *conditional formatting*. In this case we're going to apply conditional formatting to the fill of the tracts based on the `p_hung` field values.

We're going to need to classify our `p_hung` field to create our choropleth map. TileMill is not a GIS and thus does not generate classification schemes based on your data. You'll have to do that in QGIS, ArcGIS, or by some other means. For the sake of this activity, I've created a classification scheme you can use.
####### Classification Scheme

`Class 1: <1`
<br>
`Class 2: 1-3.9`
<br>
`Class 3: 4-6.9`
<br>
`Class 4: 7-9.9`
<br>
`Class 5: >=10`

You may, of course, create your own.

We're almost ready to apply this classification scheme to the map. But first, it would be good to get a color scheme. Rather than make one up, let's use [ColorBrewer2](http://colorbrewer2.org/). You have a lot of freedom here to pick your colors, but make sure that under the **Nature of your data** you select `sequential`. This will help ensure that you select a color scheme appropriate for the data.

Also, make sure to switch from HEX to and RGB colorspace. TileMill can handle both, but it handles transparency better with RGB.

###### Color Scheme

`Class 1: 255,255,204`
<br>
`Class 2: 194,230,153`
<br>
`Class 3: 120,198,121`
<br>
`Class 4: 49,163,84`
<br>
`Class 5: 0,104,55`

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-cb1.png" width=100% />

Let's make this choropleth happen! Go ahead and just delete:

`polygon-opacity:1;`
<br>
`polygon-fill:#2980b9;`

We don't need them as they are.

We can now apply our conditional formatting. Let's try it by just applying fill to those tracts with less than 1% of the population claiming Hungarian ancestry. Underneath `line-width: 0.5` add:

`[p_hung<1] {polygon-fill: rgb(255,255,204);}`

Click **Save**.

If there are no syntax errors, you should see that only a few census tracts have been filled in. The rest are hollow because they have no polygon-fill value at the moment. We'll change that next.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map8.png" width=100% />

Let's add the rest of our classes with the appropriate colors following the model from the last step:

`[p_hung>=1] {polygon-fill: rgb(194,230,153);}`
<br>
`[p_hung>=4] {polygon-fill: rgb(120,198,121);}`
<br>
`[p_hung>=7] {polygon-fill: rgb(49,163,84);}`
<br>
`[p_hung>=10] {polygon-fill: rgb(0,104,55);}`

Click **Save**. 

If all went as planned and there are no errors, you should see a choropleth map based on the classification scheme defined earlier.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map9.png" width=100% />

#### Adding transparency

This is a pretty nice looking choropleth map, but there's no figure to ground relationship or geographic context surrounding Lucas County. We could add additional state and county layers to create that context, but we can also place this choropleth map on top of a basemap. Although adding transparency to choropleth maps is a big no-no, in our next activity we'll be placing this map over a basemap well-suited for such a cartographic deviance.

To get that transparency in our polygon fill, we need to make a slight adjustment to our polygon-fill specifications. We can adjust transparency in RGB colors through the alpha channel, but at the moment our style doesn't address transparency. Our fill is totally opaque. To adjust that alpha channel is quite simple. Instead of `polygon-fill: rgb(...)` we need to use `polygon-fill: rgba(...)`. 

The **a** in rgba is our alpha channel and it allows us to add a fourth value in our color specification. We can select a number, 0 (totally transparent) - 1 (totally opaque), to adjust the opacity of the fill. Let's use 0.5. 

Your polygon fill formatting should look something like this:

`[p_hung<1] {polygon-fill: rgba(255,255,204,0.5);}`
<br>
`[p_hung>=1] {polygon-fill: rgba(194,230,153,0.5);}`
<br>
`[p_hung>=4] {polygon-fill: rgba(120,198,121,0.5);}`
<br>
`[p_hung>=7] {polygon-fill: rgba(49,163,84,0.5);}`
<br>
`[p_hung>=10] {polygon-fill: rgba(0,104,55,0.5);`

Click **Save**

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map10.png" width=100% />

#### Adding Interactivity

TileMill allows you to add some limited interactivity to you map. Primarily this means you can create info windows with additional information pulled form your data. For our map, we'll add an info window that tells the user the total and percentage of the population that is Hungarian descended as well as the census tract number.

On the vertical stack of buttons in the bottom-left corner, click the pointing hand. This will open up the *Templates* window. Here you have options to add a legend, hover teaser, click function, and link to external resources. Go ahead and click **Teaser**.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tp-tease1.png" width=100% />

There is a dropdown menu currently set at `--disabled--`. This dropdown lets us determine what layer we're going to call on for interaction. Select `tracts`. You'll see the gray are abelow filled with the various attributes of from our tracts data surrounded by triple curly braces (Mustache tags). 

<img src="https://maptastik.github.com/os-webmap-workshop/images/tp-tease1.png" width=100% />

In the content area above we can combine plain text or HTML with our mustachioed attributes. Let's use `{{{p_hung}}}`, `{{{t_hung}}}`, and `{{{NAME}}}` along with some HTML:

`There are <strong>{{{t_hung}}}</strong> residents (<strong>{{{p_hung}}}%</strong> of population) in tract <strong>{{{NAME}}}</strong> of Lucas County claiming Hungarian ancestry.`

Click on the **Full** button and the same text to that content area. By adding this text in both **Teaser** and **Full** we can see our info window text when we hover or click a feature.

Click **Save**.

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map11.png" width=100% />

#### Add legend

You can create a legend using HTML. Open up the *Templates* window and select **Legend** and add the following HTML:


`<div class='my-legend'>`

`<div class='legend-title'>Hungarian Ancestry in Lucas County, OH (by census tract)</div>`

`<div class='legend-scale'>`
<br>
`<!--You can adjust background:rgba(...) and the values between </span> &amp; </li> to reflect your color and classification schemes-->`
<br>
`<ul class='legend-labels'>`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`<li><span style='background:rgba(255,255,204,0.5);'></span><1%</li>`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`<li><span style='background:rgba(194,230,153,0.5);'></span>1% - 3.9%</li>`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`<li><span style='background:rgba(120,198,121,0.5);'></span>4% - 6.9%</li>`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`<li><span style='background:rgba(49,163,84,0.5);'></span>7% - 9.9%</li>`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`<li><span style='background:rgba(0,104,55,0.5);'></span>>= 10%</li>`
 <br>`</ul>`
<br>`</div>`
<br>
`<!--You can change the text between the various tags to reflect and link to your data source-->`
<br>`<div class='legend-source'>Source: <a href="#link to source">ACS 2012 5yr</a></div>`
<br>`</div>`
<br>
<br>
`<!--This the general styling info for your legend. You can change any of this to suit your needs-->`
<br>
`<style type='text/css'>`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`.my-legend .legend-title {
    text-align: left;
    margin-bottom: 8px;
    font-weight: bold;
    font-size: 90%;
    }`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`.my-legend .legend-scale ul {
    margin: 0;
    padding: 0;
    float: left;
    list-style: none;
    }`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`.my-legend .legend-scale ul li {
    display: block;
    float: left;
    width: 50px;
    margin-bottom: 6px;
    text-align: center;
    font-size: 80%;
    list-style: none;
    }`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`.my-legend ul.legend-labels li span {
    display: block;
    float: left;
    height: 15px;
    width: 50px;
    }`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`.my-legend .legend-source {
    font-size: 70%;
    color: #999;
    clear: both;
    }`
<br>&nbsp;&nbsp;&nbsp;&nbsp;`.my-legend a {
    color: #777;
    }`
<br>`</style>`

Click **Save**.

You have completed styling the map in TileMill!

<img src="https://maptastik.github.com/os-webmap-workshop/images/tm-map12.png" width=100% />


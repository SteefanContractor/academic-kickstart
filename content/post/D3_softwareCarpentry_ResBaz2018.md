+++
title = "ResBaz 2018 - Visualising data on the web with D3"
date = 2018-07-08T06:07:57+03:00
draft = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = ""
preview = true

+++

<iframe src="https://steefancontractor.github.io/ResBazD3" width="130%" height="650"></iframe>

I created the above [web based visualisation](https://steefancontractor.github.io/ResBazD3/) during a workshop on D3.js at [ResBaz 2018](https://resbaz.github.io/resbaz2018/sydney/). Here, you can read about my ResBaz experience and learn about how you can create a scatterplot like this too! 

## ResBaz 2018

On 4th July I participated in ResBaz 2018, an excellent conference for building and sharing research skills as well as networking. It is a fantastic platform for cross-disciplinary information and skill exchange. Plus the food was excellent which was an unexpected bonus (I later found out that they spent 90% of their budget on food!). I highly recommend ResBaz to other early career researchers. This year ResBaz was held in Sydney at the Macquarie University. 

During the first two days of the conference attendees participate in various workshops and the third day is focussed on networking. I enrolled into the Advanced Python Workshop, the second day of which had nothing to do with Python, but instead, was focussed on a javascript framework for producing interactive visualisations on the web called D3.js. I have always been interested in picking up D3, since I think it is a great tool for publishing visualisations on the web! The resulting content can be static web pages and do not require servers. However, because of the fine control of every graphic element that D3 offers, it is not useful for creating quick and dirty visualisations. If your aim is to create an interactive plot to explore your data, then I think D3 is not the right tool for it. In such cases you are better off using API services like R Shiny which provide less fine control of the various plot elements but is much quicker to code as a result. Although I had done a brief web course on D3 in the past, I did not feel confident creating various plots in D3 until after this hands-on workshop.

The purpose of this post is to showcase the web based scatterplot created during this workshop and to give a basic overview of what D3 is and how you can get started with it. If interested, one can go through the [Software carpentry D3 course](https://mq-software-carpentry.github.io/D3-visualising-data/) for step by step instructions on the entire project. 

## First steps - setting up the web page with HTML and CSS

So the first thing to understand is that D3 is a javascript library for creating web-based visualisations. This means that we need a web page to display the visualisation we create with D3.js. In our case, we created a very simple web page (shown below) using HTML and CSS that we hosted on github pages.

```html
<!DOCTYPE HTML>

<html>
<head>
	<!-- link the css stylesheet and load the fonts from google fonts -->
	<link rel="stylesheet" type="text/css" href="main.css">
	<link href="https://fonts.googleapis.com/css?family=Pacifico" rel="stylesheet">
</head>
<body>
	<!-- Add a heading -->
	<h1>The Wealth and Health of Nations</h1> 
	<!-- Create a div box for the actual chart and give it an id so we can refer to it in the js -->
	<div id="chart"></div>
	<!-- create a div box for the checkboxes and again give it an id -->
	<div id="controls"></div>

	<!-- We only load the d3.js library and main.js (our code containing the js code) after the  -->
	<!-- div boxes with hte appropriate ids are defined in the html to avoid a reference to an   -->
	<!-- object -->
	<script src="https://d3js.org/d3.v5.min.js"></script>
	<script src="main.js"></script>
</body>
</html>
```

```css
.svg {
	margin-left: 700px;
	width: 50px;
	height: 50px;
}

/*look up chrome.js for colour definitions*/
.circle {
	stroke-width: 3px;
	stroke: black;
	/*fill: #7570b3;*/
}

.label {
	font-size: 16px;
}

.checkbox {
	margin-left: 80px;
}

body {
font-family: 'Pacifico', cursive;
}
```

For those who are not familiar with web development, HTML defines the structure of the web page, so in our case our web page consists of a heading and then a box that will contain the scatterplot and then another box that will contain a bunch of checkboxes to allow us to select/deselect certain regions. However the style of these elements (heading and div boxes) are set by the css. This means that if we would like to, for example, change the position of an element in a web page, or add a border to a div box or change the font of the text in the body, we do this in the css code. 

So now that our web page is set up, we can start writing the actual javascript to create the plot. The final javascript code is shown below. I will not explain every single element, but instead a few key elements that I hope would make the code easier to follow. 

## Writing the JS code to create the plot

```js
// the .then means the function is called only after the json file is downloaded
var dataURL = 'https://raw.githubusercontent.com/MQ-software-carpentry/D3-visualising-data/gh-pages/code/nations.json';
d3.json(dataURL)
	.then( function (nations) {
		console.table(nations);

		var chart = d3.select('#chart');
		var svg = chart.append('svg');

		var g = svg.append('g');
		var margin = { top: 20, left: 80, bottom: 50, right: 20 };
		var width = 960;
		var height = 350;
		var g_width = width - margin.left - margin.right;
		var g_height = height - margin.top - margin.bottom;

		svg.attr('width', width)
		svg.attr('height', height)

		g.attr('transform', 'translate(' + margin.left + ',' + margin.top + ')')

		var incomes = [];
		var lifeExpectancies = [];
		var populations = [];
		nations.map( function (n) {
			incomes = incomes.concat(n.income);
			lifeExpectancies = lifeExpectancies.concat(n.lifeExpectancy);
			populations = populations.concat(n.population);
		});
		var xExtent = d3.extent(incomes);
		var yExtent = d3.extent(lifeExpectancies);
		var zExtent = d3.extent(populations);

		var regions = [
	      "Sub-Saharan Africa",
	      "South Asia",
	      "Middle East & North Africa",
	      "America",
	      "Europe & Central Asia",
	      "East Asia & Pacific"
    	];

    	var controls = d3.select('#controls');
    	regions.map( function (r) {
    		var div = controls.append('div');
    		div.append('input')
    			.attr('type', 'checkbox')
    			.attr('class', "checkbox")
    			.attr('value', r)
    			.attr('checked', true);
    		div.append('label')
    			.text(r);
    	});

    	var filtered_nations = nations.map(function(n) {return n;});

    	d3.selectAll('input').on('change', function () {
    		var region = this.value
    		if (this.checked) {
    			var new_nations = nations.filter (function (n) {
    				return n.region == region;
    			});
    			filtered_nations = filtered_nations.concat(new_nations);
    		} else {
    			filtered_nations = filtered_nations.filter( function (n) {
    				return n.region != region;
    				// console.log(n.region, region)
    			})
    			// console.log(filtered_nations);
    		}	
    		update();
    	});


    	var colorScale = d3.scaleOrdinal(d3.schemeCategory10);

		console.log(xExtent)

		var xScale = d3.scaleLog()
						.domain(xExtent)
						.range([0, g_width]);
		var xAxis = d3.axisBottom(xScale)
						.ticks(10, ',.0f');
		g.append('g')
			.attr('class', 'x axis') // two classes added; x and axis class
			.attr('transform', 'translate(0,' + g_height + ')')
			.call(xAxis);

		var yScale = d3.scaleLinear()
						.domain(yExtent)
						.range([g_height, 0]);
		var yAxis = d3.axisLeft(yScale);
		g.append('g')
			.attr('class', 'y axis')
			.attr('transform', 'translate(0,' + 0+ ')')
			.call(yAxis);

		var rScale = d3.scaleSqrt()
						.domain(zExtent)
						.range([0, 40]);

		g.append('text')
			.attr('class', 'label')
			.text('Income per capita (dollars)')
			.attr('x', g_width/2)
			.attr('y', g_height + 40)


		g.append('text')
			.attr('class', 'label')
			.text('Life Expectancy (years)')
			.attr('transform', 'translate(-40,' + (g_height/2) + ') rotate(-90)')
			.attr('x', -g_height/4);

		function update() { 
			var circles = g.selectAll('.circle')
							.data(filtered_nations, function(d) {return d.name;});

			circles.enter()
				.append('circle') //html tag that happens to be the same name
				.attr('class', 'circle') //the class needs to be the same as the selectALL class above
				.attr('cx', function(d) {return xScale(d.income[d.income.length-1])})
				.attr('cy', function(d) {return yScale(d.lifeExpectancy[d.lifeExpectancy.length-1])})
				.attr('r', function(d) {return rScale(d.population[d.population.length-1])})
				.attr('fill', function(d) {return colorScale(d.region)});

			circles.exit().remove();
		}
		update();

	}).catch( function (err) {
		console.error(err)
	});
```

Ok, the indenting did not preserve after copying the code into the markdown but the basic idea is this - we read the raw data from the url (dataURL). Then we call the d3 library's json function to read this data. We use a ".then" clause because we do not wish to run the rest of the code unless the data has finished being read in. The output of the json(dataURL) is fed into a function inside the .then clause. We can now assume nations contains all our data and treat it like any other variable. Inside this function (which is inside the .then) we set up all the attributes for our chart, including margin, width, height etc. The basic concept behind how HTML elements can be manipulated using javascript is this, we can select elements using their IDs and then add new tags to them using .append() and add new attributes (such as class, transform etc.), to the tags using .attr().

One of the things we have done repeatedly in this js code is looping over our data. This is done with the function map. So whenever you encounter "nations.map( function (n) )", I am saying *loop over the table nations with "n" representing each row of the table nations*.

The way we set up the checkboxes in the javascript is also very interesting. We first select the div with ID "controls", and create a new div inside this div for each checkbox. Inside each one of these "checkbox" divs we create another tag "input" with the attribute *type="checkbox"*. To select/deselect data that is fed into our plotting function, we use d3.selectALL() to select all elements of class "input" and then use the .on() clause on these elements to do "something" everytime these elements change (i.e. get checked or unchecked). 

After the checkboxes are created we create the axes for the plot. We create y-axis on a log-scale, and the x-axis on a linear scale and add labels to these axes. 

Finally we write a function called update(). All the circles are created inside this function. We need to do this inside a function because everytime we check or uncheck a region checkbox, we filter the data accordingly, and then we would like to replot our circles accordingly. So note that back in our code where we select all checkboxes (elements with class "input"), each time a checkbox is checked or unchecked, we call this update() function to update the plot based on the filtered dataset. I would also like to draw your attention to how we associate each circle with the income, lifeExpectancy, population and region columns of our data inside the update function. To do this we feed the income, lifeExpectancy, population and region data for a particular point to the x position ("cx"), y position ("cy"), radius ("r") and fill colour ("fill") attributes of our circles. 

I do not expect most people who have never looked at HTML, CSS or Javascript code to understand what is happening based on just this blogpost. The aim here was to just show you what D3.js is capable of and showcase some interesting and powerful programming concepts that I was not familiar with before trying out D3.js. If you feel inspired please go through the [step by step instructions](http://mq-software-carpentry.github.io/D3-visualising-data/) on creating the plot yourself. The best way to progress from their would be to go to [d3js.org](https://d3js.org) and check out the numerous examples of the various plots you can make with D3. Note also that Chrome and Firefox have amazing developer tools available, which means that while writing the code one can use the *Inspect* tool (On mac: Cmd + Shift + C; On windows: Ctrl + Shift + C) in Chrome to see the HTML code related to various plot elements.

You can find my github repo [here](https://github.com/steefancontractor/ResBazD3), the live plot [here](https://steefancontractor.github.io/ResBazD3), the instructor's github [here](https://github.com/Spaxe/ResBazD3) and the original software-carpentry "D3 visualising data" repository [here](https://github.com/mq-software-carpentry/D3-visualising-data/). Also, a great reference for anything web development, including json, svg, css and javascript is the [MDM website](https://developer.mozilla.org/).

---
title: Dashboard Extension Tutorial
layout: docs
indexed_by_search: false
--- 

This tutorial walks you through the steps of creating and running a dashboard extension. The tutorial is based on the Marks Selection sample. 

This tutorial assumes you have downloaded or cloned the Extensions API repository. The code for the Marks Selection sample can be found in the `Tutorial` folder. You can either jump to that folder directly and check out the code, or you can follow the instructions on this page where we will walk through each step and create the files needed to build the extension. 

For information about setting up your environment and downloading Tableau Desktop for the Developer Preview, see [Get Started]({{site.baseurl}}/docs/trex_getstarted.html)

In this tutorial you will learn how to:

* TOC
{:toc}


## Set up the framework for a dashboard extension  

In this section, we will create the files that form the foundation of the extension. 

The files include the components of our web application:
- A simple HTML page, which includes jQuery and some other libraries. 
- A JavaScript file that will contain our code that uses the Extensions API library. 
- A simple `.css` file to make our HTML page look good.

And to register and identify the extension with Tableau:
- An extensions manifest file (`.trex`) that describes our dashboard extensions.


We will also launch a web server to host the HTML page on your computer. 
At the end of this first part, we'll be able to see our extension show up in Tableau, and load this basic html page inside a dashboard.

 
### Create a simple HTML page

This is the page that will appear inside the dashboard when you drag the extension onto the dashboard.

1. Create a new file called `MarksSelection-demo.html` and save it to the top-level directory in your local `extensions-api` repository. You can really save it anywhere you want. You just need to place it somewhere that you can host as a web page with a simple HTTP web server. 
2. Copy the following code into the file and save the file:

```html

<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- These two libraries are copied from the download builder here: https://datatables.net/download/ -->
    <link rel="stylesheet" type="text/css" href="https://cdn.datatables.net/v/bs-3.3.7/jq-3.2.1/jq-3.2.1/dt-1.10.16/b-1.4.2/b-colvis-1.4.2/cr-1.4.1/fh-3.1.3/r-2.2.0/sc-1.4.3/datatables.min.css"/>
    <script type="text/javascript" src="https://cdn.datatables.net/v/bs-3.3.7/jq-3.2.1/jq-3.2.1/dt-1.10.16/b-1.4.2/b-colvis-1.4.2/cr-1.4.1/fh-3.1.3/r-2.2.0/sc-1.4.3/datatables.min.js"></script>
    
    <title>MarksSelection-demo</title>

    <!-- Include our own style sheets and JavaScript code -->
    <link rel="stylesheet" href="./MarksSelection-demo.css">
    <script src="./MarksSelection-demo.js"></script>
  </head>
  <body>
    Hello Extensions!
  </body>
</html>

```
This file includes some libraries we will want later. It also includes references to the stylesheet and our JavaScript file `MarksSelection-demo.js` that we will be creating. 


### Create the `.css` file

1. Create a stylesheet called `MarksSelection-demo.css` and save it to the top-level directory in your local `extensions-api` repository (or the same directory where you saved your HTML page). 
 
2. Copy the following code into the file:

```html
h4, h5 {
  text-align: center;
}

div.DTS div.dataTables_scrollBody {
  background: white;
}

.sheet_name {
  font-weight: bold;
}

```

The `.css` file adds some formatting that will make the HTML page look better later. 


### Create the JavaScript file for our dashboard extension

1. Create a JavaScript file called `MarksSelection-demo.js` and save it to the top-level directory in your local `extensions-api` repository (or the same directory where you saved your other files). 
 
2. Copy the following code into the file:


```javascript

"use strict";

// Wrap everything in an anonymous function to avoid poluting the global namespace
(function() {

  // Use the jQuery document ready signal to know when everything has been initialized
  $(document).ready(function() {
    // Add your startup code here

  });
})();


```
The JavaScript is pretty simple. It just uses the jQuery document ready function so we can take action after the page loads. 


### Create a dashboard extensions manifest file (`.trex`)

The `.trex` file contains basic information like the name of the extension (as it will appear in Tableau) and the URL where the extension is hosted. To have the extension show up in Tableau, the `.trex` file must be in your `My Tableau Repository (Beta)/Extensions` folder.  

1. Create a file called `MarksSelection-demo.trex` and save it to `My Tableau Repository (Beta)/Extensions` folder. 
 
2. Copy the following code into the file:

```xml

<?xml version="1.0" encoding="utf-8"?>
<manifest manifest-version="0.1" xmlns="http://wwww.tableau.com/xml/addin_manifest">
  <tableau-addin id="com.tableau.extensions.dashboard.tutorial.markselection.demo" addin-version="0.1.0">
    <default-locale>en_US</default-locale>
    <name resource-id="name"/>
    <description>Demonstrates getting the selected marks and responding to user input</description>
    <addin-type>
      <dashboard-addin/>
    </addin-type>
    <author name="projectfrelard" email="projectfrelard@tableau.com" organization="tableau" website="www.tableau.com"/>
    <min-api-version>0.6</min-api-version>
    <source-location>
      <url>http://localhost:8765/MarksSelection-demo.html</url>
    </source-location>
    <icon>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</icon>
  </tableau-addin>
  <resources>
    <resource id="name">
      <text locale="en_US">MarksSelection-demo</text>
    </resource>
  </resources>
</manifest>


```

There is a lot going on here. But we only need be concerned with putting the file in the right location and a few elements. Some key pieces are:
-  The `<tableau-addin id>` element, which uses reverse domain name notation to uniquely identify the extension.
- The `<source-location>` element that specifies the URL of our hosted HTML page. For this tutorial, the address is `http://localhost:8765/MarksSelection-demo.html`. 
- The `<min-api-version>` element that specifies the minimum version of the Extensions API library that is required to run the extension.  

 

### Set up a web service to host the HTML page

For this tutorial, you need to start up a web server on your computer to host the HTML page. If you downloaded or cloned the Extensions API repository, you can start the web service in the root directory on your computer. You need to run the web server from the directory where you have saved your MarksSelection-demo files. 

- From the root of this directory start an http server on port 8765.
	- Python 2.x : `python -m SimpleHTTPServer 8765`
	- Python 3.x : `python -m http.server 8765`
	- Node.js : `npm install http-server -g && http-server -p 8765`



### Test it in Tableau

1. Close Tableau, if it is already running. Tableau just checks the `Extensions` folder at start up. 
2. Start Tableau and open a workbook that has a dashboard, or create a dashboard. 
3. In the dashboard, under **Extensions**, select the `MarksSelection-demo` you just created and drag it on to the dashboard. 
The HTML page should appear in the dashboard with the message: "Hello Extensions!"


## Add user interaction

Step 3. Get data

Step 4. Add event handling

Step 5. Save settings in the workbook




# Marks Selection Tutorial

This sample demonstrates a number of different aspects of the Dashboard Extensions API. The goal of this extension is to display a data table summarizing the selected marks on a particular worksheet on a dashboard (similar to clicking view data on a tooltip). When the selected marks change, we want to update the data table with the newly selected marks. If no marks are selected, we'll just show the data for every mark on the worksheet.

To make it easier to follow along, the tutorial is organized in four parts:

1. Initialization
2. Ask the User to Select a Sheet
3. Getting and Displaying the Data
4. Responding to Selection Changes
5. Persisting Settings in the Workbook
6. Performing Actions

### Part 1 - Initialization

The very first thing you need to do is to provide an extension, or manifest file (`.trex`). If you cloned or downloaded the `.zip` file for the TC17demo repository, you can find the sample `MarksSelection.trex` file in the `Extensions` folder. The `.trex` file contains basic information like the name of the extensions (as it will appear in Tableau) and the url where the extension is hosted. To have the extension show up in Tableau, you must copy the `.trex` file into your `My Tableau Repository/Extensions` folder and restart Tableau.  

The next step is to create an HTML page to host the extension. Our HTML page is called `MarksSelection.html` and it is quite basic. It includes some external JavaScript and styling libraries, the Tableau Extensions API JavaScript file, and the JavaScript file where we will write our code. The page also defines some very basic UI elements. The JavaScript libraries we included were Bootstrap, DataTables, and JQuery. Data Tables has a convenient download builder [here](https://datatables.net/download/) which we used to bundle all this together.

The final step for writing a complete extension is to call the Extension API method `initializeAsync()` when the page has loaded. This indicates to Tableau that this is a real extension and everything is working as expected. The method call also triggers Tableau to configure the rest of the Extensions API.

### Part 2 - Ask the User to Select a Sheet

Now that our extension is initialized, we'll need to prompt the user to choose which sheet on their dashboard to get data from. We will provide the user with a list of buttons and once they select one, show the data for that sheet.

In order to prompt the user, we first need to ask Tableau which worksheets are on the dashboard with our extension by calling `tableau.extensions.dashboardContent.dashboard.worksheets` to get a list of all the worksheets. For each worksheet, we create a new button and add a click event handler for that button. When the button is clicked, we will take action. This action is described in a later part of the tutorial.

### Part 3 - Getting and Displaying the Data

Now that the user has selected a sheet, we want to change the UI that is displayed. We do this by showing and hiding the divs on our page by calling the `setVisibleSection()` function that we defined. We also set up some other UI components, such as defining a new table element, and hooking up the button we defined earlier for switching which sheet to get data from.

Once the right UI is showing, we need to ask Tableau for the data of the sheet the user picked. The first step is to find the worksheet object by name inside the `tableau.extensions.dashboardContent.dashboard.worksheets` array we built the list from before. After we have the sheet, we call the `worksheet.getSummaryDataAsync({ignoreSelection: false})` API to get the data for the marks that are selected (or all the marks if there is no selection). The final step is to process the returned data into a format that Data Tables expects and pass the data over.

### Part 4 - Responding to Selection Changes

The next part of our tutorial covers responding to the user selection changes and updating the data. This is actually an extremely simple process. After we get the worksheet we are going to get data for, we call `worksheet.addEventListener(tableau.TableauEventType.MarkSelectionChanged, handler)`. Whenever this sheet has a new selection, our `handler` function will be called and we can reload the data table with new data.

The other important aspect of the `addEventListener` function is that it returns a helper function that makes it easy to remove the event listner. When we re-initialize the data table, we check to see if our `unregisterEventHandlerFunction` function is defined, if it is, we unregister it, so we are only listening to one sheet at a time.


### Part 5 - Persisting Settings in the Workbook

As you may have noticed when playing with our sample up to this point, when the extension reloads or the workbook is reloaded, we lose track of which sheet the user has selected to view data for. This creates a suboptimal user experience and would be quite unusable if your extensions had a lot of customization. Luckily, the Extensions API includes a settings API which allows you to persiste key/value pairs into a workbook to be used again when the extensions is reloaded.

To take advantage of the settings API, we add calls to `tableau.extensions.settings.set('sheet', worksheetName)` and `tableau.extensions.settings.saveAsync()` once the user has selected their sheet. These APIs together will persist the sheet name into the workbook for it to be available between reloads and will go with the workbook if you publish to server or send to another user. To retrieve these values, we add a call to `tableau.extensions.settings.get('sheet')` in our initialization code. If we have a saved sheet name, we immediately switch to the data table view instead of showing the configuration scr

### Part 6 - Performing Actions

The final step of our tutorial will be about performing actions on our dashboard. For our example, this will mean selecting a set of marks from a workbook which the user has previously bookmarked. Other examples of actions are things like filtering a viz, changing the value of a parameter, or refreshing a data source to update a viz.

To use the select marks API, we first need to get the worksheet we want to select marks for by searching for it by name. Once we have the worksheet back, we can call `worksheet.selectMarksAsync()`.
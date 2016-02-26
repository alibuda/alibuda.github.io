---
title       : Shiny Application and Reproducible Pitch
subtitle    : How Many Days Between Different Dates
author      : ShiShuhuan
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Application Description

This application calculates how many days between different Dates. Users will be required to input values in two boxes and the select two checkboxes. The results will be shown immediately in the main panel.

--- .class #id 

## Sample Results


```r
start_Date <- as.Date('2015-06-01')
end_Date <- as.Date('2016-02-11')
d <- difftime(end_Date,start_Date,units = "days")
d
```

```
## Time difference of 255 days
```
* Options

```r
is_include_sD <- TRUE
is_include_eD <- TRUE
max(as.numeric(d) + 1*is_include_sD+(is_include_eD-1),0)
```

```
## [1] 256
```

--- .class #id 

## ui.R

```r
shinyUI(pageWithSidebar(
    headerPanel("How many days between different Dates"),
    sidebarPanel(
        dateInput("start_date", "Start Date:"),
        dateInput("end_date", "End Date:"),
        checkboxInput("id1", "Including Start Date",F),
        checkboxInput("id2", "Including End Date",T)
    ),
    mainPanel(
        h3('Outputs'),
        h4('Start Date:'),
        verbatimTextOutput("oid1"),
        h4('End Date:'),
        verbatimTextOutput("oid2"),
        h4('Day Diff:'),
        verbatimTextOutput("odate")
    )
))
```

<!--html_preserve--><div class="container-fluid">
<div class="row">
<div class="col-sm-12">
<h1>How many days between different Dates</h1>
</div>
</div>
<div class="row">
<div class="col-sm-4">
<form class="well">
<div id="start_date" class="shiny-date-input form-group shiny-input-container">
<label class="control-label" for="start_date">Start Date:</label>
<input type="text" class="form-control datepicker" data-date-language="en" data-date-weekstart="0" data-date-format="yyyy-mm-dd" data-date-start-view="month"/>
</div>
<div id="end_date" class="shiny-date-input form-group shiny-input-container">
<label class="control-label" for="end_date">End Date:</label>
<input type="text" class="form-control datepicker" data-date-language="en" data-date-weekstart="0" data-date-format="yyyy-mm-dd" data-date-start-view="month"/>
</div>
<div class="form-group shiny-input-container">
<div class="checkbox">
<label>
<input id="id1" type="checkbox"/>
<span>Including Start Date</span>
</label>
</div>
</div>
<div class="form-group shiny-input-container">
<div class="checkbox">
<label>
<input id="id2" type="checkbox" checked="checked"/>
<span>Including End Date</span>
</label>
</div>
</div>
</form>
</div>
<div class="col-sm-8">
<h3>Outputs</h3>
<h4>Start Date:</h4>
<pre id="oid1" class="shiny-text-output"></pre>
<h4>End Date:</h4>
<pre id="oid2" class="shiny-text-output"></pre>
<h4>Day Diff:</h4>
<pre id="odate" class="shiny-text-output"></pre>
</div>
</div>
</div><!--/html_preserve-->

--- .class #id 

## sever.R

```r
shinyServer(
    function(input, output) {
        output$oid1 <- renderPrint({input$start_date})
        output$oid2 <- renderPrint({input$end_date})
        output$odate <- renderPrint({
            if(input$start_date > input$end_date)
                "Start Date should be early !"
            else 
                max(as.numeric(difftime(input$end_date,input$start_date,
                                        units = "days"))+1*input$id1+(input$id2-1),0)
        })
    }
)
```

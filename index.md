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

This application calculates how many days between different Dates.In **output1** Users choose **Start Date** and **End Date**,then consider weather include these days in the calculation.In **output2** Users input a Date and decide regard this Date as **Start Date** or **End Date**.After inputing a diff, you will get the Date you want.

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
shinyUI(
    navbarPage(
        "How many days between different Dates",
        tabsetPanel(
            tabPanel("Documents",
                     mainPanel(
                         includeHTML("Documents.html")
                         )
                     ),
            tabPanel("Output1",
                     sidebarPanel(
                         dateInput("start_date1", "Start Date:"),
                         dateInput("end_date1", "End Date:"),
                         checkboxInput("id11", "Including Start Date",F),
                         checkboxInput("id21", "Including End Date",T)
                         ),
                     mainPanel(
                         h3('Output1'),
                         h4('Start Date:'),
                         verbatimTextOutput("oid11"),
                         h4('End Date:'),
                         verbatimTextOutput("oid21"),
                         h4('Day Diff:'),
                         verbatimTextOutput("odate1")
                         )
                     ),
            tabPanel("Output2",
                     sidebarPanel(
                         selectInput("SE","Start Date/End Date",list("Start Date","End Date")),
                         dateInput("date", "Date:"),
                         numericInput("diff","Day Diff:",value = 0,min = 0,step = 1)
                         ),
                     mainPanel(
                         h3('Output2'),
                         h4('Start Date:'),
                         verbatimTextOutput("oid12"),
                         h4('End Date:'),
                         verbatimTextOutput("oid22"),
                         h4('Day Diff:'),
                         verbatimTextOutput("odate2")
                         )
                     )
            )
        ))   
```

```
## Error in eval(expr, envir, enclos): could not find function "shinyUI"
```

--- .class #id 

## sever.R

```r
shinyServer(
    function(input, output) {
        output$oid11 <- renderPrint({input$start_date1})
        output$oid21 <- renderPrint({input$end_date1})
        output$odate1 <- renderPrint({
            if(input$start_date1 > input$end_date1)
                "Start Date should be early !"
            else 
                max(as.numeric(difftime(input$end_date1,input$start_date1,units = "days"))
                    +1*input$id11+(input$id21-1),0)
        })
        output$oid12 <- renderPrint({
            if(input$SE == "Start Date")
                input$date
            else
                input$date - input$diff
        })
        output$oid22 <- renderPrint({
            if(input$SE == "End Date")
                input$date
            else
                input$date + input$diff
        })
        output$odate2 <- renderPrint({input$diff})

    }
)
```

```
## Error in eval(expr, envir, enclos): could not find function "shinyServer"
```

---
title       : Course 9 project slides
subtitle    : 
author      : Alessandro Destefanis
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## To be done

Scope of the project is the creation of a simple R Shiny application and the presentation of the application thanks to slides.

Aim of the application is to perform an exploration analysis of the mtcars dataset.


```r
str(mtcars)
```

```
## 'data.frame':	32 obs. of  11 variables:
##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
##  $ disp: num  160 160 108 258 360 ...
##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
##  $ qsec: num  16.5 17 18.6 19.4 17 ...
##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```

--- .class #id 

## Code used 1

Here's the code inside sidebarLayout() function in ui.R


```r
sidebarPanel(
        selectInput('x', 'X', choices = columns, selected = "wt"),
        selectInput('y', 'Y', choices = columns, selected = "mpg"),
        selectInput('color', 'Color', choices = columns, selected = "cyl"),
        selectInput('facet_row', 'Facet Row', c(None = '.', columns), selected = "carb"),
        selectInput('facet_col', 'Facet Column', c(None = '.', columns)),
        helpText("In order to explore the dataset please select the variables you desire to be showed.")
    ),
    mainPanel(
       plotlyOutput("carsPlot")
```

--- .class #id 

## Code used 2

Here's the code used to define the graph to be printed, inside server.R


```r
output$carsPlot <- renderPlotly({
    
      p <- ggplot(mtcars, aes_string(x = input$x, y = input$y, color = input$color)) + geom_point()
      facets <- paste(input$facet_row, '~', input$facet_col)
      if (facets != '. ~ .')
          p <- p + facet_grid(facets)
      
      ggplotly(p) %>% 
          layout(height = input$plotHeight, autosize=TRUE)
  })
```

--- .class #id 

## The imput panel

<!--html_preserve--><div class="container-fluid">
<div class="row">
<div class="col-sm-4">
<form class="well">
<div class="form-group shiny-input-container">
<label class="control-label" for="x">X</label>
<div>
<select id="x"><option value="mpg">mpg</option>
<option value="cyl">cyl</option>
<option value="disp">disp</option>
<option value="hp">hp</option>
<option value="drat">drat</option>
<option value="wt" selected>wt</option>
<option value="qsec">qsec</option>
<option value="vs">vs</option>
<option value="am">am</option>
<option value="gear">gear</option>
<option value="carb">carb</option></select>
<script type="application/json" data-for="x" data-nonempty="">{}</script>
</div>
</div>
<div class="form-group shiny-input-container">
<label class="control-label" for="y">Y</label>
<div>
<select id="y"><option value="mpg" selected>mpg</option>
<option value="cyl">cyl</option>
<option value="disp">disp</option>
<option value="hp">hp</option>
<option value="drat">drat</option>
<option value="wt">wt</option>
<option value="qsec">qsec</option>
<option value="vs">vs</option>
<option value="am">am</option>
<option value="gear">gear</option>
<option value="carb">carb</option></select>
<script type="application/json" data-for="y" data-nonempty="">{}</script>
</div>
</div>
<div class="form-group shiny-input-container">
<label class="control-label" for="color">Color</label>
<div>
<select id="color"><option value="mpg">mpg</option>
<option value="cyl" selected>cyl</option>
<option value="disp">disp</option>
<option value="hp">hp</option>
<option value="drat">drat</option>
<option value="wt">wt</option>
<option value="qsec">qsec</option>
<option value="vs">vs</option>
<option value="am">am</option>
<option value="gear">gear</option>
<option value="carb">carb</option></select>
<script type="application/json" data-for="color" data-nonempty="">{}</script>
</div>
</div>
<div class="form-group shiny-input-container">
<label class="control-label" for="facet_row">Facet Row</label>
<div>
<select id="facet_row"><option value=".">None</option>
<option value="mpg">mpg</option>
<option value="cyl">cyl</option>
<option value="disp">disp</option>
<option value="hp">hp</option>
<option value="drat">drat</option>
<option value="wt">wt</option>
<option value="qsec">qsec</option>
<option value="vs">vs</option>
<option value="am">am</option>
<option value="gear">gear</option>
<option value="carb" selected>carb</option></select>
<script type="application/json" data-for="facet_row" data-nonempty="">{}</script>
</div>
</div>
<div class="form-group shiny-input-container">
<label class="control-label" for="facet_col">Facet Column</label>
<div>
<select id="facet_col"><option value="." selected>None</option>
<option value="mpg">mpg</option>
<option value="cyl">cyl</option>
<option value="disp">disp</option>
<option value="hp">hp</option>
<option value="drat">drat</option>
<option value="wt">wt</option>
<option value="qsec">qsec</option>
<option value="vs">vs</option>
<option value="am">am</option>
<option value="gear">gear</option>
<option value="carb">carb</option></select>
<script type="application/json" data-for="facet_col" data-nonempty="">{}</script>
</div>
</div>
<span class="help-block">In order to explore the dataset please select the variables you desire to be showed.</span>
</form>
</div>
<div class="col-sm-8">
<div id="carsPlot" style="width:100%; height:400px; " class="plotly html-widget html-widget-output"></div>
</div>
</div>
</div><!--/html_preserve-->

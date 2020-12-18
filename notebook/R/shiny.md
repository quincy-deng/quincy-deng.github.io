# shiny可视化网页学习

主题结构

```R
# Sidebar Layout
ui <- fluidPage(
  titlePanel
  sidebarLayout
    sidebarPanel
    mainPanel
# Grid Layout
ui <- fluidPage
  titlePanel
  fluidRow
    column
      h4
      sliderInput
      checkboxInput
      checkboxInput
    column
      selectInput
      selectInput
# Tabsets
ui <- fluidPage(
  titlePanel
  sidebarLayout
    sidebarPanel
    mainPanel
        tabsetPanel
            tabPanel
            tabPanel
# Navlists
ui <- fluidPage(
  titlePanel
  navlistPanel(
    "Header A",
    tabPanel("Component 1"),
    tabPanel("Component 2"),
    "Header B",
    tabPanel("Component 3"),
    tabPanel("Component 4")
# Navbar Pages
ui <- navbarPage("My Application",
  tabPanel("Component 1"),
  tabPanel("Component 2"),
  tabPanel("Component 3")
) 
      
```

`p` `<p>` A paragraph of text

`h1` `<h1>` A first level header

`h2` `<h2>` A second level header

`h3` `<h3>` A third level header

`h4` `<h4>` A fourth level header

`h5` `<h5>` A fifth level header

`h6` `<h6>` A sixth level header

`a` `<a>` A hyper link

`br` `<br>` A line break (e.g. a blank line)

`div` `<div>` A division of text with a uniform style

`span` `<span>` An in-line division of text with a uniform style

`pre` `<pre>`Text ‘as is’ in a fixed width font

`code` `<code>` A formatted block of code

`img` `<img>` An image

`strong` `<strong>` Bold text

`em` `<em>` Italicized text

`HTML`  Directly passes a character string as HTML code


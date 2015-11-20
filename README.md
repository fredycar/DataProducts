# This is the code to Peer Assessment...

UI

shinyUI(bootstrapPage(
  
  selectInput(inputId = "n_breaks",
              label = "Number of bins in histogram (approximate):",
              choices = c(10, 20, 35, 50),
              selected = 20),
  
  checkboxInput(inputId = "individual_obs",
                label = strong("Show individual observations"),
                value = FALSE),
  
  checkboxInput(inputId = "density",
                label = strong("Show density estimate"),
                value = FALSE),
  
  plotOutput(outputId = "main_plot", height = "300px"),
  
  # Display this only if the density is shown
  conditionalPanel(condition = "input.density == true",
                   sliderInput(inputId = "bw_adjust",
                               label = "Bandwidth adjustment:",
                               min = 0.2, max = 2, value = 1, step = 0.2)
  )
  
))

SERVER

shinyServer(function(input, output) {
  
  output$main_plot <- renderPlot({
    
    hist(mtcars$mpg,
         probability = TRUE,
         breaks = as.numeric(input$n_breaks),
         xlab = "Miles (US) gallon (mpg)",
         main = "Fuel Consumption Distribution", col=rainbow(12))
    
    if (input$individual_obs) {
      rug(mtcars$mpg)
    }
    
    if (input$density) {
      dens <- density(mtcars$mpg,
                      adjust = input$bw_adjust)
      lines(dens, col = "darkred")
    }
    
  })
})

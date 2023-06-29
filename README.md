# A WPF Custom Control for Zooming and Panning

### Update Date: 22 Mar 2011   

### Introduction  
This article examines the use and implementation of a reusable WPF custom control that is used to zoom and pan its content. The article and the sample code show how to use the control from XAML and from C# code.  

The main class, ZoomAndPanControl, is derived from the WPF ContentControl class. This means that the primary purpose of the control is to display content. In XAML, content controls are wrapped around other UI elements. For example, the content might be an image, a map or a chart. In this article, I use a Canvas as the content. This Canvas contains some colored rectangles that can be dragged about by the user.  

I'll present this article in two parts. The first part shows how to use ZoomAndPanControl and has walkthrough of the three sample projects. This should be enough if all you want to do is use the code or just try it out. The second part of the article goes into detail as to how the control is implemented. This part will be useful if you want to make your own modifications to ZoomAndPanControl or generally to help you understand how to develop a non-trivial custom control.  


### Assumed Knowledge  
It is assumed that you already know C# and have a basic knowledge of using WPF and XAML.  

### Background  
In my previous article, I alluded that I have been working on a flow-charting control. The working area in which the flow-chart is displayed and edited can be much larger than the window that contains it. Usually, this is the perfect place to use a ScrollViewer to wrap the content. ScrollViewer is a pretty easy class to use. It handles content larger than itself by providing a viewport onto the content. The viewport is optionally bounded by scrollbars that allow the user to view any portion of that content.  

However I wanted the user to be able to zoom in and see more detail or zoom out to see an overview. Implementing the zooming by scaling the content is fairly easily done using WPF 2D transformations. Although making it play nicely with the ScrollViewer is another matter entirely!  

Writing this kind of custom control is harder than you might think. My first implementation was a bit of a disaster (well not completely - it did inspire me to rewrite the code and then write this article). The code for zooming and panning was intertangled with the code for displaying and editing the flow-chart. It probably didn't help that I was also learning WPF at the time. There are one or two examples around on the internet that show how to do this kind of thing, however I found that they were either lacking in that they didn't do entirely what I wanted or that they came with baggage in the form of extra code that I just didn't need. Suffice to say that before I wrote the code for this article, what I had was a complicated mess that kept breaking and was generally difficult to modify.  

### ZoomAndPanControl: What it is, What it is not  
My main aim with this article is to keep the complexity of the code to a minimum. To this end ZoomAndPanControl doesn't attempt to do anything much more than what I need of it. Notably I have not attempted to implement any kind of UI virtualisation.  

In addition, there is no input handling logic in the reusable control. I think that this kind of code is application specific and likely to change. Implementing it generically would add complication and so I have delegated input handling to the application code. In the sample code, the input handling code can be found in the MainWindow class (which is derived from Window).  

I have found that moving the zoom and pan logic to a custom control has allowed me to cleanly separate out this code from the rest of the application. As a result, both sets of code are simpler, cleaner and more understandable.  
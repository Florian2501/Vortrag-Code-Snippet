<!--
author:   Florian Schierz

email:    florischierz@gmail.com

logo: https://upload.wikimedia.org/wikipedia/commons/2/2a/Corporate_Woman_Giving_a_PowerPoint_Presentation.svg

version:  0.0.1

language: de

narrator: Deutsch Female

import: https://github.com/liascript/CodeRunner

comment:  Es wird gezeigt, wie typische bekannte Präsentationselemente auch
          mithilfe von LiaScript genutzt werden können.

tags:     LiaScript, PowerPoint, Tutorial

-->
# Watch it in LiaScript!

Click here to open it in LiaScript:
[![LiaScript](https://raw.githubusercontent.com/LiaScript/LiaScript/master/badges/course.svg)](https://liascript.github.io/course/?https://github.com/Florian2501/Vortrag-Code-Snippet/blob/master/CodeSnippetPresentation.md)

# The Problem In Familys

||
![Family-love](https://cdnext.funpot.net/bild/funpot0000250003/39/Pingu-Streit.gif)

- argues
- disputes
- conflicts

{{1}}
>No matter how you split up the tasks, everyone would say, that he or she does more than all the others together.

## Idea
- Create an app to track the done tasks of every family member to avoid argue.

- With the system you can see who has done how much -> decide who has to do a remaining task.

- See details of done tasks with graphical visualization.

- Helps to distribute tasks and make family life more fair.

![Peace](https://media.tenor.com/images/923df43fb015bd6c375fb7c7b7d5f293/tenor.gif)
## Implementation
- use WPF App to visualise it

{{1}}
`Table Window`
![TableWindow](Tabelle.png)

- visualise the points


{{2}}
`Task Window`
![TaskWindow](NewTask.png)

- enter done task


{{3}}
`Create-Task Window`
![CreateTaskWindow](CreateTask.png)

- create a new task that is not in the program yet
- change values of an existing task


{{4}}
`Person Window`
![PersonWindow](Persons.png)

- manage persons -> delete or enter


## My Special Code Snippet

For better visualization add a bar chart starting with the best at the top. The bars has to be in correct relation to the other ones.

Furthermore there should be the names, places and total points.
![Picture of the bar chart](BarChart.png)



                              {{0-1}}
********************************************************************************

```csharp        Snippet
int place = 1;

foreach ((int points, string name) in places)
            {
                //Build the Textblock of place and Name
                TextBlock nameBlock = new TextBlock();
                nameBlock.Margin = new Thickness(15, 5, 0,0);
                nameBlock.Text = place.ToString() + ". " + name;
                Grid.SetRow(nameBlock, (place-1) * 2);//every second row

                //Build the text block with the points
                TextBlock pointsBlock = new TextBlock();
                pointsBlock.Text = points.ToString();
                pointsBlock.Margin = new Thickness(15, 5, 0, 0);
                Grid.SetRow(pointsBlock, (place - 1) * 2);
                Grid.SetColumn(pointsBlock, 1);//every second row and in the second column

                //add the textblocks to the fitting Gridcell in the Menu
                placeGrid.Children.Add(nameBlock);
                placeGrid.Children.Add(pointsBlock);

                //create a rectangle in the correct length and in relation to the first place -> will be always the whole line (190)
                Rectangle bar = new Rectangle();
                bar.Width = 189.0 * ((double)points / (double)places[0].Item1) + 1.0;//+1 that there is even something if the points are 0
                bar.Height = 20;
                bar.Fill = new SolidColorBrush(ColorTheme.design.BarChart);//The given color for the bar chart
                bar.HorizontalAlignment = HorizontalAlignment.Left;
                bar.Margin = new Thickness(5, 0, 0, 0);

                //set the rectangle to the row that is empty under each name
                Grid.SetRow(bar, (place * 2) - 1);
                //make it over the whole width
                Grid.SetColumnSpan(bar, 2);
                //add the abr to the Grid
                placeGrid.Children.Add(bar);

                place++;
            }
```

********************************************************************************


Going through the code step by step:

{{1-2}}
********************************************************************************

```csharp        Part 1
int place = 1;

foreach ((int points, string name) in places)
{
//Build the Textblock of place and Name
TextBlock nameBlock = new TextBlock();
nameBlock.Margin = new Thickness(15, 5, 0,0);
nameBlock.Text = place.ToString() + ". " + name;
Grid.SetRow(nameBlock, (place-1) * 2);//every second row

//Build the text block with the points
TextBlock pointsBlock = new TextBlock();
pointsBlock.Text = points.ToString();
pointsBlock.Margin = new Thickness(15, 5, 0, 0);
Grid.SetRow(pointsBlock, (place - 1) * 2);
Grid.SetColumn(pointsBlock, 1);//every second row and in the second column

```

- sorted array of points and names of the persons
- place-var to count up
- create TextBlocks with the certain name and places
- put it in every second row and first column (auto)
- same with points for the persons but in second column
********************************************************************************

{{2-3}}
********************************************************************************

```csharp        Part 2
//add the textblocks to the fitting Gridcell in the Menu
placeGrid.Children.Add(nameBlock);
placeGrid.Children.Add(pointsBlock);
```

- simply add the crated TextBlocks to the existing Grid
********************************************************************************

{{3}}
********************************************************************************

```csharp        Part 3
//create a rectangle in the correct length and in relation to the first place -> will be always the whole line (190)
Rectangle bar = new Rectangle();
bar.Width = 189.0 * ((double)points / (double)places[0].Item1) + 1.0;//+1 that there is even something if the points are 0
bar.Height = 20;
bar.Fill = new SolidColorBrush(ColorTheme.design.BarChart);//The given color for the bar chart
bar.HorizontalAlignment = HorizontalAlignment.Left;
bar.Margin = new Thickness(5, 0, 0, 0);

//set the rectangle to the row that is empty under each name
Grid.SetRow(bar, (place * 2) - 1);
//make it over the whole width
Grid.SetColumnSpan(bar, 2);
//add the abr to the Grid
placeGrid.Children.Add(bar);

place++;
}
```

- create new Rectangle with the correct length, height and color
- add it to the correct row below the correct name
- set the width over all columns of the grid
- add it to the Grid
- increase place for next loop
********************************************************************************
{{4}}
![Alarm](https://media3.giphy.com/media/eImrJKnOmuBDmqXNUj/giphy.gif)
**Exception Alert!**

## Debug!
>**Problem:**  If nobody did a task and everyone has 0 points, it divides by 0 -> **Exception!!!**

```csharp        Part 3
//create a rectangle in the correct length and in relation to the first place -> will be always the whole line (190)
Rectangle bar = new Rectangle();

//////////////////////////////////////////////////////////////////////
//check wether it is not a number when no one even has a point
double percent = (double)points / ((double)places[0].Item1);
if (Double.IsNaN(percent)||Double.IsInfinity(percent))
{
  bar.Width = 1;
}
else
{
  bar.Width = 189.0 * percent + 1.0;//+1 that there is even something if the points are 0
}
/////////////////////////////////////////////////////////////////////

bar.Height = 20;
bar.Fill = new SolidColorBrush(ColorTheme.design.BarChart);//The given color for the bar chart
bar.HorizontalAlignment = HorizontalAlignment.Left;
bar.Margin = new Thickness(5, 0, 0, 0);

//set the rectangle to the row that is empty under each name
Grid.SetRow(bar, (place * 2) - 1);
//make it over the whole width
Grid.SetColumnSpan(bar, 2);
//add the abr to the Grid
placeGrid.Children.Add(bar);

place++;
}
```
- check the result of the division for `Not a Number` or `Infinity`
- set it to 1 if it is invalid (to see at least a small line)
- else multiply by the `percent` value


{{1}}
********************************************************************************
**Do you see other problems?**
Please tell me and let us make the life of families more peaceful.
********************************************************************************

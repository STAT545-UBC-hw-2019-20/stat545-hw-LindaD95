# **Assignment 5** 

### Factor and Figure Management

Assignment 5 focused on pivoting using the Gapminder dataset. Exercises involving table joins used a much smaller dataset that looked at weeding planning 


The html for the assignment can be found  [here](https://stat545-ubc-hw-2019-20.github.io/stat545-hw-LindaD95/Hw05/hw05.html)

#### **Exercise 1:** Thoughts on 

Accessibility to your work when using Rstuido allows for better collaboration and obtaining help from other R users. Frustration arises from those who by default start the scripts with setwd() and rm(list = ls())  at the beginning of their scripts due to the fact that in order for someone else to access the files if they rewirte the *exact* path on their computer this is partially due to the fact that the rules that dictate file paths between operating systems are not harmonious. Jenny Byran recommends to use the here::here package as it more robust than simply just working with R projects which she still recommends to do also. The here package creates relative file paths, avoiding the language barrier between operating systems. Even if you are not in the directory, here() can find the working directory. Manipulated data to produce a new table or figure could also be saved in different subdirectories without having to change the relative directory. Another great benefit of here is that files can still run outside of Rstudio. 
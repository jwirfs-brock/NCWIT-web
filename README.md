# Infoviz Project 2

I created this visualization, which shows the distribution of percent females enrolled in STEM programs in U.S. universities sorted by major group and when they have to declare a major, for INFO 5602 as part of a team project during the spring 2018 semester at University of Colorado. This visualization uses data collected from [NCWIT](https://www.ncwit.org/) member institutions, which was shared with us via Lecia Barker.

## [You can view the visualization here.](https://jwirfs-brock.github.io/NCWIT-web/index.html)

## [View all of the visualizations that our team made (including a physical sonification!)](https://info-4602-5602.github.io/project-2-ncwit-team-13/dashboard.html)

## Description and Overview
This visualization addresses the question: **Does when a student has to declare a major (upon enrollment, after the first year, after the second year, or some other time) have an effect on how many female students enroll in STEM programs?**

To address this question, I created a visualization showing the distribution, across all of the school and programs, of the percent of female students. This is shown as both a violin plot and an overlying swarmplot. On the swarmplot, each dot is a unique institution/program/year combination. The distribution statistics used to render the violin plot are used on those unique rows as well.

The position/space channel separates majors/programs (which we lumped into broad categories) into clusters. The color channel reflects when students must declare a major.

We were curious what patterns would emerge, and were somewhat surprised to note that there doesn’t seem to be a noticeable relationship between when students must declare and what the female enrollment rate is. This may seem like a null result, but it’s still useful: Perhaps, instead of worrying about how to tweak the enrollment timeline, schools can focus on other factors that have more of an effect on outcomes for females in STEM.

However, some interesting patterns are evident: In media and design, nearly all programs have students selecting their major upon enrollment. It’s more common in computer science and engineering for students to have to declare later on in their college career. You can see that in the all-female programs in computer science (represented by the line up at the top at 100% female), there are no programs where students choose a major upon enrollment. Alternatively, in the CS programs with no women (the line at 0%), choosing a major upon enrollment dominates.

While I took the lead designing and implementing this visualization, Jen Liu providing a supporting role by giving feedback and guidance on design decisions.

### How to view

Use the buttons at the top to switch between which declaration-group is highlighted: upon enrollment, after the first year, after the second year, or other. (Note, there was an additional category, “university close” -- because this was so small we chose not to include it in the interface.)

Mouse over the “expand distribution” lab next to “Computer Science” or “Engineering” to see a pop-out view of these majors.

#### Design Process

The first phase of the design process was selecting a question to guide our inquiry. Our team brainstormed questions and topics after looking at the dataset, and narrowed it down to our favorites. This question was selected because Lecia Barker explicitly called out the fact that students have to declare majors at different times as something that schools were interested in learning about -- does this have an effect on enrollment, retention, and success in the programs?

The next phase was doing some data cleaning and preliminary analysis using Python, pandas and Jupyter notebooks. [You can see the notebook with both data cleaning and visualizations here.](https://github.com/INFO-4602-5602/project-2-ncwit-team-13/blob/master/viz_gender_enrollment/data-analysis/JWB-NCWIT-analysis.ipynb) [Note that it takes a while to load because the plots take time to render.]

I cleaned the data by making sure the values for the “When do students typically declare their major?” column were normalized by fixing misspelled values (i.e. “Upon Enrollent”). I also added in calculated fields for percent of females enrolled and percent of females graduated. During this process, I saw that there were many institution/year/program entries that were missing data. To deal with this, I dropped rows that had no values, and created a data frame for schools with enrollment data and a separate frame for schools with graduation data (a much smaller subset of the original dataset). Originally, our plan was to see if there was a relationship between when students have to declare a major and what percent of graduates are females, but upon looking at the data I decided that this winnowed the results too much. I chose to use percent of females enrolled in the major as the main metric to display because it is both meaningful and there is more data.

For chosing a plot type, I knew that I wanted to emphasize the distribution of female enrollment, so I looked at box plots, scatter plots, violin plots and swarmplots. Using [Andrew Sielen’s suite of distribution plots for d3](http://bl.ocks.org/asielen/92929960988a8935d907e39e60ea8417) as a guide, I tried out some of the different possibilities. I also looked at the plots available in [Seaborn](https://seaborn.pydata.org/index.html), a visualization package for Python that handles distribution plots well. I ended up choosing a violin plot with a swarmplot overlay. This allows the user to get an impression/gist of the overall distribution, while also being able to see individual values. I adjusted the violin plot settings so that the total area of each violin is proportional to the number of data points in each category (note: it’s NOT proportional to the number of total students in each category, which also would have been interesting). I chose to “clip” the violin plots at 0 percent and 100 percent, because it was confusing to see them extend into, say, negative numbers, when I know there are no negative values for percent females.

Here's an example of an unstyled violin plot made using Seaborn. Note that it has tips that extend beyond 0 and 1, the widths are uniform (that is, the area isn't scaled by number of data points), and it has the default color palette:

![Violin plots without dots](https://i.imgur.com/uqdb503.png)

The next choice was which data to encode in which channels. To decide, I made prototype plots of each and circulated them among the team to get feedback.

Here’s an example of a design I didn’t use, which encodes when students must declare a major by position:

![Swarm plots arranged by wheb students declare a major](https://i.imgur.com/Otqvzmh.png)

Next, I had to decide what interaction to include. I decided that the import elements a user might want to explore in this visualization would be how the patterns change based on when a student enrolls. So I made plots that highlight each category using color pop-out (muting the other categories) and gave the user the ability to toggle between them.

Another major design decision I had to make was how to handle the “overlapping” points. If you look at the edges of the CS and engineering clusters, you’ll see that they appear to be bounded, and there are a bunch of points piled up on top of each other at the edges. This is because there are so many data point in those groups, and the distribution is so highly clustered  around the 15% to 20% female zone, that the point clustering algorithm in Seaborn breaks. First, I considered reducing the size of the points. While this worked in terms of being able to display all points without overlap (see below), it made it much harder to see individual points in the color “pop-out” views.

![Swarm plots with tiny dots](https://i.imgur.com/YmzU5DG.png)

I also considered making the frame wider, or reducing the number of categories shown. These were also non-ideal options. In the end, I decided to show a coordinated view that displays the full distribution for the large categories, CS and engineering, on a tooltip when you mouse over the title. This of course has its flaws, too.

Here's the whiteboard from a meeting Jen and I had, discussing some design decisions:
![White board listing some design decisions](https://i.imgur.com/luYv0VN.jpg)

If I had more time, I would have tried to implement the visualization in D3 instead of in Seaborn and Javascript. This would have allowed us to add enhanced interactivity, such as a mouseover feature for each dot that shows you the institution code, total enrollment, and other related data. With more time, I also could have explored the time dimension in these relationships.

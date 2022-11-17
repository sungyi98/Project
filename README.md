# Machine Learning Team Project

### Group 2

21600313 Park Chungho   
21700480 Yoon Sungie    
21700365 Son Juchan   
22000625 Jang Yeri

---------------------------

## libraries
Figure 1 is the libraries used in this project. The libraries and functions used are as follows.  

> __pandas__ : Library used to read data and generate dataframes  
> https://pandas.pydata.org/about/   

> __matplotlib__: A library that plots various data in many ways   
> https://doorbw.tistory.com/173   

> __numpy__: Library used to change the data type   
> https://rfriend.tistory.com/285    

> __random__: Library used to generate random numbers   
> https://wikidocs.net/79    

> __Basemap__: Library used to visualize maps   
> https://pinkwink.kr/1199    


```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import random as rd
from mpl_toolkits.basemap import Basemap
```

Additionally, it was difficult to install the 'Basemap' library. This problem was 
solved by installing a library as follows as a Conda virtual environment. After 
installing conda, we created a virtual environment named project and activated it. 
Then, ‘basemap-data-hires’ and ‘basemap’ were installed in order. 

```
conda create -n project   
conda activate project   
conda install -c conda-forge basemap-data-hires   
conda install -c conda-forge basemap   
```
-------------------
## Used Datasets
In Problem 1, eight points and pre-determined centroid points information were used as data.

```python
# eight points information
dots = np.array([[2,10],[2,5],[8,4],[5,8],[7,5],[6,4],[1,2],[4,9]])

# centroids information
centroids = np.array([[2,10],[5,8],[1,2]])
```

In Problem 2, information on the territory points and information on the vertiport candidates were used as data.
```python
territory = pd.read_csv('/Users/sungie/Desktop/jupyter/South_Korea_territory.csv', sep = ',').to_numpy()
candidate = pd.read_csv('/Users/sungie/Desktop/jupyter/Vertiport_candidates.csv', sep = ',').to_numpy()
```

------------------
## User-Defined Functions
Prior to the explanation, several rules were established for clear understanding. 
Thick words in Italics refer to functions or variables. Adding () to the end is a 
function, and the other is a parameter of an internal variable. [] is direct reference 
to the annotation within the code shown in the figure

### 1. Calculate Distance Functions
> _cal_dist(P1,P2):_    
* cal_dist() is a function that calculate the distance between two points based on the Euclidean distance. Given two points (P1 and P2) consisting of a pair of X and Y, the distance between the two points is calculated and returned
```python
def cal_dist(P1,P2):
    dist = np.sqrt(np.sum((P1 - P2) ** 2))
    return dist
```

### 2. Drawing Functions   
> _draw_plot(dots, centroids, color_d, color_c):_
* draw_plot() is the drawing function of Problem 1.
* Information on eight points(dots), central points(centroids), information on the color of dots (color_d), and information on the color of centroids(color_c) are included as input values.
* A point of a designated color is displayed on the graph according to the input information.

```python
def draw_plot(dots,centroids,color_d,color_c):
    
    plt.figure(figsize = (3,3)) # Set figure size
    plt.xlabel('X - axis') # Set X,Y axis
    plt.ylabel('Y - axis')
    
    # Displays 'dots' and 'centroids' as dots of a specified color
    for i in range(dots.shape[0]):
        plt.scatter(dots[i][0],dots[i][1],color=color_d)
    for j in range(centroids.shape[0]):
        plt.scatter(centroids[j][0],centroids[j][1],color=color_c)
    
    return plt
```

> _draw_map(territory, candidate, color, vertiports = False):_
draw_map() is the drawing function of Problem 2.
Information on the territory of the map(territory), information on the vertiport candidates(candidate), and information on the colors of the candidates(color) are input.
In addition, an option named vertiports is also entered, which will be explained later.
When the function is executed, draw an appropriate map in the [# Draw a map] section.
In the section [#Mark 'territory' as a blue dot on the map], the territory data entered is displayed as a blue dot on the map. Also, in the section [# Mark 'candidate' as a dot of the color specified on the map], the candidate information is displayed on the map. 
As shown in Figure 8, one option was added to draw Problem 2.2 and 2.3 graphs with a single function. Candidate points in 2.2 are drawn with different colors for each cluster, while in 2.1 are drown with one designated color. If the parameter color is given as a single color in string, the candidate points are displayed on the map in the same color specified. However, if the color is given as cluster information in the form of a list, each point is displayed on the map in different colors for each cluster. 
In this part, the color dictionary(color_dict) shown in Figure 7 is used to change the index of each cluster to each color. color_dict is dictionary that replaces int-type cluster data with color information, and it can specify colors up to 20 clusters.
vertiports is an option to represent the finally selected vertiports in star-shaped dots as shown in Figure 10, and has been added to visualize Problem2 plots as a single function. If the vertiports value is not entered or set to False, the basic type of dots are drown. On the other hand, if vertiport is set to True, the indexed star-shaped dots are drawn.

```python
def draw_map(territory,candidate,color,vertiports=False):
    
    # Draw a map
    map = Basemap(projection='merc', lat_0=37.35, lon_0=126.58, resolution = 'h', 
                  urcrnrlat=41, llcrnrlat=33, llcrnrlon=123.5, urcrnrlon=130.5)
    
    parallels = np.arange(30.,50.,2.5)
    map.drawparallels(parallels,labels=[True,False,False,False])
    meridians = np.arange(125.,131.,2.5)
    map.drawmeridians(meridians,labels=[True,False,False,True])

    map.drawcoastlines()
    map.drawcountries()
    map.drawmapboundary()
    
    # Mark 'territory' as a blue dot on the map
    for i in range(territory.shape[0]):
        x,y = map(territory[i][0],territory[i][1])
        map.plot(x, y, 'o', markersize=1, color='blue')
    
    # Mark 'candidate' as a dot of the color specified on the map
    for j in range(candidate.shape[0]):
        a,b = map(candidate[j][0],candidate[j][1])
        
        ## Specify the color of the point
        # If the color is entered in string type
        if type(color) is str:
            col = color
        # If the color is entered as cluster information
        else:
            col = color_dict[color[j]]
        
        ## If the "Vertiport" option is entered
        if vertiports:
            # Use a star marker and add a label to the top of the dot
            map.plot(a, b, markersize=6, color=col, marker='*')
            plt.annotate(j+1, (a, 1.05*b), size=8)
        
        # If not, mark as a point of the specified color
        else: 
            map.plot(a, b, 'o', markersize=1, color=col)
    return plt
```


순서 없는 목록은 다음과 같이 작성할 수 있습니다.

* 깃 튜토리얼
  * 깃 Clone
  * 깃 Pull
  * 깃 Commit
    * 깃 Commit 1)
    * 깃 Commit 2)

인용 구문은 다음과 같이 작성할 수 있습니다.

> '공부합시다.' - 나동빈 - 

테이블은 다음과 같이 작성할 수 있습니다.

이름|영어|정보|수학
---|---|---|---|
나동빈|98점|87점|100점|
홍길동|97점|78점|93점|
이순신|89점|93점|97점|

강조는 다음과 같이 할 수 있습니다.

**치킨** 먹다가 ~~두드리기~~났어요. ㅠㅠ
 

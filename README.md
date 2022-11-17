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
* draw_map() is the drawing function of Problem 2.
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

```python
## Match cluster information with color information
# Color dictionary range :0~19
color_dict = {0:'gold', 1:'lightpink', 2:'powderblue' ,3:'orchid',4:'yellowgreen'
              ,5:'limegreen',6:'olive',7:'lightskyblue',8:'slategray',9:'royalblue'
              ,10:'grey',11:'mistyrose',12:'darkslateblue',13:'darkorange', 14:'tomato'
              ,15:'peru',16:'lavender',17:'firebrick',18:'silver',19:'lightcoral'}
```


### 3. K-Means Algorithm Function    
> __kmeans(dots, auto = False, centroids = None):__   
* kmeans() is a function of finding the centroid value through the K-Means algorithm, which is commonly used in Problems 1 and 2.
In dots, information about the points to be clustered is entered. In Problem 1, eight points are used as input values, and in Problem 2, vertiport candidate information is used as input value.
K means the number of clusters to be created. 
auto and centroids are options added to efficiently implement Problem 1 and 2 using one function, which will be explained later.

max_iter, a variable declared within the function, means the maximum number of iterations to prevent infinite iterations when repeatedly trying to find the centroid value. The variable theta function as a reference point for negligible difference value when selecting the final centroid.
In the [# Rows and columns of the data] part, the row(N) and column(dim) of dots are extracted.
In the [# initialize variables] part, the sse and count(cnt) values are initialized. 

In the [## Generate K random centroid values] part, the centroid value is randomly generated with the centroids option. Unlike Problem 1, in which clustering begins with an pre-determined centroid values, Problem 2 requires randomly generated centroid values. Therefore, when no value is transferred to the centroids parameter in Problem 2, the default value None is entered into the centroids. Accordingly, K random centroid values having the same data shape as dots are generated.

[## if ‘auto’ is False, performance only once] is related to the auto option. In Problem 1, the kmeans() function is called several times manually to show the centroid values found in each iteration. On the other hand, in Problem 2, the K-Means algorithm is automatically repeated several times with only one kmeans() function call, showing the final centroid value. The auto makes this possible. In Problem 1, when auto is entered as False, max_iter is designated as 1, and the K-Means algorithm is implemented only once and terminated. On the other hand, when auto is entered as True in Problem 2, the function proceeds with existing max_iter value. The part where the K-Means algorithm is repeated is [## Repeat the k-means algorithm as much as max_iteration].

cluster is a list used to store cluster information of each point. If K is 10, the information of each cluster is stored as number between 0 and 9.
cnt is a variable for recording how many times the K-Means algorithm has been repeated before finding the final centroid values. Data clustering is performed in [## k-means algorithm].
dist is a list that stores the distance from one point value to K centroids in order. For one point, the distance to K centroids is repeatedly calculated and accumulated in dist. When the distance values between one point and K centroids are obtained, the index of the smallest value among the distance values is added to the list cluster through the argmin() function.
In this way, the distance between K centroid values and N points is calculated, and the index of the nearest centroid of each point is all added to the cluster. Thus, the cluster value of each point is specified. 
[# update centroids] is the part that updates the centroid values to the newly calculated centroid values. The new centroid values are obtained by averaging the points corresponding to each cluster in the [# calculate the average of points belonging to each cluster]. After the existing centroid values are stored in prev_centroids, the newly obtained centroid values are stored in the centroids.

[# Terminate the function if the difference between prev_centroids and updated centroids is feasible] is the terminating part of the function. If the distance difference between the previous centroid values and the newly updated centroid values is smaller than the reference value, it is judged as the final centroid and the function is terminated. The sse variable, the sum of the squares of the difference between the K previous centroid values and the updated K centroid values, is used. If this sse value is less than theta value specified earlier, it is considered a negligible difference and the function is terminated. And at the end of kmeans() function, the final centroid values, the iteration number of the K-Means algorithm, and the final cluster information of N points are returned.


## 4. Built-in Functions & External Functions    
### 1) Built-in Functions   
> _X.append(A)_: A function that adds A to the end of the list X.   

> _df.loc[‘A’]_: Value of row 'A' in df 

> _‘{}’.format(Variable)_: Insert variable value into {}    

### 2) External Functions
> #### Numpy     
> Reference: https://numpy.org/doc/stable/index.html 
>> * _np.sqrt(Array)_: Square value of all values in the Array    
>> * _np.sum(Array)_: Sum of all values in the Array    
>> * _np.sqrt(Array)_: Square value of all values in the Array     
>> * _np.Inf_: infinite value    
>> * _np.zeros(X)_: Create X zero-filled arrays   
>> * _np.argmin(List)_: Returns the index of the lowest value among the values in the List
>> * _np.array(List)_: Convert List to array type.
>> * _np.round(X,N_): Round X to N decimal places
>> * _np.arange(A,B,C)_: Create an array at intervals of C from A to less than B.
>> * _X.shape_: A function that tells the shape of an array(X), returns the length of rows and columns.


> #### Pandas   
> Reference: https://pandas.pydata.org/docs/index.html    
>> * _pd.DataFrame(Data, columns=Column_name, index=Index_name)_: The Data is made into a data frame, and the column name is Column_names and the index is Index_name.
>> * _pd.read_csv(‘File’, sep = ‘,’)_: Read the File with a delimiter of ‘,’.
>> * _DataFrame.to_numpy()_: Convert a Pandas object to ndarray, a numpy array object.

> #### Matplotlib
> Reference: https://matplotlib.org/stable/tutorials/introductory/pyplot.html 
>> * _plt.figure(figsize=(A,B))_: Draw a graph with a horizontal length A and a vertical length B.
>> * _plt.xlable(‘X_lab’)_: Mark the x-axis with ‘X_lab’.
>> * _plt.ylabel(‘Y_lab’)_: Mark the y-axis as ‘Y_lab’.
>> * _plt.scatter(X,Y,color=Col)_: Place a Col colored dot on the (X,Y) position.
>> * _plt.title(‘Plt_title’)_: Show the title ‘Plt_title' on the plot
>> * _plt.show()_: Showing a drawn picture.
>> * _plt.annotate(X,Y,size=Size)_: Add an annotation with Size size in (X,Y) position.

> #### Random 
> Reference: https://numpy.org/doc/stable/reference/random/generated/numpy.random.randint.html 
>> * _rd.randint(A,B)_: Returns a random integer value between A and B.
> #### Basemap
> Reference : https://matplotlib.org/basemap/api/basemap_api.html 
>> * _map = Basemap()_: Draw a map of the desired shape considering latitude and longitude.
>> * _map.drawparallels()_: Parallel labelling on the map.
>> * _map.drawmeridians()_: Meridian labeling on the map.
>> * _map.drawcoastlines()_: Draw coastlines.
>> * _map.drawcountries()_: Draw country boundaries.
>> * _map.drawmapboundary()_: Draw a line around map projection region.
>> * _map.plot()_: Plotting on the map.


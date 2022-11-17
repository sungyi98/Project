# Vertiport Placement    
---------------------------  

__Vertiport Placement__ is a Python program that uses the K-Means algorithm to clustering vertiports candidate data.    
Problem 1 is a toy example, clustering the given 8 points.     
Problem 2 clusters the vertiport candidates and visualizes them on the map.   

---------------------------    

### Group 2

21600313 Park Chungho   
21700480 Yoon Sungie    
21700365 Son Juchan   
22000625 Jang Yeri



## libraries
The libraries and functions used are as follows.  

> __pandas__ : Library used to read data and generate dataframes  
> https://pandas.pydata.org/about/   

> __matplotlib__: Library used to plots various data in many ways   
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
## Installation   
-------------------------------
It was difficult to install the '_Basemap_' library. This problem was 
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
territory = pd.read_csv('South_Korea_territory.csv', sep = ',').to_numpy()
candidate = pd.read_csv('Vertiport_candidates.csv', sep = ',').to_numpy()
```

------------------
## User-Defined Functions
Prior to the explanation, several rules were established for clear understanding. 
Thick words in Italics refer to functions or variables. Adding () to the end is a 
function, and the other is a parameter of an internal variable. [] is direct reference 
to the annotation within the code

### 1. Calculate Distance Functions
> _cal_dist(P1,P2):_    
* _cal_dist()_ is a function that calculate the distance between two points based on the _Euclidean distance_. Given two points (P1 and P2) consisting of a pair of X and Y, the distance between these two points is calculated and returned
```python
def cal_dist(P1,P2):
    dist = np.sqrt(np.sum((P1 - P2) ** 2))
    return dist
```

### 2. Drawing Functions   
> _draw_plot(dots, centroids, color_d, color_c):_
* _draw_plot()_ is the drawing function of Problem 1.
* Information on eight points(_dots_), centroid points(_centroids_), information on the color of dots (_color_d_), and information on the color of centroids(_color_c_) are included as input values.
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
* _draw_map()_ is the drawing function of Problem 2.
* Information on the territory of the map(_territory_), information on the vertiport candidates(_candidate_), and information on the colors of the candidates(_color_) are input.
* In [# Draw a map], An appropriate map is drawn.
* In [# Mark 'territory' as a blue dot], the territory data entered is displayed as a blue dot on the map.   
* In [# Mark 'candidate' as a dot of the color specified], the candidate information is displayed on the map. 
* _color_ is an option to draw Problem 2.1 and 2.2 graphs with a single drawing function. Candidate points in 2.2 are drawn with different colors for each cluster, while in 2.1 are drown with one designated color. If the parameter color is given as a single color in string, the candidate points are displayed on the map in the same color specified. However, if the color is given as cluster information in the form of a list, each point is displayed in different colors for each cluster.   
* _Vertiports_ is an option to represent the finally selected vertiports in star-shaped dots. If the vertiports value is not entered or set to False, the basic type of dots are drown. On the other hand, if vertiport is set to True, the indexed star-shaped dots are drawn.

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
    
    # Mark 'territory' as a blue dot   
    for i in range(territory.shape[0]):
        x,y = map(territory[i][0],territory[i][1])
        map.plot(x, y, 'o', markersize=1, color='blue')
    
    # Mark 'candidate' as a dot of the color specified    
    for j in range(candidate.shape[0]):
        a,b = map(candidate[j][0],candidate[j][1])
        
        # Specify the color of the point
        # If the color is entered in string type
        if type(color) is str:
            col = color
        # If the color is entered as cluster information
        else:
            col = color_dict[color[j]]
        
        # If the "Vertiport" option is True, indexed star-shaped dots are drawn
        if vertiports:
            map.plot(a, b, markersize=6, color=col, marker='*')
            plt.annotate(j+1, (a, 1.05*b), size=8)
        
        # If not, basic type of dots are drown
        else: 
            map.plot(a, b, 'o', markersize=1, color=col)
    return plt
```
> _color_dict
* The color dictionary(_color_dict_) is used to change the index of each cluster to each color. _color_dict_ is dictionary that replaces int-type cluster data with color information, and it can specify colors up to 20 clusters.  

```python
# Match cluster information with color information
# Color dictionary range :0~19
color_dict = {0:'gold', 1:'lightpink', 2:'powderblue' ,3:'orchid',4:'yellowgreen'
              ,5:'limegreen',6:'olive',7:'lightskyblue',8:'slategray',9:'royalblue'
              ,10:'grey',11:'mistyrose',12:'darkslateblue',13:'darkorange', 14:'tomato'
              ,15:'peru',16:'lavender',17:'firebrick',18:'silver',19:'lightcoral'}
```

------------------------------------------
### 3. K-Means Algorithm Function    
> __kmeans(dots, K, auto = False, centroids = None):__   
* _kmeans()_ is a function of finding the centroid value through the K-Means algorithm, which is commonly used in Problems 1 and 2.
* _dots_ is information about the point of clustering, and _K_ is the number of clusters.   
* _auto_ is option to decide whether to implement the K-Means algorithm only once or until the final centroid point is found. When _auto_ is False, max_iter is designated as 1, and the K-Means algorithm is implemented only once and terminated. On the other hand, when _auto_ is true, the function repeats until it finds a centroid point that meets the criteria.  
* _centroids_ is an option for using pre-determined centroid values or randomly generated ones.
* If no value is passed to the _centroids_ parameter, the default value None is entered in _centroids_. Accordingly, K random centroid values are generated.
* _max_iter_, a variable declared within the function, means the maximum number of iterations to prevent infinite iterations when repeatedly trying to find the centroid value. 
* _theta_ is the reference value of the negligible difference when selecting the final centroid value.
* _cluster_ is a list used to store cluster information of each point. If K is 10, the information of each cluster is stored as number between 0 and 9.
* _cnt_ is a variable for recording how many times the K-Means algorithm has been repeated before finding the final centroid values.
* _dist_ is a list that stores the distance from one point value to K centroids in order. For one point, the distance to K centroids is repeatedly calculated and accumulated in _dist_. When the distance values between one point and K centroids are obtained, the index of the smallest value among the distance values is added to the _cluster_ through the _argmin()_ function. In this way, the distance between K centroid values and N points is calculated, and the index of the nearest centroid of each point is all added to the _cluster_. Thus, the cluster value of each point is specified. 
* In [# == update centroids ==], the newly calculated centroid values are updated. The new centroid values are obtained by averaging the points corresponding to each cluster.
* In [# == Termination ==], If the distance difference between the previous centroid values and the newly updated centroid values is smaller than the reference value, it is judged as the final centroid and the function is terminated. The sse variable, the sum of the squares of the difference between the K previous centroid values and the updated K centroid values, is used. If this sse value is less than theta value specified earlier, it is considered a negligible difference and the function is terminated. And at the end of kmeans() function, the final centroid values, the iteration number of the K-Means algorithm, and the final cluster information of N points are returned.
* In [# == Termination == ], the sse value is the sum of squares of the differences between the previous K centroid values and the updated K centroid values. If the distance difference between the previous centroids and the newly updated centroids is smaller than the _theta_, it is considered that there is no in the centroid values and the function is terminated. And the final centrid values, the number of iterations of the K-Means algorithm, and the final cluster information of N points are returned.

```python
def kmeans(dots, K, auto=False, centroids=None):
    
    max_iter=100 # Maximum iteration count
    theta=0.00005 # Small negligible value
    
    # Rows and columns of data
    N, dim = dots.shape 
    
    # Initialize variables
    sse = np.Inf 
    cnt = 0
    
    # Generate K random centroid values
    if centroids is None:
        centroids = np.zeros((K, dim))
        for k in range(K):
            rand_index = rd.randint(0, N-1)
            centroids[k] = dots[rand_index]
    
    # If 'auto' is False, perform iteration only once
    if not auto:
        max_iter = 1
    
    # Repeat the k-means algorithm as much as max_iteration
    for i in range(max_iter):
        
        cluster =[] # Storing cluster information for each point
        cnt = cnt+1 # Update iteration count
        
        # k-means algorithm
        for n in range(N):
            
            # Stores the distance between each point and K centroid points
            dist = []
            
            # Calculate the distance between one point and K centroid points
            for k in range(K):
                dist.append(cal_dist(dots[n], centroids[k]))
            cluster.append(np.argmin(dist))
        
        
        # == Update centroids ==
        prev_centroids = centroids # Save previous centroids
        centroids = np.zeros((K, dim)) # Initialize
        
        # Calculate new K centroids
        for k in range(K):
            
            # Initialization
            tmp_sum = np.zeros(dim) 
            tmp_count = 0
            
            # Calculate the average of points belonging to each cluster
            for n in range(N):
                if cluster[n] == k:
                    tmp_sum += dots[n]
                    tmp_count += 1
            centroids[k] = tmp_sum / tmp_count
        
        
        # == Termination ==
        # Terminate the function if the difference between the prev_centroids and updated centroids is negligible
        
        prev_sse = sse
        sse = 0
        
        # Calculate SSE value
        for k in range(K):
            sse += cal_dist(prev_centroids[k], centroids[k]) ** 2

        if prev_sse - sse < theta:
            break
    
    return centroids, cnt, cluster
```

-----------------------------------------------------------
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


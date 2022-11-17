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

1. Calculate Distance Functions
> __cal_dist(P1,P2):__


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
 

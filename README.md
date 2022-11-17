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

Additionally, it was difficult to install the 'Basemap' library. This problem was solved by installing a library as follows as a Conda virtual environment. After installing conda, we created a virtual environment named project and activated it. Then, ‘basemap-data-hires’ and ‘basemap’ were installed in order. 
```
conda create -n project   
conda activate project   
conda install -c conda-forge basemap-data-hires   
conda install -c conda-forge basemap   
```
링크는 다음과 같이 작성할 수 있습니다.

[블로그 주소](https://blog.naver.com/ndb796)

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
 

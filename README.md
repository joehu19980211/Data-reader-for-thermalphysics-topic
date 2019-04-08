# Data-reader-for-thermalphysics-topic
It's a code of loading text data of several Japanese cities' temperature, in order to calculate when the sakura

import numpy as np
import matplotlib.pyplot as plt
from glob import glob
import numpy as np
import matplotlib.pyplot as plt
from glob import glob

### 
# 讀取檔案
以dictionary儲存data，地名為呼叫data的keyword:
## dict:
### 'tokyo':
#### [data1],[data2],......
### 'kyoto':
#### [data1],[data2],......
### ......
def DataReader(files_dir):
    # Set up dictionary of data with keyword of city name
    up_files = glob( files_dir + '*' )
    data_dict = {}
    key_list = []
    for i in up_files:
        key = i.split('\\')[-1]
        key_list.append(key)
        data_dict[ key ] = []
    
    # Store data of each city in data_dict
    files = glob( files_dir + '*/*.txt' )
    N = len( files )
    for i in range(N):
        with open( files[i] ) as data:
            kw = files[i].split('\\')[-2]
            for lines in data:
                time,T = lines.split('\t')[:2]
                date,h = time.split(' ')
                y,m,d = date.split('/')
                data_dict[ kw ].append([ int(y[2:]), int(m), int(d), int(h[:2]), float(T) ])
    # Convert to array
    for i in key_list:
        data_dict[i] = np.array(data_dict[i])
    
    return data_dict

files_dir = 'C:/Users/user/Desktop/thermal_physics_topic/data/'
X = DataReader(files_dir)
print(X['tokyo'])
'''
[[ 9.  10.   1.   1.  18.3]
 [ 9.  10.   1.   2.  18.7]
 [ 9.  10.   1.   3.  18.6]
 ...
 [18.   5.  30.  22.  18. ]
 [18.   5.  30.  23.  17.9]
 [18.   5.  31.   0.  17.7]]
'''

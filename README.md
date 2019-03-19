# ML_Project2



## Part 1


A frequency refers to the pitch of the voice. In the dataset it is meanfreq so we assume it is the average of the frequency of the voice over a recording's time.

A median refers to the point that seperates the higher half of the sample from the lower half, in this case we assume it is the middle value of the sorted dataset.

An output label is used to determine which sex the voice belongs to.

## Part 2



```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

data = pd.read_csv('voice.csv')

data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3168 entries, 0 to 3167
    Data columns (total 21 columns):
    meanfreq    3168 non-null float64
    sd          3168 non-null float64
    median      3168 non-null float64
    Q25         3168 non-null float64
    Q75         3168 non-null float64
    IQR         3168 non-null float64
    skew        3168 non-null float64
    kurt        3168 non-null float64
    sp.ent      3168 non-null float64
    sfm         3168 non-null float64
    mode        3168 non-null float64
    centroid    3168 non-null float64
    meanfun     3168 non-null float64
    minfun      3168 non-null float64
    maxfun      3168 non-null float64
    meandom     3168 non-null float64
    mindom      3168 non-null float64
    maxdom      3168 non-null float64
    dfrange     3168 non-null float64
    modindx     3168 non-null float64
    label       3168 non-null object
    dtypes: float64(20), object(1)
    memory usage: 519.8+ KB
    


```python
# feature names as a list
col = data.columns       # .columns gives columns names in data 
print(col)
```

    Index(['meanfreq', 'sd', 'median', 'Q25', 'Q75', 'IQR', 'skew', 'kurt',
           'sp.ent', 'sfm', 'mode', 'centroid', 'meanfun', 'minfun', 'maxfun',
           'meandom', 'mindom', 'maxdom', 'dfrange', 'modindx', 'label'],
          dtype='object')
    


```python
data.loc[:,'label'].value_counts()
```




    female    1584
    male      1584
    Name: label, dtype: int64




```python
y = data.label.values
print(np.array(y))
```

    ['male' 'male' 'male' ... 'female' 'female' 'female']
    


```python
X = np.array(data.drop(["label"],axis=1))
print(X)
```
<details>
    <summary>Output</summary>

          meanfreq        sd    median       Q25       Q75       IQR       skew  \
    0     0.059781  0.064241  0.032027  0.015071  0.090193  0.075122  12.863462   
    1     0.066009  0.067310  0.040229  0.019414  0.092666  0.073252  22.423285   
    2     0.077316  0.083829  0.036718  0.008701  0.131908  0.123207  30.757155   
    3     0.151228  0.072111  0.158011  0.096582  0.207955  0.111374   1.232831   
    4     0.135120  0.079146  0.124656  0.078720  0.206045  0.127325   1.101174   
    5     0.132786  0.079557  0.119090  0.067958  0.209592  0.141634   1.932562   
    6     0.150762  0.074463  0.160106  0.092899  0.205718  0.112819   1.530643   
    7     0.160514  0.076767  0.144337  0.110532  0.231962  0.121430   1.397156   
    8     0.142239  0.078018  0.138587  0.088206  0.208587  0.120381   1.099746   
    9     0.134329  0.080350  0.121451  0.075580  0.201957  0.126377   1.190368   
    10    0.157021  0.071943  0.168160  0.101430  0.216740  0.115310   0.979442   
    11    0.138551  0.077054  0.127527  0.087314  0.202739  0.115426   1.626770   
    12    0.137343  0.080877  0.124263  0.083145  0.209227  0.126082   1.378728   
    13    0.181225  0.060042  0.190953  0.128839  0.229532  0.100693   1.369430   
    14    0.183115  0.066982  0.191233  0.129149  0.240152  0.111004   3.568104   
    15    0.174272  0.069411  0.190874  0.115602  0.228279  0.112677   4.485038   
    16    0.190846  0.065790  0.207951  0.132280  0.244357  0.112076   1.562304   
    17    0.171247  0.074872  0.152807  0.122391  0.243617  0.121227   3.207170   
    18    0.168346  0.074121  0.145618  0.115756  0.239824  0.124068   2.704335   
    19    0.173631  0.073352  0.153569  0.123680  0.244234  0.120554   2.804975   
    20    0.172754  0.076903  0.177736  0.120070  0.245368  0.125298   2.967765   
    21    0.181015  0.074369  0.169299  0.128673  0.254175  0.125502   2.587325   
    22    0.163536  0.072449  0.145543  0.113930  0.227449  0.113519   3.587650   
    23    0.170213  0.075105  0.146053  0.123989  0.250126  0.126137   2.816793   
    24    0.160422  0.076615  0.144824  0.120924  0.237244  0.116319   6.253208   
    25    0.164700  0.075362  0.147018  0.118698  0.240475  0.121777   4.208608   
    26    0.169579  0.075635  0.186468  0.116706  0.238549  0.121843   4.269923   
    27    0.169021  0.071778  0.143168  0.125801  0.248315  0.122515   3.079273   
    28    0.167340  0.072841  0.141739  0.122174  0.240000  0.117826   2.192126   
    29    0.180528  0.070867  0.142385  0.129541  0.252477  0.122936   2.799969   
    ...        ...       ...       ...       ...       ...       ...        ...   
    3138  0.114477  0.081973  0.090199  0.041095  0.199900  0.158806   1.103680   
    3139  0.112769  0.074424  0.094248  0.049183  0.183235  0.134052   0.945953   
    3140  0.126439  0.079412  0.127325  0.046889  0.198993  0.152103   1.452173   
    3141  0.117350  0.090035  0.109478  0.024017  0.203946  0.179929   2.610623   
    3142  0.104793  0.085201  0.077886  0.028388  0.186101  0.157712   2.419127   
    3143  0.127633  0.084931  0.158892  0.034531  0.201430  0.166899   1.591174   
    3144  0.091250  0.086956  0.048191  0.015193  0.179043  0.163851   3.089787   
    3145  0.082404  0.085136  0.035114  0.016920  0.152827  0.135906   2.570944   
    3146  0.124695  0.080989  0.131882  0.042033  0.197268  0.155234   1.970756   
    3147  0.131566  0.084354  0.131889  0.053093  0.196147  0.143055   2.243370   
    3148  0.108888  0.092021  0.070063  0.022520  0.201180  0.178660   2.235435   
    3149  0.090445  0.079045  0.059358  0.020893  0.167727  0.146834   2.187161   
    3150  0.137507  0.091521  0.161298  0.043547  0.221260  0.177713   1.119608   
    3151  0.113148  0.090335  0.084335  0.026622  0.198830  0.172207   2.258273   
    3152  0.149731  0.082852  0.180932  0.060212  0.219788  0.159576   1.240037   
    3153  0.189614  0.035933  0.194116  0.168434  0.205289  0.036855   2.724415   
    3154  0.200097  0.045533  0.203796  0.176581  0.232133  0.055552   1.160197   
    3155  0.178573  0.046679  0.164388  0.149309  0.204601  0.055293   3.066668   
    3156  0.201806  0.036057  0.201622  0.178165  0.227872  0.049707   1.585353   
    3157  0.203627  0.041529  0.204104  0.175661  0.239122  0.063461   1.462972   
    3158  0.183667  0.040607  0.182534  0.156480  0.207646  0.051166   2.054138   
    3159  0.168794  0.085842  0.188980  0.095558  0.240229  0.144671   1.462248   
    3160  0.151771  0.089147  0.185970  0.058159  0.230199  0.172040   1.227710   
    3161  0.170656  0.081237  0.184277  0.113012  0.239096  0.126084   1.378256   
    3162  0.146023  0.092525  0.183434  0.041747  0.224337  0.182590   1.384981   
    3163  0.131884  0.084734  0.153707  0.049285  0.201144  0.151859   1.762129   
    3164  0.116221  0.089221  0.076758  0.042718  0.204911  0.162193   0.693730   
    3165  0.142056  0.095798  0.183731  0.033424  0.224360  0.190936   1.876502   
    3166  0.143659  0.090628  0.184976  0.043508  0.219943  0.176435   1.591065   
    3167  0.165509  0.092884  0.183044  0.070072  0.250827  0.180756   1.705029   
    
                 kurt    sp.ent       sfm      mode  centroid   meanfun    minfun  \
    0      274.402906  0.893369  0.491918  0.000000  0.059781  0.084279  0.015702   
    1      634.613855  0.892193  0.513724  0.000000  0.066009  0.107937  0.015826   
    2     1024.927705  0.846389  0.478905  0.000000  0.077316  0.098706  0.015656   
    3        4.177296  0.963322  0.727232  0.083878  0.151228  0.088965  0.017798   
    4        4.333713  0.971955  0.783568  0.104261  0.135120  0.106398  0.016931   
    5        8.308895  0.963181  0.738307  0.112555  0.132786  0.110132  0.017112   
    6        5.987498  0.967573  0.762638  0.086197  0.150762  0.105945  0.026230   
    7        4.766611  0.959255  0.719858  0.128324  0.160514  0.093052  0.017758   
    8        4.070284  0.970723  0.770992  0.219103  0.142239  0.096729  0.017957   
    9        4.787310  0.975246  0.804505  0.011699  0.134329  0.105881  0.019300   
    10       3.974223  0.965249  0.733693  0.096358  0.157021  0.088894  0.022069   
    11       6.291365  0.966004  0.752042  0.012101  0.138551  0.104199  0.019139   
    12       5.008952  0.963514  0.736150  0.108434  0.137343  0.092644  0.016789   
    13       5.475600  0.937446  0.537080  0.219827  0.181225  0.131504  0.025000   
    14      35.384748  0.940333  0.571394  0.049987  0.183115  0.102799  0.020833   
    15      61.764908  0.950972  0.635199  0.050027  0.174272  0.102046  0.018328   
    16       7.834350  0.938546  0.538810  0.050129  0.190846  0.113323  0.017544   
    17      25.765565  0.936954  0.586420  0.059958  0.171247  0.079718  0.015671   
    18      18.484703  0.934523  0.559742  0.060033  0.168346  0.083484  0.015717   
    19      20.857543  0.930917  0.518269  0.060027  0.173631  0.090130  0.015702   
    20      20.078115  0.925539  0.523081  0.059953  0.172754  0.093574  0.015764   
    21      12.281432  0.915284  0.475317  0.059957  0.181015  0.098643  0.016145   
    22      28.653781  0.927015  0.542422  0.059941  0.163536  0.062542  0.015686   
    23      13.764582  0.913832  0.487966  0.059944  0.170213  0.077698  0.015702   
    24      85.491926  0.933030  0.567424  0.060078  0.160422  0.098944  0.016097   
    25      43.681885  0.940669  0.604020  0.059965  0.164700  0.082963  0.015640   
    26      45.895248  0.929498  0.543709  0.059966  0.169579  0.082451  0.016211   
    27      14.340299  0.902275  0.477746  0.128148  0.169021  0.130598  0.015842   
    28       8.152410  0.913763  0.539479  0.134783  0.167340  0.120052  0.016244   
    29      12.190361  0.853115  0.313426  0.133578  0.180528  0.126607  0.017039   
    ...           ...       ...       ...       ...       ...       ...       ...   
    3138     3.759184  0.954130  0.689347  0.009751  0.114477  0.189394  0.016343   
    3139     3.290904  0.965719  0.742464  0.199020  0.112769  0.169836  0.017917   
    3140     5.582106  0.971946  0.790175  0.214432  0.126439  0.179613  0.029575   
    3141    12.442898  0.953624  0.701434  0.004603  0.117350  0.178040  0.016512   
    3142    11.281968  0.956977  0.718462  0.008492  0.104793  0.183565  0.016444   
    3143     5.347645  0.949757  0.671247  0.182163  0.127633  0.178363  0.058182   
    3144    12.857558  0.930715  0.622816  0.011925  0.091250  0.170893  0.016178   
    3145     9.179264  0.921649  0.576089  0.014555  0.082404  0.183387  0.034043   
    3146     8.000504  0.958531  0.721682  0.197712  0.124695  0.182513  0.068966   
    3147    11.544740  0.968324  0.784108  0.195515  0.131566  0.191163  0.029144   
    3148     8.528681  0.947621  0.679795  0.011260  0.108888  0.160473  0.019512   
    3149     8.221164  0.942404  0.628992  0.011911  0.090445  0.182431  0.026622   
    3150     4.185207  0.967706  0.739008  0.008682  0.137507  0.190093  0.019116   
    3151     9.579337  0.957433  0.717683  0.018431  0.113148  0.187444  0.023495   
    3152     4.019385  0.949787  0.652936  0.000000  0.149731  0.183974  0.051948   
    3153    10.986864  0.871215  0.236684  0.195616  0.189614  0.163059  0.029685   
    3154     3.733815  0.919607  0.357144  0.176901  0.200097  0.168531  0.063241   
    3155    15.684088  0.891448  0.321169  0.153032  0.178573  0.155380  0.025478   
    3156     4.945634  0.884731  0.227903  0.176117  0.201806  0.191704  0.032720   
    3157     4.790370  0.903458  0.246953  0.208821  0.203627  0.146783  0.020566   
    3158     7.483019  0.898138  0.313925  0.177040  0.183667  0.149237  0.018648   
    3159     5.077956  0.956201  0.706861  0.184442  0.168794  0.182863  0.020699   
    3160     4.304354  0.962045  0.744590  0.230547  0.151771  0.201600  0.023426   
    3161     5.431663  0.950750  0.658558  0.161506  0.170656  0.198475  0.160000   
    3162     5.118927  0.948999  0.659825  0.215482  0.146023  0.195640  0.039506   
    3163     6.630383  0.962934  0.763182  0.200836  0.131884  0.182790  0.083770   
    3164     2.503954  0.960716  0.709570  0.013683  0.116221  0.188980  0.034409   
    3165     6.604509  0.946854  0.654196  0.008006  0.142056  0.209918  0.039506   
    3166     5.388298  0.950436  0.675470  0.212202  0.143659  0.172375  0.034483   
    3167     5.769115  0.938829  0.601529  0.267702  0.165509  0.185607  0.062257   
    
            maxfun   meandom    mindom    maxdom   dfrange   modindx  
    0     0.275862  0.007812  0.007812  0.007812  0.000000  0.000000  
    1     0.250000  0.009014  0.007812  0.054688  0.046875  0.052632  
    2     0.271186  0.007990  0.007812  0.015625  0.007812  0.046512  
    3     0.250000  0.201497  0.007812  0.562500  0.554688  0.247119  
    4     0.266667  0.712812  0.007812  5.484375  5.476562  0.208274  
    5     0.253968  0.298222  0.007812  2.726562  2.718750  0.125160  
    6     0.266667  0.479620  0.007812  5.312500  5.304688  0.123992  
    7     0.144144  0.301339  0.007812  0.539062  0.531250  0.283937  
    8     0.250000  0.336476  0.007812  2.164062  2.156250  0.148272  
    9     0.262295  0.340365  0.015625  4.695312  4.679688  0.089920  
    10    0.117647  0.460227  0.007812  2.812500  2.804688  0.200000  
    11    0.262295  0.246094  0.007812  2.718750  2.710938  0.132351  
    12    0.213333  0.481671  0.015625  5.015625  5.000000  0.088500  
    13    0.275862  1.277114  0.007812  2.804688  2.796875  0.416550  
    14    0.275862  1.245739  0.203125  6.742188  6.539062  0.139332  
    15    0.246154  1.621299  0.007812  7.000000  6.992188  0.209311  
    16    0.275862  1.434115  0.007812  6.320312  6.312500  0.254780  
    17    0.262295  0.106279  0.007812  0.570312  0.562500  0.138355  
    18    0.231884  0.146563  0.007812  3.125000  3.117188  0.059537  
    19    0.210526  0.193044  0.007812  2.820312  2.812500  0.068124  
    20    0.200000  0.235877  0.007812  0.718750  0.710938  0.235069  
    21    0.275862  0.209844  0.007812  3.695312  3.687500  0.059940  
    22    0.197531  0.059622  0.007812  0.445312  0.437500  0.091699  
    23    0.192771  0.101562  0.007812  0.562500  0.554688  0.161791  
    24    0.275862  0.206756  0.007812  3.953125  3.945312  0.073890  
    25    0.253968  0.143353  0.007812  1.062500  1.054688  0.125926  
    26    0.271186  0.148438  0.007812  3.609375  3.601562  0.050841  
    27    0.225352  0.335313  0.007812  0.710938  0.703125  0.397354  
    28    0.262295  0.298678  0.007812  0.679688  0.671875  0.384778  
    29    0.177778  0.234863  0.007812  0.507812  0.500000  0.329241  
    ...        ...       ...       ...       ...       ...       ...  
    3138  0.271186  0.566840  0.007812  4.273438  4.265625  0.183258  
    3139  0.262295  0.877604  0.007812  4.937500  4.929688  0.171708  
    3140  0.266667  0.614110  0.007812  4.914062  4.906250  0.090045  
    3141  0.271186  0.188721  0.007812  0.750000  0.742188  0.277759  
    3142  0.275862  0.297953  0.007812  0.859375  0.851562  0.370904  
    3143  0.271186  0.634014  0.015625  5.031250  5.015625  0.121246  
    3144  0.275862  0.767314  0.007812  5.289062  5.281250  0.187912  
    3145  0.275862  0.328962  0.007812  0.750000  0.742188  0.445053  
    3146  0.238806  0.293527  0.007812  0.851562  0.843750  0.396091  
    3147  0.275862  0.214725  0.007812  0.796875  0.789062  0.351645  
    3148  0.275862  0.497721  0.007812  2.945312  2.937500  0.236240  
    3149  0.258065  0.735453  0.007812  5.531250  5.523438  0.170489  
    3150  0.275862  0.354367  0.007812  3.117188  3.109375  0.096069  
    3151  0.262295  0.622489  0.007812  4.898438  4.890625  0.128717  
    3152  0.253968  1.361213  0.203125  6.031250  5.828125  0.365700  
    3153  0.258065  1.370192  0.164062  7.000000  6.835938  0.235948  
    3154  0.262295  0.718750  0.148438  7.000000  6.851562  0.092208  
    3155  0.253968  0.637921  0.148438  6.148438  6.000000  0.101291  
    3156  0.275862  0.593750  0.007812  5.921875  5.914062  0.124383  
    3157  0.262295  0.875558  0.171875  6.898438  6.726562  0.145534  
    3158  0.262295  0.550312  0.007812  3.421875  3.414062  0.166503  
    3159  0.271186  0.988281  0.007812  5.882812  5.875000  0.268617  
    3160  0.266667  0.766741  0.007812  4.007812  4.000000  0.192220  
    3161  0.253968  0.414062  0.007812  0.734375  0.726562  0.336918  
    3162  0.275862  0.533854  0.007812  2.992188  2.984375  0.258924  
    3163  0.262295  0.832899  0.007812  4.210938  4.203125  0.161929  
    3164  0.275862  0.909856  0.039062  3.679688  3.640625  0.277897  
    3165  0.275862  0.494271  0.007812  2.937500  2.929688  0.194759  
    3166  0.250000  0.791360  0.007812  3.593750  3.585938  0.311002  
    3167  0.271186  0.227022  0.007812  0.554688  0.546875  0.350000  
 
 </details>

    [3168 rows x 20 columns]



```python
#PART 2

from sklearn.model_selection import KFold

folder = KFold(n_splits=10, shuffle = True)

for training_indices, testing_indices in folder.split(X):
    x_train = X[training_indices]
    y_train = y[training_indices]
    x_test = X[testing_indices]
    y_test = y[testing_indices]
    print(x_train.shape, x_test.shape, y_train.shape, y_test.shape)
```

    (2851, 20) (317, 20) (2851,) (317,)
    (2851, 20) (317, 20) (2851,) (317,)
    (2851, 20) (317, 20) (2851,) (317,)
    (2851, 20) (317, 20) (2851,) (317,)
    (2851, 20) (317, 20) (2851,) (317,)
    (2851, 20) (317, 20) (2851,) (317,)
    (2851, 20) (317, 20) (2851,) (317,)
    (2851, 20) (317, 20) (2851,) (317,)
    (2852, 20) (316, 20) (2852,) (316,)
    (2852, 20) (316, 20) (2852,) (316,)
    

## Part 3


```python
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC
from sklearn.tree import DecisionTreeClassifier
from sklearn.pipeline import Pipeline, make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.neighbors import KNeighborsClassifier

pipeline1 = make_pipeline(StandardScaler(), LogisticRegression(solver='lbfgs', multi_class='multinomial'))
pipeline2 = make_pipeline(StandardScaler(), SVC(gamma='scale'))
pipeline3 = make_pipeline(StandardScaler(), DecisionTreeClassifier())
pipeline4 = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=5))
```

## Part4

What does "score" mean?
The score is a comparison between the labels that a model has predicted for a training set and the actual real labels. It is given as a value from 0 to 1, and can also be viewed as percentage of correctness.

Why are the scores different?
Because they use different parts of the data. The data was split randomly through kfold to uniformalize the training of the model.


```python
from sklearn.model_selection import cross_val_score

for model in [pipeline1 ,pipeline2, pipeline3, pipeline4]:
    print("Score:", cross_val_score(model, X, y, cv = 10))
```

    Score: [0.91194969 0.96855346 0.96855346 0.97484277 0.96202532 0.98734177
     0.98417722 0.9778481  0.94620253 0.98417722]
    Score: [0.93081761 0.95597484 0.96855346 0.9591195  0.96835443 0.99683544
     0.98734177 0.98101266 0.91455696 0.99367089]
    Score: [0.90251572 0.90566038 0.96540881 0.93396226 0.96518987 0.98417722
     0.99367089 0.9778481  0.90189873 0.95886076]
    Score: [0.90566038 0.93710692 0.9591195  0.92767296 0.95886076 0.98734177
     0.98734177 0.98417722 0.89873418 0.9778481 ]
    

## Part 5


```python
from sklearn.model_selection import cross_val_score

pipelines = [pipeline1 ,pipeline2, pipeline3, pipeline4]
models = ["Logistic Regression", "Support Vector Machine", "Decision Tree", "kNN"]

for i, model in enumerate(pipelines):
    print(models[i], cross_val_score(model, X, y, cv = 10).mean())
```

    Logistic Regression 0.9665671522967918
    Support Vector Machine 0.9656237560703765
    Decision Tree 0.9514469389379826
    kNN 0.9523863545896027
    

## Part 6


```python
meanValues = []

for i in range(1, 11):
    model = make_pipeline(StandardScaler(), KNeighborsClassifier(n_neighbors=i))
    scores = cross_val_score(model,X, y, cv=10)    
    print(i, scores.mean())
    meanValues.append(scores.mean())

# Getting the best k

best = max(meanValues)

print("Best k value: ", meanValues.index(best) + 1)
```

    1 0.9441983122362869
    2 0.9476833054693099
    3 0.950497571849375
    4 0.9527207228723829
    5 0.9523863545896027
    6 0.9520838309051827
    7 0.9501811161531727
    8 0.9520758697555929
    9 0.9508080566833852
    10 0.9514449486505854
    11 0.9460811241143222
    12 0.9460811241143222
    13 0.9457626781307222
    14 0.9448172916169095
    Best k value:  4
    

#### Conclusion

For fun we tried to  run it 100 times, however the best k value is still 4 


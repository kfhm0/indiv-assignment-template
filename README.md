import pandas as pd
import numpy as np
from datetime import datetime as dt

import matplotlib.pyplot as plt
import seaborn as sns

from pandas_datareader import data as pdr
import yfinance as yf 
yf.pdr_override()
In [25]:
#adding columns for other financial variables 
data['X1'] = (data.BS_TOT_CAP / data.BS_TOT_ASSET) # Working Capital Ratio
data['X2'] = data.BS_PURE_RETAINED_EARNINGS / data.TANGIBLE_ASSETS # return on invested capital (ROIC)
data['X3'] = data.TRAIL_12M_OPER_INC / data.TANGIBLE_ASSETS # return on assets (ROA)
data['X4'] = data.CUR_MKT_CAP / data.BS_TOT_LIAB2 # leverage ratio
data['X5'] = data.TRAIL_12M_NET_SALES / data.TANGIBLE_ASSETS # asset turnover ratio

data['ZScore'] = 6.56 * data.X1 + 3.26 * data.X2 + 6.72 * data.X3 + 1.05 * data.X4 + 0 *data.X5
In [29]:
data = pd.read_excel('/Users/kenzafahim/Desktop/PGE NEOMA /Msc Finance & Big Data /Data VIZ/bankingstability.xlsx')
data
Out[29]:	
Bank	Date	TANGIBLE_ASSETS	BS_PURE_RETAINED_EARNINGS	TRAIL_12M_OPER_INC	BS_TOT_LIAB2	TRAIL_12M_NET_SALES	CUR_MKT_CAP	BS_TOT_ASSET	BS_TOT_CAP
0	BNP FP equity	2019-03-31	2272480	1918	9172	2174793	42862	53253.9167	2284496	568726
1	BNP FP equity	2019-06-30	2361238	66025	9069	2264138	42880	52197.8369	2372620	580127
2	BNP FP equity	2019-09-30	2498656	6324	9308	2398596	43424	55828.5017	2510204	581733
3	BNP FP equity	2019-12-31	2153044	69549	10057	2052868	44597	66026.8580	2164713	510694
4	BNP FP equity	2020-03-31	2661714	1282	9436	2559799	44341	34381.9584	2673276	601753
...	...	...	...	...	...	...	...	...	...	...
195	MS US equity	2022-12-31	1155961	94862	14089	1079000	65936	143693.1028	1180231	546864
196	MS US equity	2023-03-31	1175777	96392	13261	1097950	73742	147674.3456	1199904	578620
197	MS US equity	2023-06-30	1140937	97151	12754	1063550	82775	142627.7092	1164911	543640
198	MS US equity	2023-09-30	1145110	98007	12511	1068855	90799	135324.4606	1169013	564431
199	MS US equity	2023-12-31	1169931	97996	11813	1093711	96194	153052.3048	1193693	575732
200 rows × 10 columns

In [30]:
data.columns
Out[30]:	
Index(['Bank', 'Date', 'TANGIBLE_ASSETS', 'BS_PURE_RETAINED_EARNINGS',
       'TRAIL_12M_OPER_INC', 'BS_TOT_LIAB2', 'TRAIL_12M_NET_SALES',
       'CUR_MKT_CAP', 'BS_TOT_ASSET', 'BS_TOT_CAP'],
      dtype='object')
In [31]:
data.to_excel("Data banking stabFilled.xlsx")
In [32]:
data.set_index("Date", inplace=True)
In [33]:
data.describe()
Out[33]:	
TANGIBLE_ASSETS	BS_PURE_RETAINED_EARNINGS	TRAIL_12M_OPER_INC	BS_TOT_LIAB2	TRAIL_12M_NET_SALES	CUR_MKT_CAP	BS_TOT_ASSET	BS_TOT_CAP
count	2.000000e+02	200.000000	200.000000	2.000000e+02	200.0000	200.000000	2.000000e+02	2.000000e+02
mean	1.864667e+06	113282.645000	17060.800000	1.744287e+06	71006.2100	130466.803127	1.888965e+06	6.182872e+05
std	8.914066e+05	82597.013928	14158.146566	8.335049e+05	43474.4133	118619.106812	9.044150e+05	2.784349e+05
min	3.255720e+05	1282.000000	-2811.000000	3.046630e+05	15946.0000	9658.458600	3.461320e+05	1.067060e+05
25%	1.295769e+06	34796.500000	5705.000000	1.231942e+06	33801.2500	37403.928525	1.301234e+06	4.508655e+05
50%	1.742750e+06	99997.500000	12522.500000	1.613322e+06	59807.5000	89688.825100	1.758957e+06	6.049445e+05
75%	2.557470e+06	178045.750000	23697.750000	2.405459e+06	100738.0000	176491.617475	2.592499e+06	8.474518e+05
max	3.903496e+06	332901.000000	63523.000000	3.668788e+06	239425.0000	491760.518800	3.954687e+06	1.193471e+06
In [44]:
#Visualize the Bank's differences on their total capital fot the last 5 years. 
plt.figure(figsize=(10, 6))
# Use groupby and sum to aggregate total assets for each bank over time
data_grouped = data.groupby(['Bank', data.index])[['BS_TOT_CAP']].sum()

# Plot total assets for each bank
for bank in data_grouped.index.get_level_values('Bank').unique():
    plt.plot(data_grouped.loc[bank].index, data_grouped.loc[bank]['BS_TOT_CAP'], marker='o', linestyle='-', label=bank)

plt.title('Plot of Total Capital by Date')
plt.xlabel('Date')
plt.ylabel('Total capital')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

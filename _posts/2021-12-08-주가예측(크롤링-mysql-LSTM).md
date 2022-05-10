---
title: "주가예측 (크롤링, mysql, LSTM)"
toc: true
toc_sticky: true
excerpt_separator: "<!--more-->"
author_profile: true
sidebar:
  nav: "main"
categories:
  - Post Formats
tags:
   - python
   - 크롤링
   - 데이터 분석
   - sql
---



# 주가 예측

- mysql 설치 -> db INVESTAR 생성 
- Analyzer.py 를 이용해 종가 크롤링, mysql 저장
- LSTM 으로 종가 예측

```
import pandas as pd
import pymysql
from datetime import datetime
from datetime import timedelta
import re

class MarketDB:
    def __init__(self):
        """생성자: mysql 연결 및 종목코드 딕셔너리 생성"""
        self.conn = pymysql.connect(host='localhost', user='root', 
            password='1111', db='INVESTAR', charset='utf8')
        self.codes = {}
        self.get_comp_info()
        
    def __del__(self):
        """소멸자: mysql 연결 해제"""
        self.conn.close()

    def get_comp_info(self):
        """company_info 테이블에서 읽어와서 codes에 저장"""
        sql = "SELECT * FROM company_info"
        krx = pd.read_sql(sql, self.conn)
        for idx in range(len(krx)):
            self.codes[krx['code'].values[idx]] = krx['company'].values[idx]

    def get_daily_price(self, code, start_date=None, end_date=None):
        """KRX 종목의 일별 시세를 데이터프레임 형태로 반환
            - code       : KRX 종목코드('005930') 또는 상장기업명('삼성전자')
            - start_date : 조회 시작일('2020-01-01'), 미입력 시 1년 전 오늘
            - end_date   : 조회 종료일('2020-12-31'), 미입력 시 오늘 날짜
        """
        if start_date is None:
            one_year_ago = datetime.today() - timedelta(days=365)
            start_date = one_year_ago.strftime('%Y-%m-%d')
            print("start_date is initialized to '{}'".format(start_date))
        else:
            start_lst = re.split('\D+', start_date)
            if start_lst[0] == '':
                start_lst = start_lst[1:]
            start_year = int(start_lst[0])
            start_month = int(start_lst[1])
            start_day = int(start_lst[2])
            if start_year < 1900 or start_year > 2200:
                print(f"ValueError: start_year({start_year:d}) is wrong.")
                return
            if start_month < 1 or start_month > 12:
                print(f"ValueError: start_month({start_month:d}) is wrong.")
                return
            if start_day < 1 or start_day > 31:
                print(f"ValueError: start_day({start_day:d}) is wrong.")
                return
            start_date=f"{start_year:04d}-{start_month:02d}-{start_day:02d}"

        if end_date is None:
            end_date = datetime.today().strftime('%Y-%m-%d')
            print("end_date is initialized to '{}'".format(end_date))
        else:
            end_lst = re.split('\D+', end_date)
            if end_lst[0] == '':
                end_lst = end_lst[1:] 
            end_year = int(end_lst[0])
            end_month = int(end_lst[1])
            end_day = int(end_lst[2])
            if end_year < 1800 or end_year > 2200:
                print(f"ValueError: end_year({end_year:d}) is wrong.")
                return
            if end_month < 1 or end_month > 12:
                print(f"ValueError: end_month({end_month:d}) is wrong.")
                return
            if end_day < 1 or end_day > 31:
                print(f"ValueError: end_day({end_day:d}) is wrong.")
                return
            end_date = f"{end_year:04d}-{end_month:02d}-{end_day:02d}"
         
        codes_keys = list(self.codes.keys())
        codes_values = list(self.codes.values())

        if code in codes_keys:
            pass
        elif code in codes_values:
            idx = codes_values.index(code)
            code = codes_keys[idx]
        else:
            print(f"ValueError: Code({code}) doesn't exist.")
        sql = f"SELECT * FROM daily_price WHERE code = '{code}'"\
            f" and date >= '{start_date}' and date <= '{end_date}'"
        df = pd.read_sql(sql, self.conn)
        df.index = df['date']
        return df 



        

```





# LSTM 주가 예측

```python
from tensorflow.keras import Sequential
from tensorflow.keras.layers import Dense, LSTM, Dropout

import numpy as np
import matplotlib.pyplot as plt
import Analyzer
```


```python
mk = Analyzer.MarketDB()
raw_df = mk.get_daily_price('005930', '2019-01-01','2021-12-10')   

window_size = 10  #10일선
data_size = 5
```


```python
def MinMaxScaler(data):
    "최소값과 최대값을 이용하여 0~1 값으로 변환"
    numerator = data - np.min(data,0)
    denominator = np.max(data,0) - np.min(data,0)
    #0으로 나누기 에러가 발생하지 않도록 매우 작은 값(1e-7)을 더해서 나눔
    return numerator/(denominator+1e-7)
```


```python
dfx = raw_df[['open','high','low','volume','close']]
print(dfx)
```

                 open   high    low    volume  close
    date                                            
    2019-01-02  39400  39400  38550   7847664  38750
    2019-01-03  38300  38550  37450  12471493  37600
    2019-01-04  37450  37600  36850  14108958  37450
    2019-01-07  38000  38900  37800  12748997  38750
    2019-01-08  38000  39200  37950  12756554  38100
    ...           ...    ...    ...       ...    ...
    2021-12-01  72000  74800  71600  21954856  74400
    2021-12-02  73900  75800  73800  23652940  75800
    2021-12-03  75600  76000  74100  18330240  75600
    2021-12-06  75100  76700  74900  16391250  76300
    2021-12-07  76100  77700  75600  18081692  77400
    
    [725 rows x 5 columns]



```python
dfx = MinMaxScaler(dfx)
print(dfx)
```

                    open      high       low    volume     close
    date                                                        
    2019-01-02  0.036897  0.030405  0.032289  0.040060  0.024276
    2019-01-03  0.016083  0.016047  0.011396  0.093888  0.002801
    2019-01-04  0.000000  0.000000  0.000000  0.112951  0.000000
    2019-01-07  0.010407  0.021959  0.018044  0.097119  0.024276
    2019-01-08  0.010407  0.027027  0.020893  0.097207  0.012138
    ...              ...       ...       ...       ...       ...
    2021-12-01  0.653737  0.628378  0.660019  0.204289  0.690009
    2021-12-02  0.689688  0.645270  0.701804  0.224057  0.716153
    2021-12-03  0.721854  0.648649  0.707502  0.162093  0.712418
    2021-12-06  0.712394  0.660473  0.722697  0.139520  0.725490
    2021-12-07  0.731315  0.677365  0.735992  0.159199  0.746032
    
    [725 rows x 5 columns]



```python
dfy =dfx[['close']]
print(dfx) 
```

                    open      high       low    volume     close
    date                                                        
    2019-01-02  0.036897  0.030405  0.032289  0.040060  0.024276
    2019-01-03  0.016083  0.016047  0.011396  0.093888  0.002801
    2019-01-04  0.000000  0.000000  0.000000  0.112951  0.000000
    2019-01-07  0.010407  0.021959  0.018044  0.097119  0.024276
    2019-01-08  0.010407  0.027027  0.020893  0.097207  0.012138
    ...              ...       ...       ...       ...       ...
    2021-12-01  0.653737  0.628378  0.660019  0.204289  0.690009
    2021-12-02  0.689688  0.645270  0.701804  0.224057  0.716153
    2021-12-03  0.721854  0.648649  0.707502  0.162093  0.712418
    2021-12-06  0.712394  0.660473  0.722697  0.139520  0.725490
    2021-12-07  0.731315  0.677365  0.735992  0.159199  0.746032
    
    [725 rows x 5 columns]



```python
x= dfx.values.tolist()
y= dfy.values.tolist()
```


```python
data_x = []
data_y = []
```


```python
#x ->1일부터 10일치 y-> 11일차 close값   +1일 반복
for i in range(len(y)-window_size) :
    _x = x[i : i + window_size] # 다음 날 종가(i+window_size)는
    _y = y[i+window_size]#다음 날 종가
    data_x.append(_x)
    data_y.append(_y)
```


```python
print(data_x[0])
```

    [[0.03689687795641079, 0.030405405405354045, 0.03228869895530429, 0.04005992061946327, 0.024276377217508353], [0.016083254493820087, 0.01604729729727019, 0.01139601139598975, 0.09388818668705562, 0.0028011204481740407], [0.0, 0.0, 0.0, 0.11295072158970235, 0.0], [0.01040681173129535, 0.021959459459422365, 0.018043684710317105, 0.09711874714827101, 0.024276377217508353], [0.01040681173129535, 0.027027027026981374, 0.020892687559314543, 0.09720672189964542, 0.012138188608754177], [0.02270577105009895, 0.03378378378372671, 0.027540360873641898, 0.15187695884101762, 0.04014939309049458], [0.04824976348146026, 0.04307432432425156, 0.05223171889828636, 0.12020035565687881, 0.04388422035472664], [0.05487228003773913, 0.0498310810809969, 0.058879392212613714, 0.08445357271903066, 0.05695611577953883], [0.05676442762524737, 0.05236486486477641, 0.056980056979948755, 0.0882246359897687, 0.04855275443501671], [0.049195837275214385, 0.05912162162152175, 0.056980056979948755, 0.08249422820671451, 0.068160597572235]]



```python
print(data_y[0])
```

    [0.07469654528464109]



```python
# 7(train):3(test)
train_size = int(len(data_y)*0.7)
train_x = np.array(data_x[0: train_size])
train_y = np.array(data_y[0: train_size])

test_size = len(data_y) - train_size
test_x = np.array(data_x[train_size : len(data_x)])
test_y = np.array(data_y[train_size : len(data_y)])
```


```python
model = Sequential()
```


```python
model.add(LSTM(units=10, activation ='relu', return_sequences=True, 
              input_shape=(window_size , data_size)))     #input_shape 사이즈만 정하고 fit 하면 사이즈에 맞춰서 해준다
model.add(Dropout(0.1))
model.add(LSTM(units = 10 , activation='relu'))
model.add(Dropout(0.1))
model.add(Dense(units=1))
model.summary()
```

    Model: "sequential_3"
    _________________________________________________________________
     Layer (type)                Output Shape              Param #   
    =================================================================
     lstm_7 (LSTM)               (None, 10, 10)            640       
                                                                     
     dropout_7 (Dropout)         (None, 10, 10)            0         
                                                                     
     lstm_8 (LSTM)               (None, 10)                840       
                                                                     
     dropout_8 (Dropout)         (None, 10)                0         
                                                                     
     dense_3 (Dense)             (None, 1)                 11        
                                                                     
    =================================================================
    Total params: 1,491
    Trainable params: 1,491
    Non-trainable params: 0
    _________________________________________________________________



```python
model.compile(loss ='mean_squared_error', optimizer= 'adam') #mean_squared_error
```


```python
model.fit(train_x,train_y, epochs=60, batch_size =30)
pred_y = model.predict(test_x)                          #
print(pred_y)
```

    Epoch 1/60
    17/17 [==============================] - 4s 6ms/step - loss: 0.0915
    Epoch 2/60
    17/17 [==============================] - 0s 6ms/step - loss: 0.0521
    Epoch 3/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0185
    Epoch 4/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0124
    Epoch 5/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0083
    Epoch 6/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0093
    Epoch 7/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0074
    Epoch 8/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0072
    Epoch 9/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0080
    Epoch 10/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0065
    Epoch 11/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0073
    Epoch 12/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0065
    Epoch 13/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0066
    Epoch 14/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0063
    Epoch 15/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0063
    Epoch 16/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0070
    Epoch 17/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0075
    Epoch 18/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0047
    Epoch 19/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0046
    Epoch 20/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0053
    Epoch 21/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0054
    Epoch 22/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0061
    Epoch 23/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0064
    Epoch 24/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0062
    Epoch 25/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0061
    Epoch 26/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0050
    Epoch 27/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0059
    Epoch 28/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0046
    Epoch 29/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0048
    Epoch 30/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0043
    Epoch 31/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0049
    Epoch 32/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0051
    Epoch 33/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0044
    Epoch 34/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0058
    Epoch 35/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0055
    Epoch 36/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0053
    Epoch 37/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0042
    Epoch 38/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0043
    Epoch 39/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0052
    Epoch 40/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0037
    Epoch 41/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0036
    Epoch 42/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0049
    Epoch 43/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0055
    Epoch 44/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0045
    Epoch 45/60
    17/17 [==============================] - 0s 6ms/step - loss: 0.0033
    Epoch 46/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0039
    Epoch 47/60
    17/17 [==============================] - 0s 6ms/step - loss: 0.0039
    Epoch 48/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0037
    Epoch 49/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0041
    Epoch 50/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0042
    Epoch 51/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0033
    Epoch 52/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0029
    Epoch 53/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0032
    Epoch 54/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0042
    Epoch 55/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0034
    Epoch 56/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0035
    Epoch 57/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0029
    Epoch 58/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0027
    Epoch 59/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0028
    Epoch 60/60
    17/17 [==============================] - 0s 5ms/step - loss: 0.0032
    [[0.8431188 ]
     [0.838995  ]
     [0.8311686 ]
     [0.8308065 ]
     [0.839684  ]
     [0.8376981 ]
     [0.84196115]
     [0.82501715]
     [0.8188369 ]
     [0.79905593]
     [0.77803665]
     [0.7766985 ]
     [0.77061075]
     [0.77547234]
     [0.78276473]
     [0.79016197]
     [0.79858327]
     [0.80120647]
     [0.7998183 ]
     [0.7913428 ]
     [0.7901287 ]
     [0.79574424]
     [0.8060478 ]
     [0.80852675]
     [0.80480033]
     [0.813496  ]
     [0.8009395 ]
     [0.7815424 ]
     [0.76578563]
     [0.7511778 ]
     [0.7547182 ]
     [0.75814044]
     [0.7665498 ]
     [0.77085066]
     [0.7619324 ]
     [0.760892  ]
     [0.7541899 ]
     [0.7535681 ]
     [0.7624981 ]
     [0.7681916 ]
     [0.772734  ]
     [0.7676825 ]
     [0.7627911 ]
     [0.75970465]
     [0.76078814]
     [0.7602175 ]
     [0.7653578 ]
     [0.77291167]
     [0.7826775 ]
     [0.7970122 ]
     [0.80641156]
     [0.8084191 ]
     [0.80471   ]
     [0.8019458 ]
     [0.7997113 ]
     [0.79742557]
     [0.7943793 ]
     [0.78807986]
     [0.7850379 ]
     [0.7908717 ]
     [0.7920155 ]
     [0.79282427]
     [0.7898373 ]
     [0.78651696]
     [0.7843659 ]
     [0.77879214]
     [0.7748919 ]
     [0.7733896 ]
     [0.77083874]
     [0.7748191 ]
     [0.7727847 ]
     [0.7694845 ]
     [0.7666827 ]
     [0.7697894 ]
     [0.77886564]
     [0.78238904]
     [0.7884947 ]
     [0.7762907 ]
     [0.75061214]
     [0.7326053 ]
     [0.7148405 ]
     [0.7125166 ]
     [0.72130126]
     [0.72984993]
     [0.73353463]
     [0.7310639 ]
     [0.7311583 ]
     [0.7353962 ]
     [0.742452  ]
     [0.74416256]
     [0.75058734]
     [0.75836045]
     [0.76615316]
     [0.7652389 ]
     [0.7573256 ]
     [0.7506523 ]
     [0.74634516]
     [0.7469109 ]
     [0.7511359 ]
     [0.74767756]
     [0.74522275]
     [0.7449172 ]
     [0.74723035]
     [0.748631  ]
     [0.7487216 ]
     [0.7475847 ]
     [0.7463148 ]
     [0.7478885 ]
     [0.7490648 ]
     [0.7453061 ]
     [0.74233514]
     [0.7411703 ]
     [0.7397409 ]
     [0.7399872 ]
     [0.7398467 ]
     [0.73948604]
     [0.7443592 ]
     [0.7454319 ]
     [0.7434599 ]
     [0.7335111 ]
     [0.722691  ]
     [0.71589875]
     [0.7182114 ]
     [0.7202091 ]
     [0.72288543]
     [0.7218751 ]
     [0.71812564]
     [0.7167081 ]
     [0.7152201 ]
     [0.7138332 ]
     [0.715906  ]
     [0.72199494]
     [0.731437  ]
     [0.7413972 ]
     [0.74835825]
     [0.75742096]
     [0.7626523 ]
     [0.7604331 ]
     [0.7594628 ]
     [0.7653121 ]
     [0.765996  ]
     [0.76992106]
     [0.7412575 ]
     [0.6984502 ]
     [0.6614294 ]
     [0.6376436 ]
     [0.6246922 ]
     [0.6197486 ]
     [0.6293256 ]
     [0.64300793]
     [0.64712   ]
     [0.6492312 ]
     [0.6545595 ]
     [0.6598085 ]
     [0.666681  ]
     [0.66844684]
     [0.66979676]
     [0.66998404]
     [0.6686884 ]
     [0.66954786]
     [0.6744486 ]
     [0.675194  ]
     [0.67782706]
     [0.6774047 ]
     [0.6809116 ]
     [0.6811939 ]
     [0.6842397 ]
     [0.6890228 ]
     [0.6910401 ]
     [0.68808013]
     [0.6848676 ]
     [0.6807181 ]
     [0.67618066]
     [0.66496646]
     [0.654214  ]
     [0.64205426]
     [0.623886  ]
     [0.6127063 ]
     [0.6067418 ]
     [0.5959608 ]
     [0.5845804 ]
     [0.5789188 ]
     [0.57177156]
     [0.5634097 ]
     [0.55970824]
     [0.5652711 ]
     [0.57045305]
     [0.57568395]
     [0.58173513]
     [0.585686  ]
     [0.5881001 ]
     [0.5892447 ]
     [0.58950543]
     [0.58935946]
     [0.5888989 ]
     [0.5838171 ]
     [0.5817462 ]
     [0.5827567 ]
     [0.5809558 ]
     [0.58129424]
     [0.5817472 ]
     [0.58318317]
     [0.5835967 ]
     [0.5853616 ]
     [0.59164643]
     [0.60199213]
     [0.61597544]
     [0.6265936 ]
     [0.6310769 ]
     [0.6288254 ]
     [0.63055265]
     [0.6300947 ]
     [0.6346362 ]
     [0.6419091 ]
     [0.64816123]]



```python
plt.figure()
plt.plot(test_y, color= 'red', label ='real SEC stock price')
plt.plot(pred_y, color= 'blue', label = 'predicted SEC stock price')
plt.xlabel('time')
plt.ylabel('stock price')
plt.legend()
plt.show()
```


![png](\github_img\LSTM주가예측\output_16_0.png)
    



```python
print("Tomorrow's SEC price :",raw_df.close[-1],pred_y[-1],dfy.close[-1])
print("Tomorrow's SEC price :",raw_df.close[-1]*pred_y[-1]/dfy.close[-1],'KRW')

#raw_df.close[-1] : dfy.close[-1] = x : pred_y[-1]
```

    Tomorrow's SEC price : 77400 [0.64816123] 0.7460317460303528
    Tomorrow's SEC price : [67246.03833581] KRW






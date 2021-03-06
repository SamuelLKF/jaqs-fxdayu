
# HFDataView

## 介绍
HFDataview 是针对高频因子(bar级别)和事件设计的数据操作和信号计算模块，功能和Dataview一致

## HFDataView与DataView的区别

- 1.HFDataView使用分钟数据而不是日度、季度数据
- 2.支持传入一个multiindex的数据 用来初始化dv。要求:trade_date、symbol为index， fields为columns. 其中,trade_date要为整数类型
- 3.支持设置读取数据的时间索引格式（整数型 日期型）

## TODO LIST
- 1.分钟数据直接下载
- 2.多频率数据的自动对齐

## 使用案例


```python
from jaqs_fxdayu.data import RemoteDataService
```


```python
data_config = {
    "remote.data.address": "tcp://data.tushare.org:8910",
    "remote.data.username": "18566262672",
    "remote.data.password": "eyJhbGciOiJIUzI1NiJ9.eyJjcmVhdGVfdGltZSI6IjE1MTI3MDI3NTAyMTIiLCJpc3MiOiJhdXRoMCIsImlkIjoiMTg1NjYyNjI2NzIifQ.O_-yR0zYagrLRvPbggnru1Rapk4kiyAzcwYt2a3vlpM",
    "timeout":360    
}
```


```python
ds = RemoteDataService()
ds.init_from_config(data_config)
```

    
    Begin: DataApi login 18566262672@tcp://data.tushare.org:8910
        login success 
    





    '0,'




```python
df,msg = ds.bar("000001.SZ,000002.SZ", trade_date =20180605,  freq="1M")
```


```python
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>close</th>
      <th>code</th>
      <th>date</th>
      <th>freq</th>
      <th>high</th>
      <th>low</th>
      <th>oi</th>
      <th>open</th>
      <th>settle</th>
      <th>symbol</th>
      <th>time</th>
      <th>trade_date</th>
      <th>turnover</th>
      <th>volume</th>
      <th>vwap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.26</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.29</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.29</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93100</td>
      <td>20180605</td>
      <td>4337203.0</td>
      <td>422300.0</td>
      <td>10.270431</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.26</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.26</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93200</td>
      <td>20180605</td>
      <td>4376199.0</td>
      <td>426872.0</td>
      <td>10.251783</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.26</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.26</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93300</td>
      <td>20180605</td>
      <td>4369881.0</td>
      <td>426828.0</td>
      <td>10.238037</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.24</td>
      <td>10.22</td>
      <td>NaN</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93400</td>
      <td>20180605</td>
      <td>6379272.0</td>
      <td>623700.0</td>
      <td>10.228110</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.24</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.24</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93500</td>
      <td>20180605</td>
      <td>2555450.0</td>
      <td>249700.0</td>
      <td>10.234081</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 构造trade_date -- 时间索引 整数类型
df["trade_date"] = df["trade_date"]*1000000+df["time"]
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>close</th>
      <th>code</th>
      <th>date</th>
      <th>freq</th>
      <th>high</th>
      <th>low</th>
      <th>oi</th>
      <th>open</th>
      <th>settle</th>
      <th>symbol</th>
      <th>time</th>
      <th>trade_date</th>
      <th>turnover</th>
      <th>volume</th>
      <th>vwap</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10.26</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.29</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.29</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93100</td>
      <td>20180605093100</td>
      <td>4337203.0</td>
      <td>422300.0</td>
      <td>10.270431</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10.26</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.26</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93200</td>
      <td>20180605093200</td>
      <td>4376199.0</td>
      <td>426872.0</td>
      <td>10.251783</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.26</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.26</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93300</td>
      <td>20180605093300</td>
      <td>4369881.0</td>
      <td>426828.0</td>
      <td>10.238037</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.24</td>
      <td>10.22</td>
      <td>NaN</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93400</td>
      <td>20180605093400</td>
      <td>6379272.0</td>
      <td>623700.0</td>
      <td>10.228110</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.24</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.24</td>
      <td>NaN</td>
      <td>000001.SZ</td>
      <td>93500</td>
      <td>20180605093500</td>
      <td>2555450.0</td>
      <td>249700.0</td>
      <td>10.234081</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = df.set_index(["trade_date","symbol"])
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>close</th>
      <th>code</th>
      <th>date</th>
      <th>freq</th>
      <th>high</th>
      <th>low</th>
      <th>oi</th>
      <th>open</th>
      <th>settle</th>
      <th>time</th>
      <th>turnover</th>
      <th>volume</th>
      <th>vwap</th>
    </tr>
    <tr>
      <th>trade_date</th>
      <th>symbol</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20180605093100</th>
      <th>000001.SZ</th>
      <td>10.26</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.29</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.29</td>
      <td>NaN</td>
      <td>93100</td>
      <td>4337203.0</td>
      <td>422300.0</td>
      <td>10.270431</td>
    </tr>
    <tr>
      <th>20180605093200</th>
      <th>000001.SZ</th>
      <td>10.26</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.26</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>93200</td>
      <td>4376199.0</td>
      <td>426872.0</td>
      <td>10.251783</td>
    </tr>
    <tr>
      <th>20180605093300</th>
      <th>000001.SZ</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.26</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.26</td>
      <td>NaN</td>
      <td>93300</td>
      <td>4369881.0</td>
      <td>426828.0</td>
      <td>10.238037</td>
    </tr>
    <tr>
      <th>20180605093400</th>
      <th>000001.SZ</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.24</td>
      <td>10.22</td>
      <td>NaN</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>93400</td>
      <td>6379272.0</td>
      <td>623700.0</td>
      <td>10.228110</td>
    </tr>
    <tr>
      <th>20180605093500</th>
      <th>000001.SZ</th>
      <td>10.23</td>
      <td>000001</td>
      <td>20180605</td>
      <td>1M</td>
      <td>10.24</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.24</td>
      <td>NaN</td>
      <td>93500</td>
      <td>2555450.0</td>
      <td>249700.0</td>
      <td>10.234081</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 初始化dv
from jaqs_fxdayu.data import HFDataView
dv = HFDataView()
dv.create_init_dv(df)
```

    Initialize dataview success.



```python
dv.fields
```




    ['close',
     'code',
     'date',
     'freq',
     'high',
     'low',
     'oi',
     'open',
     'settle',
     'time',
     'turnover',
     'volume',
     'vwap']




```python
dv.symbol
```




    ['000001.SZ', '000002.SZ']




```python
dv.start_date
```




    20180605093100




```python
dv.end_date
```




    20180605150000




```python
# 取数据 索引为整型
dv.get_ts("close").head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>symbol</th>
      <th>000001.SZ</th>
      <th>000002.SZ</th>
    </tr>
    <tr>
      <th>trade_date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20180605093100</th>
      <td>10.26</td>
      <td>27.12</td>
    </tr>
    <tr>
      <th>20180605093200</th>
      <td>10.26</td>
      <td>27.13</td>
    </tr>
    <tr>
      <th>20180605093300</th>
      <td>10.23</td>
      <td>27.18</td>
    </tr>
    <tr>
      <th>20180605093400</th>
      <td>10.23</td>
      <td>27.19</td>
    </tr>
    <tr>
      <th>20180605093500</th>
      <td>10.23</td>
      <td>27.15</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 取数据 索引为日期型
dv.get_ts("close",date_type="datetime").head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>symbol</th>
      <th>000001.SZ</th>
      <th>000002.SZ</th>
    </tr>
    <tr>
      <th>trade_date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018-06-05 09:31:00</th>
      <td>10.26</td>
      <td>27.12</td>
    </tr>
    <tr>
      <th>2018-06-05 09:32:00</th>
      <td>10.26</td>
      <td>27.13</td>
    </tr>
    <tr>
      <th>2018-06-05 09:33:00</th>
      <td>10.23</td>
      <td>27.18</td>
    </tr>
    <tr>
      <th>2018-06-05 09:34:00</th>
      <td>10.23</td>
      <td>27.19</td>
    </tr>
    <tr>
      <th>2018-06-05 09:35:00</th>
      <td>10.23</td>
      <td>27.15</td>
    </tr>
  </tbody>
</table>
</div>




```python
dv.add_formula(field_name="momentum",formula="Return(close,20)",add_data=True,overwrite=True).head()
```

    Field [momentum] is overwritten.





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>symbol</th>
      <th>000001.SZ</th>
      <th>000002.SZ</th>
    </tr>
    <tr>
      <th>trade_date</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20180605093100</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20180605093200</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20180605093300</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20180605093400</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20180605093500</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
data = dv.get_symbol("000001.SZ",fields="momentum,open,high,low,close",date_type="datetime")
data.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>field</th>
      <th>close</th>
      <th>high</th>
      <th>low</th>
      <th>momentum</th>
      <th>open</th>
    </tr>
    <tr>
      <th>trade_date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018-06-05 09:31:00</th>
      <td>10.26</td>
      <td>10.29</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>2018-06-05 09:32:00</th>
      <td>10.26</td>
      <td>10.26</td>
      <td>10.25</td>
      <td>NaN</td>
      <td>10.25</td>
    </tr>
    <tr>
      <th>2018-06-05 09:33:00</th>
      <td>10.23</td>
      <td>10.26</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.26</td>
    </tr>
    <tr>
      <th>2018-06-05 09:34:00</th>
      <td>10.23</td>
      <td>10.24</td>
      <td>10.22</td>
      <td>NaN</td>
      <td>10.23</td>
    </tr>
    <tr>
      <th>2018-06-05 09:35:00</th>
      <td>10.23</td>
      <td>10.24</td>
      <td>10.23</td>
      <td>NaN</td>
      <td>10.24</td>
    </tr>
  </tbody>
</table>
</div>




```python
import matplotlib.pyplot as plt
data.plot(kind="line",figsize=(12,10),secondary_y=["momentum"])
plt.show()
```


![png](output_17_0.png)


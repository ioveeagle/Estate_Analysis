# Estate_Analysis
周圍不動產數據分析

## Project Description
輸入特定地點及範圍，利用spark分析大量資料，回傳範圍內成交的不動產數據分析

## Data

內政部 不動產交易實價查詢服務網

https://plvr.land.moi.gov.tw/DownloadOpenData

 ## Data Preprcessing

 **NearBy estate  Data pre-processing.ipynb**

 ### 取得經緯度

 `!pip install geocoder`

 ```python
 def get_geographic_coordinates(address):
    try:
      ret = geocoder.arcgis(address)
      return(ret.latlng)
    except:
      #print ERROR ADDRESS if address can't be parsed  
      print("ERROR")
 ```

 ### 將類別型資料轉成數值型資料

    Example

    {'transaction_sign': {'num': 2, '房地(土地+建物)': 1, '房地(土地+建物)+車位': 2}}

將處理好的資料存入 **pre_processing_data**

## NearBy estate.ipynb




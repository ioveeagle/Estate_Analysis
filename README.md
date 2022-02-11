# Estate_Analysis

周圍不動產數據分析

## Project Description

輸入特定地點及範圍，利用spark分析大量資料，回傳範圍指定類型不動產成交數據分析

挑出離群值後做成dataset 可用來訓練模型

## Data

內政部 不動產交易實價查詢服務網

https://plvr.land.moi.gov.tw/DownloadOpenData

 ## Data Preprcessing

 **NearBy estate  Data pre-processing.ipynb**
 
 [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1jbTUWmZQKcX9Bi3Z1kogu3EIuGUjjMJG?usp=sharing)

 ### 查經緯度

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
 
 方便未來做成dataset訓練模型

Example

    {'transaction_sign': {'num': 2, '房地(土地+建物)': 1, '房地(土地+建物)+車位': 2}}

將處理好的資料存入 **pre_processing_data**

## Search

 **NearBy estate.ipynb**
 
 [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ZCkgZ3bVR2xlqSDQj9IIAvaMqO0sTDcr?usp=sharing)


### 周圍房屋資料分布

```python
address=input("請輸入欲查詢之地址: ")
building_state=input("請輸入欲查詢之房屋類別: ")
range=input("請輸入查詢範圍(單位:KM): ")
nearby_infor(int(building_state),address,float(range))
 ```

將輸入的地址轉為經緯度，找出與目標地址特定距離特定類型之不動產

### 查詢距離

```python
 def getDistance(latA, lonA, latB, lonB):
  try:
    #print(latA, lonA, latB, lonB)
    ra = 6378140  # 赤道半徑
    rb = 6356755  # 極半徑
    flatten = (ra - rb) / ra  # Partial rate of the earth
    # change angle to radians
    radLatA = math.radians(latA)
    radLonA = math.radians(lonA)
    radLatB = math.radians(latB)
    radLonB = math.radians(lonB)

    pA = math.atan(rb / ra * math.tan(radLatA))
    pB = math.atan(rb / ra * math.tan(radLatB))
    x = math.acos(math.sin(pA) * math.sin(pB) + math.cos(pA) * math.cos(pB) * math.cos(radLonA - radLonB))
    c1 = (math.sin(x) - x) * (math.sin(pA) + math.sin(pB)) ** 2 / math.cos(x / 2) ** 2
    c2 = (math.sin(x) + x) * (math.sin(pA) - math.sin(pB)) ** 2 / math.sin(x / 2) ** 2
    dr = flatten / 8 * (c1 - c2)
    distance = ra * (x + dr)
    distance = round(distance / 1000, 4)
    return distance
  except:
    return 0
 ```

### Example
    
    請輸入欲查詢之地址: 台中市西區大忠南街
    請輸入欲查詢之房屋類別 1.住宅大樓(11層含以上有電梯),2.公寓(5樓含以下無電梯),3.華廈(10層含以下有電梯),4.透天,5.全部: 1
    請輸入查詢範圍(單位:KM): 1
    --------------------------------------------------
    查詢的地點周遭有18筆資料
    --------------------------------------------------
    transaction_sign:
    房地(土地+建物)+車位 66.67%
    房地(土地+建物) 33.33%
    --------------------------------------------------
    type:
    都市：其他:第二種住宅區 77.78%
    都市：其他:第四種商業區 5.56%
    非都市： ; 非都市編定： 5.56%
    都市：其他:第三種住宅區。 11.11%
    --------------------------------------------------
    building_state:
    住宅大樓(11層含以上有電梯) 100.0%
    --------------------------------------------------
    main_use:
    住家用 83.33%
    集合住宅 16.67%
    --------------------------------------------------
    building_materials:
    鋼筋混凝土造 94.44%
    ＲＣ造 5.56%
    --------------------------------------------------
    berth_category:
    坡道平面 50.0%
    坡道機械 11.11%
    0 33.33%
    升降平面 5.56%
    --------------------------------------------------
    residential_guard:
    1 100.0%
    --------------------------------------------------
    elevator:
    1 100.0%
    --------------------------------------------------
    land_shifting_total_area平均為5.193
    --------------------------------------------------
    building_shifting_total_area平均為52.688
    --------------------------------------------------
    transaction_land_num平均為1.056
    --------------------------------------------------
    transaction_building_num平均為1.0
    --------------------------------------------------
    transaction_berth_num平均為0.833
    --------------------------------------------------
    building_age平均為7704.944
    --------------------------------------------------
    building_room_num平均為2.722
    --------------------------------------------------
    building_hall_num平均為1.833
    --------------------------------------------------
    building_bathroom_num平均為1.722
    --------------------------------------------------
    building_compartmented_num平均為1.0
    --------------------------------------------------
    building_price平均為13610277.778
    --------------------------------------------------
    unit_price平均為77233.333
    --------------------------------------------------
    berth_area平均為2.163
    --------------------------------------------------
    berth_price平均為144444.444
    --------------------------------------------------

### 計算離群值

```python
#以總價找離群值輸入:building_price
#以單坪價找離群值輸入:unit_price
building_state=input("請輸入欲查詢之房屋類別 1.住宅大樓(11層含以上有電梯),2.公寓(5樓含以下無電梯),3.華廈(10層含以下有電梯),4.透天,5.全部: ")
find_different("unit_price",building_state)
```





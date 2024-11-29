```
import os
import re
import pandas as pd
from datetime import date

# 此处替换需要合并报告的文件夹路径
brand = 'Brand'
date_br = '202407'
country = 'US'
path_root = r'F:\OneDrive\Client Report'

path_raw = os.path.join(path_root, brand, 'BR Report', 'Raw Data', date_br)
path_date = os.path.join(path_root, brand, 'BR Report', 'Month Concat')

filenames = os.listdir(path_raw)

# 构建源表

df = pd.DataFrame(columns=[
    # 自定义日期列
    'Date',
    
    # 基本信息
    '(Parent) ASIN','(Child) ASIN','Title',

    # Session相关
    'Sessions - Mobile App','Sessions - Mobile APP - B2B',
    'Sessions - Browser','Sessions - Browser - B2B',
    'Sessions - Total','Sessions - Total - B2B',
    'Session Percentage - Mobile App','Session Percentage - Mobile APP - B2B',
    'Session Percentage - Browser','Session Percentage - Browser - B2B',
    'Session Percentage - Total','Session Percentage - Total - B2B',

    # Page View相关
    'Page Views - Mobile App','Page Views - Mobile APP - B2B',
    'Page Views - Browser','Page Views - Browser - B2B',
    'Page Views - Total','Page Views - Total - B2B',
    'Page Views Percentage - Mobile App','Page Views Percentage - Mobile App - B2B',
    'Page Views Percentage - Browser','Page Views Percentage - Browser - B2B',
    'Page Views Percentage - Total','Page Views Percentage - Total - B2B',

    # BBX相关
    'Featured Offer (Buy Box) Percentage','Featured Offer (Buy Box) Percentage - B2B',

    # Unit相关
    'Units Ordered','Units Ordered - B2B',
    'Unit Session Percentage','Unit Session Percentage - B2B',

    # Sales相关
    'Ordered Product Sales','Ordered Product Sales - B2B',
    'Total Order Items','Total Order Items - B2B'
])

for filename in filenames:
    # 日期处理
    date_match = re.search(r'\b(\d{4})(\d{2})(\d{2})\b', filename)
    date_yyyy = int(date_match.group(1))
    date_mm = int(date_match.group(2))
    date_dd = int(date_match.group(3))
    date_now = date(date_yyyy, date_mm, date_dd)

    # 向其中添加日期列
    df_daily = pd.read_csv(path_raw+ '\\' + filename)
    df_daily['Date'] = date_now

    if country == "US":
        df = pd.concat([df, df_daily], ignore_index=True, sort=False)

# 数据字段处理

def cvt_percentage(value):
    value = str(value).replace('%', '')
    value = float(value)/100
    return value

def cvt_currency(value):
    value = str(value).replace(',', '').replace('.', '')
    value = str(value).replace('$', '')
    value = float(value)
    return value

def cvt_int(value):
    value = str(value).replace(',', '').replace('.', '')
    value = int(value)
    return value

# 定义列名到转换函数的映射字典
columns_mapping = {
    # Session相关
    'Sessions - Mobile App': cvt_int,
    'Sessions - Mobile APP - B2B': cvt_int,
    'Sessions - Browser': cvt_int,
    'Sessions - Browser - B2B': cvt_int,
    'Sessions - Total': cvt_int,
    'Sessions - Total - B2B': cvt_int,
    'Session Percentage - Mobile App': cvt_percentage,
    'Session Percentage - Mobile APP - B2B': cvt_percentage,
    'Session Percentage - Browser': cvt_percentage,
    'Session Percentage - Browser - B2B': cvt_percentage,
    'Session Percentage - Total': cvt_percentage,
    'Session Percentage - Total - B2B': cvt_percentage,
    
    # PV相关
    'Page Views - Mobile App': cvt_int,
    'Page Views - Mobile APP - B2B': cvt_int,
    'Page Views - Browser': cvt_int,
    'Page Views - Browser - B2B': cvt_int,
    'Page Views - Total': cvt_int,
    'Page Views - Total - B2B': cvt_int,
    'Page Views Percentage - Mobile App': cvt_percentage,
    'Page Views Percentage - Mobile App - B2B': cvt_percentage,
    'Page Views Percentage - Browser': cvt_percentage,
    'Page Views Percentage - Browser - B2B': cvt_percentage,
    'Page Views Percentage - Total': cvt_percentage,
    'Page Views Percentage - Total - B2B': cvt_percentage,
    
    # BBX相关
    'Featured Offer (Buy Box) Percentage': cvt_percentage,
    'Featured Offer (Buy Box) Percentage - B2B': cvt_percentage,
    
    # Unit相关
    'Units Ordered': cvt_int,
    'Units Ordered - B2B': cvt_int,
    'Total Order Items': cvt_int,
    'Total Order Items - B2B': cvt_int,
    'Unit Session Percentage': cvt_percentage,
    'Unit Session Percentage - B2B': cvt_percentage,
    
    # 货币相关
    'Ordered Product Sales': cvt_currency,
    'Ordered Product Sales - B2B': cvt_currency
}

# 遍历字典并应用转换
for col, func in columns_mapping.items():
    df[col] = df[col].apply(func)

# 此处修改合并后文件的保存路径

df.to_excel(path_date + '\\' + date_br + '.xlsx', index=False, sheet_name=country)
```

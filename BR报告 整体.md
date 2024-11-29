```
import os
import pandas as pd

brand = 'brand'
region = 'US'
brand_region = brand + ' ' +region

path_head = 'D://Client//{}//{}//{} BR Report'.format(brand, brand_region, brand_region).replace('//', '\\')
path_in = '{}//{} BR Report Monthly'.format(path_head, brand_region).replace('//', '\\')
path_out = '{}//{} BR Report Concat.xlsx'.format(path_head, brand_region).replace('//', '\\')

filenames = os.listdir(path_in)
path_names = []
db_br = []

for filename in filenames:
    path_names.append(os.path.join(path_in, filename).replace('\\', '/'))

for path_name in path_names:
    db = pd.read_excel(path_name)
    db_br.append(db)

br = pd.concat(db_br)
br['Date'] = br['Date'].dt.date

write = pd.ExcelWriter(path_out)
br.to_excel(write, index=False, sheet_name='Business Report')
write.close()
```

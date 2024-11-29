# 说明

这段脚本用于在power pivot的日期表中, 添加一列Events, 用于将固定的时间范围内, 定义为特殊事件, 可以根据需求自由补充

```
=
VAR CurrentYear = YEAR('Date'[Date])
RETURN

SWITCH(
    TRUE(),
    
    -- 2023年7月PD时间划分
    'Date'[Date] >= DATE(2023, 7, 01) && 'Date'[Date] <= DATE(2023, 7, 10), "2023 PD 01 Pre",
    'Date'[Date] >= DATE(2023, 7, 11) && 'Date'[Date] <= DATE(2023, 7, 12), "2023 PD 02 Deal",
    'Date'[Date] >= DATE(2023, 7, 13) && 'Date'[Date] <= DATE(2023, 7, 16), "2023 PD 03 Post",
    'Date'[Date] >= DATE(2023, 7, 17) && 'Date'[Date] <= DATE(2023, 7, 31), "2023 PD 04 Daily",
    
    -- 2023年10月FPD时间划分
    'Date'[Date] >= DATE(2023, 10, 01) && 'Date'[Date] <= DATE(2023, 10, 09), "2023 FPD 01 Pre",
    'Date'[Date] >= DATE(2023, 10, 10) && 'Date'[Date] <= DATE(2023, 10, 11), "2023 FPD 02 Deal",
    'Date'[Date] >= DATE(2023, 10, 12) && 'Date'[Date] <= DATE(2023, 10, 15), "2023 FPD 03 Post",
    'Date'[Date] >= DATE(2023, 10, 16) && 'Date'[Date] <= DATE(2023, 10, 31), "2023 FPD 04 Daily",

    -- 2023年11月BFCM时间划分
    'Date'[Date] >= DATE(2023, 11, 01) && 'Date'[Date] <= DATE(2023, 11, 16), "2023 BFCM 01 Pre",
    'Date'[Date] >= DATE(2023, 11, 17) && 'Date'[Date] <= DATE(2023, 11, 17), "2023 BFCM 02 Deal Day1",
    'Date'[Date] >= DATE(2023, 11, 18) && 'Date'[Date] <= DATE(2023, 11, 23), "2023 BFCM 03 Deal",
    'Date'[Date] >= DATE(2023, 11, 24) && 'Date'[Date] <= DATE(2023, 11, 24), "2023 BFCM 04 Deal BF",
    'Date'[Date] >= DATE(2023, 11, 25) && 'Date'[Date] <= DATE(2023, 11, 26), "2023 BFCM 05 Deal BFCM",
    'Date'[Date] >= DATE(2023, 11, 27) && 'Date'[Date] <= DATE(2023, 11, 27), "2023 BFCM 06 Deal CM",
    'Date'[Date] >= DATE(2023, 11, 28) && 'Date'[Date] <= DATE(2023, 11, 30), "2023 BFCM 07 Post",
    
    -- 2023年12月XMAS时间划分
    'Date'[Date] >= DATE(2023, 12, 01) && 'Date'[Date] <= DATE(2023, 12, 10), "2023 XMAS 01",
    'Date'[Date] >= DATE(2023, 12, 11) && 'Date'[Date] <= DATE(2023, 12, 17), "2023 XMAS 02",
    'Date'[Date] >= DATE(2023, 12, 18) && 'Date'[Date] <= DATE(2023, 12, 24), "2023 XMAS 03",
    'Date'[Date] >= DATE(2023, 12, 25) && 'Date'[Date] <= DATE(2023, 12, 25), "2023 XMAS 04 XMAS",
    'Date'[Date] >= DATE(2023, 12, 26) && 'Date'[Date] <= DATE(2023, 12, 31), "2023 XMAS 04",

    -- 2023剩余时间标记为2023 Daily
    CurrentYear = 2023, "2023 Daily",
    
    -- 2024年7月PD的时间段的划分
    'Date'[Date] >= DATE(2024, 7, 01) && 'Date'[Date] <= DATE(2024, 7, 15), "2024 PD 01 Pre",
    'Date'[Date] >= DATE(2024, 7, 16) && 'Date'[Date] <= DATE(2024, 7, 17), "2024 PD 02 Deal",
    'Date'[Date] >= DATE(2024, 7, 18) && 'Date'[Date] <= DATE(2024, 7, 21), "2024 PD 03 Post",
    'Date'[Date] >= DATE(2024, 7, 22) && 'Date'[Date] <= DATE(2024, 7, 31), "2024 PD 04 Daily",

    -- 2024年10月FPD的时间段划分
    'Date'[Date] >= DATE(2024, 10, 01) && 'Date'[Date] <= DATE(2024, 10, 07), "2024 FPD 01 Pre",
    'Date'[Date] >= DATE(2024, 10, 08) && 'Date'[Date] <= DATE(2024, 10, 09), "2024 FPD 02 Deal",
    'Date'[Date] >= DATE(2024, 10, 10) && 'Date'[Date] <= DATE(2024, 10, 14), "2024 FPD 03 Post",
    'Date'[Date] >= DATE(2024, 10, 15) && 'Date'[Date] <= DATE(2024, 10, 31), "2024 FPD 04 Daily",

    -- 2024年11月BFCM的时间段划分
    'Date'[Date] >= DATE(2024, 11, 01) && 'Date'[Date] <= DATE(2024, 11, 20), "2024 BFCM 01 Pre 01",
    'Date'[Date] >= DATE(2024, 11, 21) && 'Date'[Date] <= DATE(2024, 11, 21), "2024 BFCM 02 Deal Day1",
    'Date'[Date] >= DATE(2024, 11, 22) && 'Date'[Date] <= DATE(2024, 11, 28), "2024 BFCM 03 Deal",
    'Date'[Date] >= DATE(2024, 11, 29) && 'Date'[Date] <= DATE(2024, 11, 29), "2024 BFCM 04 Deal BF",
    'Date'[Date] >= DATE(2024, 11, 30) && 'Date'[Date] <= DATE(2024, 12, 01), "2024 BFCM 05 Deal BFCM",
    'Date'[Date] >= DATE(2024, 12, 02) && 'Date'[Date] <= DATE(2024, 12, 02), "2024 BFCM 06 Deal CM",


    -- 2024年12月XMAS的时间段划分
    'Date'[Date] >= DATE(2024, 12, 03) && 'Date'[Date] <= DATE(2024, 12, 08), "2024 XMAS 01",
    'Date'[Date] >= DATE(2024, 12, 09) && 'Date'[Date] <= DATE(2024, 12, 15), "2024 XMAS 02",
    'Date'[Date] >= DATE(2024, 12, 16) && 'Date'[Date] <= DATE(2024, 12, 22), "2024 XMAS 03",
    'Date'[Date] >= DATE(2024, 12, 23) && 'Date'[Date] <= DATE(2024, 12, 24), "2024 XMAS 04",
    'Date'[Date] >= DATE(2024, 12, 25) && 'Date'[Date] <= DATE(2024, 12, 25), "2024 XMAS 05 XMAS",
    'Date'[Date] >= DATE(2024, 12, 26) && 'Date'[Date] <= DATE(2024, 12, 31), "2024 XMAS 06",
    
   -- 2024剩余时间标记为2024 Daily
   CurrentYear = 2024, "2024 Daily"
)

```

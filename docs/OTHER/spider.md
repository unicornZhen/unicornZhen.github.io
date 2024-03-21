## 简介

爬取http://www.infobank.cn多线程爬虫，涉及的技术有表格操作(openpyxl)、简单的爬虫(requests,bs4)、python多线程、网页解析（bs4）

## 采用的技术简介

openpyxl：涉及execel表格的操作，从表格中读取存储所需要的请求股票代码和股票年份，将爬取的结果以Excel表格存储下来

bs4：解析网页文件html的解析器，本质上是一颗网页树，节点是各级标签

多线程：很简单，不过python目前的多线程只能利用到单核的性能

## 源码

```python
import requests
from bs4 import BeautifulSoup
import re
import openpyxl
from multiprocessing.dummy import Pool
import time
import os


# 从输入表格获取数据
def read_excel(file, start):
    wb = openpyxl.load_workbook(file)
    # 存储所有的股票代码及其对应的年份，每个股票代码自身构成一个集合,stock_ids=[[stock_id,year_start,year_end,index],[...],[stock_id,year_start,year_end,index]]
    stock_ids = []
    stock_id_years = []
    prefer = "-1"
    sheet = wb['Sheet2']
    # 爬取开始位置：(1:2),(2:1576)
    end = sheet.max_row + 1
    for i in range(start, end):
        r = sheet[i]
        if r[0].value == prefer:
            stock_id_years[2] = r[1].value
        else:
            if len(stock_id_years) == 4:
                stock_id_years[0].replace(' ', '')
                stock_id_years[1].replace(' ', '')
                stock_id_years[2].replace(' ', '')
                stock_id_years[1] = int(stock_id_years[1])
                stock_id_years[2] = int(stock_id_years[2])
                stock_ids.append(stock_id_years)
            prefer = r[0].value
            stock_id_years = []
            stock_id_years.append(r[0].value)
            stock_id_years.append(r[1].value)
            stock_id_years.append(r[1].value)
            stock_id_years.append(i)
    if len(stock_id_years) == 4:
        stock_id_years[0].replace(' ', '')
        stock_id_years[1].replace(' ', '')
        stock_id_years[2].replace(' ', '')
        stock_id_years[1] = int(stock_id_years[1])
        stock_id_years[2] = int(stock_id_years[2])
        stock_ids.append(stock_id_years)
    return stock_ids


# 存储数据
def save_data(results, file_name):
    # results是一个存储某个股票id的爬取数据结果，将股票id存到excel中
    wb = openpyxl.load_workbook(file_name)
    ws = wb.active
    results_total_num = 0
    for result in results:
        for recoder in result:
            recoder[1] = recoder[1][0:4] + '-' + recoder[1][4:6] + '-' + recoder[1][6:]
            ws.append(recoder)
        results_total_num += len(result)
    wb.save(file_name)
    return results_total_num


# 爬取数据并存储
def craw_stockId_allpages(stock_id_years):
    print(stock_id_years)
    # 根据股票id及其对应的年份爬取数据
    result = []
    url = "http://www.infobank.cn/IrisBin/Search.dll?Page"
    headers = {
        "Host": "www.infobank.cn",
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36 Edg/110.0.1587.46"
    }
    library_values = [['HK', '中国经济新闻库'], ['BG', '中国商业报告库'], ['FL', '中国法律法规库'],
                      ['WK', 'English Publications'],
                      ['TJ', '中国统计数据库'], ['SS', '中国上市公司文献库'], ['JK', '中国医疗健康库'],
                      ['RW', '中国人物库'],
                      ['KX', 'INFOBANK环球商讯库'], ['OG', '中国中央及地方政府机构库'],
                      ['PJ', '中国拟建在建项目数据库'],
                      ['QY', '中国企业产品库'], ['HS', '香港上市公司资料库(中文)'], ['MC', '名词解释库']]
    # library_values=[['BG', '中国商业报告库']]
    for library_value in library_values:
        data = {
            "cmd": library_value[0] + ":@@=" + stock_id_years[0],
            "ns": "50",
            "pn": 0,
        }
        total_page_num = 0
        page_num = 0
        while True:
            flag = True
            data['pn'] = page_num
            resp = requests.post(url, data=data, headers=headers)
            if resp.status_code != 200:
                continue
            resp.encoding = "gb2312"
            # 使用正则表达式#,page_num.需要匹配换行,re.S
            title_pattern = r'<td\s*valign="top"\s*>\s*<a.*\/a><\/td>'
            date_pattern = r'<td\s*width="10&#37"\s*valign="top"\s*>.*<\/td>'
            # page_num_pattern=r'<td\s*width="35&#37".*?><p.*?<\/td>'
            page_num_pattern = r'共\d+页'

            titles = re.findall(title_pattern, resp.text)
            dates = re.findall(date_pattern, resp.text)
            if total_page_num == 0:
                total_page_num_str = re.findall(page_num_pattern, resp.text, re.S)
                if len(total_page_num_str) == 0:
                    break
                total_page_num = int(re.findall(r'\d+', total_page_num_str[0])[0])
            if len(titles) != len(dates) or len(titles) == 0:
                break
            for i in range(0, len(titles)):
                title_str = titles[i]
                date_str = dates[i]
                soup = BeautifulSoup(title_str, "html.parser")
                title = soup.find('a').text
                soup = BeautifulSoup(date_str, "html.parser")
                date = soup.text
                date.replace(' ', '')
                date_year = int(date[0:4])
                if date_year >= stock_id_years[1] and date_year <= stock_id_years[2]:
                    result.append([stock_id_years[0], date, title, library_value[1]])
                    flag = False
            page_num += 1
            if page_num + 1 >= total_page_num or flag:
                break
    return result


# 创建新的数据输出文件
def create_file(file_name: str):
    if os.path.exists(file_name):
        print(file_name, 'already exists')
        return
    wb = openpyxl.Workbook(file_name)
    ws = wb.active
    ws.append(['证券代码', '新闻发布时间', '新闻标题', '新闻来源'])
    wb.save(file_name)


# 程序异常时存储信息，存到log.txt
def save_log(start, end, file_index):
    log_file = "log.txt"
    with open(log_file, 'w', encoding='utf-8') as file:
        file.write(str(start) + '\n' + str(end) + '\n' + str(file_index) + '\n')


# 读取log.txt信息，从上次运行处接着运行
# log.info:start end sheet_index
def get_parameters(file_name_prefix):
    start_info = []
    log_file = r".\log.txt"
    with open(log_file, encoding='utf-8') as file:
        file_lines = file.readlines()
    for file_line in file_lines:
        start_info.append(int(file_line))
    file_name = file_name_prefix + str(start_info[2]) + r'.xlsx'
    if os.path.exists(file_name):
        wb = openpyxl.load_workbook(file_name)
        ws = wb.active
        start_info.append(ws.max_row)
    else:
        wb = openpyxl.Workbook()
        ws = wb.active
        ws.append(['证券代码', '新闻发布时间', '新闻标题', '新闻来源'])
        start_info.append(0)
        wb.save(file_name)
    return start_info[1], start_info[2], start_info[3]


if __name__ == "__main__":
    input_file = r"..\CG_ManagerShareSalary.xlsx"
    file_name_prefix = r'..\data'
    read_start, file_index, total_data_nums = get_parameters(file_name_prefix=file_name_prefix)
    # 每个excel表格最多存储100000多条数据
    file_name = file_name_prefix + str(file_index) + '.xlsx'
    sheet_max_datas = 100000

    # 读取数据
    print("读取数据中.....")
    stock_ids = read_excel(file=input_file, start=read_start)
    print(len(stock_ids))
    print(stock_ids)

    # 多线程爬取数据
    print("爬取数据中.....")
    start = 0
    num = 5
    results = []
    pool = Pool(num)
    while True:
        print('*' * 100)
        print('start:', start + 1, 'end:', start + num)
        try:
            if start + num > len(stock_ids):
                results = pool.map(craw_stockId_allpages, stock_ids[start:len(stock_ids)])
            else:
                results = pool.map(craw_stockId_allpages, stock_ids[start:start + num])
        except Exception:
            # 处理异常
            save_log(start=stock_ids[0][3], end=stock_ids[start][3], file_index=file_index)
            print("遇到异常，程序运行状态已保存至log.txt中")
        # 存储数据
        else:
            print("saving data.....")
            total_data_nums += save_data(results, file_name)
            if total_data_nums >= sheet_max_datas:
                total_data_nums = 0
                file_index += 1
                file_name = file_name_prefix + str(file_index) + '.xlsx'
                create_file(file_name)
            if start + num >= len(stock_ids):
                break
            start += num
            if start % 60 == 0:
                print("sleeping 10s.....")
                time.sleep(10)
    print("爬取信息完毕！！！")
```


# python-crawling-for-housing
#不動産屋のサイトをクローリングしてCSV形式でDLする

from urllib.request import urlopen
from urllib.error import HTTPError
from bs4 import BeautifulSoup as soup
import requests
import csv

base_url = 'https://www.koenji-f.jp/bukken/r/search32'

def get_information():
    
    file = open('不動産.csv', 'w', newline='', encoding='utf-8')
    writer = csv.writer(file)
    writer.writerow(['名前', '賃料', '敷金・礼金', '共益費', '所在地', '交通', '間取り', '面積', '築年月'])

    for i in range(2, 8):
        my_url = base_url + f'-{i}.html'
        client = urlopen(my_url)
        page = client.read()
        client.close()
        page_soup = soup(page, 'html.parser')
        
        ekibetsu = page_soup.find_all('div', {'class' : 'ekibetsu_bukken'})
    
 
        for bukken in ekibetsu:
            name = bukken.find('h4').text
            for i, item in enumerate(bukken.find_all('td'), 0):
                price = bukken.find_all('td')[0].text
                shikikin = bukken.find_all('td')[1].text
                kyoeki = bukken.find_all('td')[2].text
                address = bukken.find_all('td')[3].text
                access = bukken.find_all('td')[4].text
                room = bukken.find_all('td')[5].text
                square = bukken.find_all('td')[6].text
                built_in = bukken.find_all('td')[7].text
            writer.writerow([name, price, shikikin, kyoeki, address, access, room, square, built_in])
    file.close()
        
get_information()

# Music-Recommendation-System
감성분석을 통한 Content-based Filtering 기법으로 기호에 맞는 음악 추천.
import pandas as pd
import numpy as np
from tqdm.notebook import tqdm
import time
from bs4 import BeautifulSoup
import urllib.request
import urllib.parse
import requests
lyric=[]
artist=[]
date=[]
comment=[]
commentlist=[]
genre=[]
hdr = { 'User-Agent' : 'Mozilla/5.0'} #헤더설정

for i in tqdm(df['code'][:2000]):
    url='https://www.melon.com/song/detail.htm?songId={}'.format(i)
    req = urllib.request.Request(url, headers=hdr)
    html=urllib.request.urlopen(req).read()
    soup = BeautifulSoup(html,'html.parser')
    try:
        artist.append(soup.find('div',class_='artist').text[1:-1])
    except AttributeError as err:
        artist.append(None)
    try:
        date.append(soup.select('#downloadfrm > div > div > div.entry > div.meta > dl.list > dd')[1].text)
    except AttributeError as err:
        date.append(None)
    try:
        genre.append(soup.select('#downloadfrm > div > div > div.entry > div.meta > dl.list > dd')[2].text)
    except AttributeError as err:
        genre.append(None)
    try:
        lyric.append(soup.find('div',class_='lyric').text[9:-1])
    except AttributeError as err: 
        lyric.append(None)
        
# 댓글에 대한 정보, 동적인 페이지라 driver사용 (추천수 댓글 10개만)
    url2 = 'https://www.melon.com/song/detail.htm?songId={}#cmtpgn=&pageNo=1&sortType=1&srchType=2&srchWord'.format(i)
    driver = webdriver.Chrome('./chromedriver.exe',options=options) #  옵션 적용
    driver.get(url2)
    html = driver.page_source
    soup = BeautifulSoup(html,'html.parser')
    for k in soup.find_all('div',class_='cntt')[4:9]:
        try:
            comment.append(k.text[16:-2])
        except AttributeError as err:
            comment.append(None)
    commentlist.append(comment)
    
    

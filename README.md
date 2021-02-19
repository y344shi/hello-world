# This is a sample Python script.

# Press ⌃R to execute it or replace it with your code.
# Press Double ⇧ to search everywhere for classes, files, tool windows, actions, and settings.
import os

import requests
import bs4
from bs4 import BeautifulSoup
import re


def modify_search_url(base_url, key_word):
# turns a base url into keyword search state
        base_url += key_word
        return base_url


def access_url(name):
    kv = {'user-agent': 'Mozilla/5.0'}
    try:
        r = requests.get(name, headers=kv)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.request.headers)
        return r.text
    except:
        return "CONNECTION ERROR"

def find_and_save_photos(demo, path):
    soup = BeautifulSoup(demo, "html.parser")
    jpgs = soup.find_all(attrs=re.compile("name"))
    for result in jpgs:
        save_photo(name, path)


def save_photo(name, path):
    try:
        path_withname = path + '/' + name.split('/')[-1] + '.jpg'
        print(path)
        if not os.path.exists(path):
            os.mkdir(path)
        if not os.path.exists(path):
            r = requests.get(name)
            with open(path, 'wb') as f:
                f.write(r.content)
                f.close()
                print("save successful")
        else:
            print("file already exists")
    except:
        return "CONNECTION ERROR"




# Press the green button in the gutter to run the script.
if __name__ == '__main__':
    name = "https://huaban.com/search/?q="
    keyword = "海报设计"
    path = "/Volumes/SSD_Storage/PYTHON_Photo_Storage"
    html_text = access_url(modify_search_url(name, keyword))
    find_and_save_photos(html_text, path)

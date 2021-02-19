# coding=utf-8
# This is a sample Python script.im
# Press ⌃R to execute it or replace it with your code.
# Press Double ⇧ to search everywhere for classes, files, tool windows, actions, and settings.
import os

import requests
import bs4
from bs4 import BeautifulSoup
import re


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


def access_url_honest(name):
    try:
        r = requests.get(name)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.request.headers)
        return r.text
    except:
        return "CONNECTION ERROR"


def access_url_keyword(name, key_w):
    try:
        kv = {'wd': key_w}
        r = requests.get(name, params=kv)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.request.url)
        return r.text[:5000]
    except:
        return "CONNECTION ERROR"


def access_url_keyword(name, key_w):
    try:
        kv = {'wd': key_w}
        r = requests.get(name, params=kv)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        print(r.request.url)
        return r.text[:5000]
    except:
        return "CONNECTION ERROR"


def convert_names(name, key_word):
    name += key_word
    return name


def access_url_save(name, path):
    try:
        path_withname = path + '/' + name.split('/')[-1]
        print(path_withname)
        if not os.path.exists(path):
            os.mkdir(path)
        if not os.path.exists(path_withname):
            r = requests.get(name)
            with open(path_withname, 'wb') as f:
                f.write(r.content)
                f.close()
                print("save successful")
        else:
            print("file already exists")
    except:
        return "CONNECTION ERROR"


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


def ip_address_seek(ip_in):  # not usable
    url = "https://ipchaxun.com/"
    kv = {'user-agent': 'Mozilla/5.0'}
    print(url + ip_in)
    try:
        r = requests.get(url + ip_in + "/", headers=kv)
        r.raise_for_status()
        r.encoding = r.apparent_encoding
        return r.text[:1500]
    except:
        return "CONNECTION ERROR"


def parent_seek(demo):
    soup = BeautifulSoup(demo, "html.parser")
    print("all parents")
    for i in soup.a.parents:
        if i is None:
            print(i)
        else:
            print(i.name)
    # print("all next siblings:")
    # for sibling in soup.html.next_siblings:
    #    print(sibling)
    # print("all previous siblings")
    # for sibling in soup.html.previous_siblings:
    #    print(sibling)


def children_seek(demo):
    soup = BeautifulSoup(demo, "html.parser")
    print("all children:")
    for i in soup.html.descendants:
        if i is None:
            print(i)
        else:
            print(i.name)
    print(soup.script.next_sibling, type(soup.script.next_sibling))


def sibling_seek(demo):
    soup = BeautifulSoup(demo, "html.parser")
    print("all sibling:")
    for i in soup.script.previous_siblings:
        if i is None:
            print(i)
        else:
            print(i.name)
    for i in soup.script.next_siblings:
        if i is None:
            print(i)
        else:
            print(i.name)


def find_photos(demo):
    soup = BeautifulSoup(demo, "html.parser")
    jpgs = soup.find_all(attrs=re.compile("name"))
    for result in jpgs:
        print(result)


def Uni_Ranking():
    def get_TEXT(url):
        try:
            r = requests.get(url, timeout=30)
            r.raise_for_status()
            r.encoding = r.apparent_encoding
            return r.text
        except:
            return ""

    def fill_uni_list(ulist, html):
        soup = BeautifulSoup(html, "html.parser")
        for tr in soup.find('tbody').children:
            if isinstance(tr, bs4.element.Tag):
                tds = tr('td')
                name = tds[1].a.string
                # print(name)
                name = name.encode('utf-8')
                # print(name, isinstance(name, unicode))
                ulist.append([int(tds[0].contents[0].string), name, isinstance(name, str),
                              float(tds[-2].contents[0].string)])
                print(tds[-2].contents[0].string)

    def print_uni_list(ulist, num):
        #print("{:^10}\t{:^6}\t{:^10}".format("排名", "学校名称", "总分"))
        #中文输出对齐问题：
        tplt = '{0:^10}\t{1:{3}^10}\t{2:^10}'
        print(tplt.format("排名", "学校名称 ", "总分", chr(12288)))
        #"{ }: sometext/spaces\t to always display { } xxxx { }%".format("2016-1-3","python","10") <- real data corresopondingly
        for i in range(num):
            u = ulist[i]
            #print("{:^10}\t{:^6}\t{:^10}".format(u[0], u[1].decode(), u[2]))
            print(tplt.format(u[0], u[1].decode(), u[3], chr(12288)))
            #print(ulist[i][3])

    def sub_main():
        uinfo = []
        url = "https://www.shanghairanking.cn/rankings/bcur/202011"
        html = get_TEXT(url)
        # eprint(html)
        fill_uni_list(uinfo, html)
        print_uni_list(uinfo, 20)

    sub_main()


def sub_main_regular_experssion():
    print( re.search(r'\d{3}-\d{8}|\d{4}-\d{7}','138-07448800 sjalkjhud asdddd 433222'))


def sub_main_old_main():
    name_in1 = "https://item.jd.com/100017133240.html"
    name_in2 = "https://pic1.zhimg.com/80/v2-2cc2923ee2ce3a002e9375931b6bb382_1440w.jpg?source=1940ef5c.jpg"
    name_in3 = "https://n.sinaimg.cn/tech/transform/284/w167h117/20210118/8afd-khstaxt1736142.gif"
    name_saved = "http://www.baidu.com/s"
    keyword = "python"
    paths = "/Volumes/SSD_Storage/PYTHON_Photo_Storage"
    kv = {'user-agent': 'Mozilla/5.0'}
    # print(name_saved)
    # print(access_url_save(name_in2, paths))
    # print(ip_address_seek("202.204.80.112"))
    r = requests.get("https://www.shanghairanking.cn/rankings/bcur/202011")
    r.encoding = r.apparent_encoding
    demo_in = r.text
    soup_in = BeautifulSoup(demo_in, "html.parser")
    # print(soup_in)
    for tr in soup_in.find('tbody').children:
        if isinstance(tr, bs4.element.Tag):
            tds = tr('td')
            name1 = float(tds[-2].contents[0].string)
            # print(name1)
        # print(tds[1].a.string)
    # for link in soup.find_all('a'):
    # print(link.get('href'))
    # find_photos(demo_in)
    # print (soup)
    Uni_Ranking()


if __name__ == '__main__':
    sub_main_regular_experssion()
# See PyCharm help at https://www.jetbrains.com/help/pycharm/

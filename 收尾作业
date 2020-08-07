# -*- coding:utf-8 -*-
import time
import socket
from urllib.parse import urlparse
from PIL import Image


from http.server import HTTPServer, BaseHTTPRequestHandler


class Resquest(BaseHTTPRequestHandler):  # http服务类  处理get请求
    def do_GET(self):
        querypath = urlparse(self.path)
        filepath, query = querypath.path, querypath.query
        print(filepath)
        if filepath == "/favicon.ico":
            self.wfile.write("".encode())
            return
        elif filepath.find("png") != -1:  # 返回图片
            filename = filepath[1:]
            fp = open(filename, 'rb')
            png = fp.read()
            fp.close()
            self.wfile.write(png)
        else:
            day = filepath[1:]  # 处理请求html
            DealDay(day)  # 生成图片
            fp = open("index.html")
            content = fp.read()
            fp.close()
            content = content % day
            self.wfile.write(content.encode())


def YearDay(year):  # 计算某一年多少天
    if (year % 4) == 0 and (year % 100) != 0 or (year % 400) == 0:
        return 366
    else:
        return 365


def GetAround(day):  # 计算图片旋转的角度
    day_time = time.strptime(day, "%Y-%m-%d")
    # now_time = time.localtime(now_stamp)

    curday = day_time.tm_yday
    totalday = YearDay(day_time.tm_year)
    # print(curday)
    # print(totalday)
    return (curday - 1)/totalday * 360


def AroundPic(around, day):  # 图片旋转保存为新的图片
    img = Image.open("org.png")
    img2 = img.rotate(around)   # 自定义旋转度数
    img2.save("%s.png" % day)


def DealDay(day):
    around = GetAround(day)
    print(around)
    AroundPic(around, day)

    # 旋转方式二
    #


def Main():
    # 开启httpserver监听80端口
    server_address = ('', 80)
    server = HTTPServer(server_address, Resquest)
    server.serve_forever()


Main()

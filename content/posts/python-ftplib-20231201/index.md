---
title: "python ftp客户端使用 ftplib"
date: 2023-12-01T10:33:17+08:00
lastmod: 2023-12-01T10:33:17+08:00
draft: false
author: ev01ing 
keepStatic: false
auto: true
categories: ["python"]
tags: ["python", "ftp", "ftplib"]
---

使用python的`ftplib`进行ftp操作，是python3的标准库，不需要安装。

```py
import ftplib
import logging
import os


class FTPClient():
    sep = "/"

    def __init__(self, ip, port, username, password):
        self.ip = ip
        self.port = port
        self.username = username
        self.password = password
        self.ftp = ftplib.FTP()
        self.ftp.connect(self.ip, self.port)
        self.ftp.login(self.username, self.password)
        self.ftp.encoding = "UTF-8"

    def download(self, path, filename, local_path):
        """
        从FTP服务器下载文件到本地

        参数：
        path (str): 文件所在路径
        filename (str): 文件名
        local_path (str): 本地保存路径

        返回：  无
        """
        remote_filename = self.join(path, filename)
        self.ftp.retrbinary('RETR ' + remote_filename,
                            open(os.path.join(local_path, os.path.basename(remote_filename)), 'wb').write)

    def listdir_recursive(self, path):
        # 递归地列出指定路径下的所有文件和文件夹的名称
        filenames = self.listdir(path)

        files = []
        for filename in filenames:
            fullname = self.join(path, filename)
            if self.is_file(fullname):
                files.append(fullname)
            elif self.is_directory(fullname):
                t_files = self.listdir_recursive(fullname)
                files.extend(t_files)
            else:
                logging.error("unkown ftp path: %s", fullname)
        return files

    def listdir(self, remote_path):
        # 列出路径下的文件和文件夹，非递归
        self.ftp.cwd(remote_path)
        return self.ftp.nlst()

    def is_directory(self, path):
        """
        判断给定路径是否为目录

        参数：
        path：要判断的路径

        返回值：True or False
        """
        try:
            results = self.ftp.nlst(path)
            if len(results) > 1:
                return True
            elif len(results) == 1:
                return path not in results
        except:
            pass
        return False

    def is_file(self, path):
        """
        判断给定路径是否是一个文件

        :param path: 文件路径
        :return: 若给定路径是一个文件则返回True，否则返回False
        """
        try:
            self.ftp.size(path)
            return True
        except:
            pass
        return False

    @classmethod
    def join(cls, *paths):
        """
        将多个路径拼接成一个路径。

        :param paths: 多个路径，可以为空
        :type paths: str
        :return: 拼接后的路径
        :rtype: str
        """
        if len(paths) == 0:
            return ""

        new_path = paths[0]
        for i in range(1, len(paths)):
            if not new_path.endswith(cls.sep):
                new_path += cls.sep
            new_path += paths[i]

        return new_path

    def close(self):
        """
        关闭FTP连接

        :return: None
        """
        self.ftp.quit()
```
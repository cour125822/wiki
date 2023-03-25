---
title: Watch Dog
description: 
published: true
date: 2023-03-06T05:31:32.446Z
tags: 
editor: markdown
dateCreated: 2023-02-26T08:24:20.687Z
---

## watchDog

### 安装

```python
pip install watchdog
```

### 案例

```python
"""
    watchdog检测当前目录文件修改
"""
import sys
import time
import logging
from watchdog.observers import Observer
from watchdog.events import  FileSystemEventHandler

if __name__ == "__main__":
    # 设置打印级别
    logging.basicConfig(level=logging.INFO,
                        format='%(asctime)s - %(message)s',
                        datefmt='%Y-%m-%d %H:%M:%S')
    # 设置检测目录
    path = sys.argv[1] if len(sys.argv) > 1 else '.'
  
    class SelfHandler(FileSystemEventHandler):
        def on_created(self, event):
            print("{}文件创建".format(event.src_path))
        
        def on_deleted(self, event):
            print("{}文件删除".format(event.src_path))
        
        def on_modified(self, event):
            print("{}文件修改".format(event.src_path))
  
    event_handler = SelfHandler()
    observer = Observer()
    # 通过在对watchdog.observers.Observer.schedule()的调用中 传递recursive=True来确保监视整个目录树。
    observer.schedule(event_handler, path, recursive=True)
    observer.start()
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
```
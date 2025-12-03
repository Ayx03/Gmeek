阅读首页和 `/getwifi` 的前端源代码没找到什么有价值的信息，注意到下载按钮指向类似 `/download?file=be7fa00ac8a16c8266ce3ee4f4e318cc.jpg` 的路径，清空 `file=` 后面的内容发现报错：`IsADirectoryError: [Errno 21] Is a directory: '/app/static/img/'`，看到这个页面就一股 Python 味，猜测源代码文件名为 `app.py` ~~因为我之前用 Python 写 Web 项目的时候就真的所有代码都放在这个文件里~~。

<img width="2560" height="1600" alt="Image" src="https://github.com/user-attachments/assets/7098b2f1-b2f6-4643-971c-25001f570745" />

尝试使用 `/download?file=../../app.py`穿越回上级目录的上级目录下载到项目源代码，还真的下载到了。使用 Visual Studio Code 打开 Python 代码发现有一个路由 `/secret_path_U_never_know` 可以得到 flag，但是从代码和访问实测看确实有身份验证会验证会话 `isadmin` 的值，但是用来给会话签名的 SECRET_KEY 也写在代码里，所以我们只要随便新增一个路由或者修改一个现有路由把 `session['isadmin']` 设置成 `True` 然后运行这个代码访问这个路由就能获得一个能通过验证的会话了

<img width="2560" height="1528" alt="Image" src="https://github.com/user-attachments/assets/2277e68f-41dd-4655-84fc-9f99de5a67af" />

最后只要在这个页面打开开发人员工具 > 应用程序（Application）然后把 Cookie 中 `session` 的值复制到远程服务器页面的这个位置在刷新页面就能获得 flag 了。运行代码之前记得把有关 flag 的都注释掉或者删掉，因为在你本地很明显没有 flag 这个包。图片下面是修改好的代码，其实你直接复制这个 `session` 设置 Cookie 应该也能过：`eyJpc2FkbWluIjp0cnVlfQ.aTAYhA.raXy4WWOlwU1Ct1iwYMVn5bvGx0`


<img width="2560" height="1523" alt="Image" src="https://github.com/user-attachments/assets/9154171f-5563-419e-9523-b1919df6edfb" />

<img width="2560" height="1528" alt="Image" src="https://github.com/user-attachments/assets/c37de198-c921-453d-a848-61cb4e191bc9" />

```py
# !/usr/bin/env python
# -*-coding:utf-8 -*-

"""
# File       : app.py
# Time       ：2022/11/07 09:16
# Author     ：g4_simon
# version    ：python 3.9.7
# Description：抽老婆，哇偶~
"""

from flask import *
import os
import random
# from flag import flag

#初始化全局变量
app = Flask(__name__)
app.config['SECRET_KEY'] = 'tanji_is_A_boy_Yooooooooooooooooooooo!'

@app.route('/', methods=['GET'])
def index():  
    return render_template('index.html')


@app.route('/getwifi', methods=['GET'])
def getwifi():
    session['isadmin']=False
    wifi=random.choice(os.listdir('static/img'))
    session['current_wifi']=wifi
    return render_template('getwifi.html',wifi=wifi)



@app.route('/download', methods=['GET'])
def source(): 
    filename=request.args.get('file')
    if 'flag' in filename:
        return jsonify({"msg":"你想干什么？"})
    else:
        return send_file('static/img/'+filename,as_attachment=True)


@app.route('/secret_path_U_never_know',methods=['GET'])
def getflag():
    if session['isadmin']:
        return jsonify({"msg":"你怎么知道这个路径的？不过还好我有身份验证"})
        # return jsonify({"msg":flag})
    else:
        return jsonify({"msg":"你怎么知道这个路径的？不过还好我有身份验证"})

# set a app route that will print out a session with isadmin = True
@app.route('/setadmin',methods=['GET'])
def setadmin():
    session['isadmin']=True
    return jsonify({"msg":"你已经成为管理员了，快去查看flag吧！"})


if __name__ == '__main__':
    app.run(host='0.0.0.0',port=80,debug=True)
    # generate a session with isadmin = True and output in the console

```
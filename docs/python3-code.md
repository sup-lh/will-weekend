<style> g{ font-size:26px; color: #D3D3D3; background: #66CDAA; border-radius: 8px; opacity: 85%; } </style>

#
# <g>GBK编码 和 UTF-8编码 的区别： 😶😶</g>


- 编码方式不同，表示的文字范围不同。（UTF-8能表示更多的语言文字，更加通用）

***
***
***
#  知晓 系统默认编码 和 本地默认编码： 🥵🥵

- ```python
    #先导包：
    import sys, locale
    #示例:
    print(sys.getdefaultencoding())
    print(locale.getdefaultlocale())
  ```
- 输出结果：
  ```python
    utf-8
    ('zh_CN', 'cp936')
  ```

- 小结下下：<br>
> [!WARNING|label:]
第一个的 utf-8 指：**系统默认编码**<br>
注： 不要把系统以为是操作系统，这里可以理解成python3的编译器本身<br>
第二个的 cp936 指：**本地默认编码**<br>
注： 这个才是操作系统的编码。（在Windows运行会变成gbk）<br><hr>
注： cp936其实就是GBK，IBM在发明Code Page的时候将GBK放在第936页，所以叫CP936。<hr>

## **系统默认编码** 指： 🧑‍⚕️
- 在python3编译器读取.py文件时，若没有头文件编码声明，则默认使用“utf-8”来对.py文件进行解码。并且在调用 encode()这个函数时，不传参的话默认是“ utf-8 ”。（这与下面的open( )函数中的“encoding”参数要做区分，非常误导人！！！）

## **本地默认编码** 指： 🧑‍🎓
- 在你编写的python3程序时，若使用了 open( )函数 ，而不给它传入 “ encoding ” 这个参数，那么会自动使用本地默认编码。没错，如果在Windows系统中，就是默认用gbk格式！！！<hr>
- 注意：**linux中可以不用传“ encoding” 的参数， 而win中不能忘了**<hr><br>
***
***
***
# Python3中的​encode和decode ✊✊
## 两者的区别 

> [!Note|label:]encode的作用，使我们看到的直观的字符转换成计算机内的字节形式。decode刚好相反，把字节形式的字符转换成我们看的懂的、直观的、“人模人样”的形式。<br>

?> 直接上代码！
- ```python
    print('*'*80)
    d='全国招聘网'.encode('unicode-escape')
    print('d:',d)
    g='全国招聘网'.encode()
    print('g:',g)
    e=b"\u5168\u56fd\u62db\u8058\u7f51".decode('unicode-escape')
    print('e:',e)
    a=b"\u5168\u56fd\u62db\u8058\u7f51".decode()
    print('a:',a)


    print('*'*80)
    print('*'*80)


    b='美国'.encode()
    print('b:',b)
    c=b'\xe7\xbe\x8e\xe5\x9b\xbd'.decode('utf-8')
    print('c:',c)
    f=b'\xe7\xbe\x8e\xe5\x9b\xbd'.decode()
    print('f:',f)
    print('*'*80)
    ```
- 输出结果为：
- ```python
    ********************************************************************************
    d: b'\\u5168\\u56fd\\u62db\\u8058\\u7f51'
    g: b'\xe5\x85\xa8\xe5\x9b\xbd\xe6\x8b\x9b\xe8\x81\x98\xe7\xbd\x91'
    e: 全国招聘网
    a: \u5168\u56fd\u62db\u8058\u7f51
    ********************************************************************************
    ********************************************************************************
    b: b'\xe7\xbe\x8e\xe5\x9b\xbd'
    c: 美国
    f: 美国
    ********************************************************************************
    ```
**能看到这说明你很有勇气啊，勇气和智慧并存嘛，So，You know**<br>
## 解释下解释下： 🎃🎃
> [!Tip|label:敲黑板了敲黑板了]
- encode为编码，decode为解码。<br><br>
- 类似 '\xe7\xbe\x8e\xe5\x9b\xbd' 这种就是**编码**后的看不懂的外星文<br>
- 当能显示中文的时候就是**解码**后的了<br><br>
- 上面代码中encode或者decode后面的括号里面的就是遵从的编码规则<br><br>
- 可见：<br>
- 编码(encode)时如果括号内不写编码规则的时候就按照python3的编译器本身的编码规则（这里是utf-8）编码<br>
- 上面 d 则是规定了编码规则（unicode-escape）<br>
- 解码(decode)时默认是按照python3的编译器本身的编码规则解码<br><hr>
- **注意：像上面那些解码的字符串的前面需要加 b 前缀，不然就会报错。**<hr><br>
***
***
***
# json文件中文显示unicode-escape编码 👝👝
> [!Danger|label:我TM直接上代码]
- 终于在上面废话那么多后说点有用的东西了！！！
- ```python
    new_dada=json.dumps(data,ensure_ascii=False)#ensure_ascii=False 就不会用 ASCII 编码，中文就可以正常显示了
    ```
    直接上代码！不需要废话！
    - 谷歌人回来了谷歌人回来了
        - 上代码！！
            ```python
            import json
            print(json.dumps('中国'))
            ```
            输出结果为：
            ```python
            "\u4e2d\u56fd"
            ```
    - 读书人借鉴回来了！！
        - 这是因为中文以 unicode 编码了，而默认是以ASCII解析的，中文不在ASCII编码中，所以无法显示。

😮‍💨😮‍💨😮‍💨😮‍💨😮‍💨😮‍💨😮‍💨😮‍💨😮‍💨
# json库的使用


# json库的使用



###  JSON与字典的转换

`dumps( )`方法 ==> 将**字典**转化为 **json字符串** 
`loads( )`方法 ==> 将**json字符串**转为**字典**

>   json与python中的字典颇为相似。
>
>   但是底层的序列化格式不同。
>
>   json打印出来的类型是字符串`<class 'str'>`类型
>
>   而字典类型则是`<class 'dict'>`。
>
>   且通过jumps方法把字典转换为字符串之后，进行了unicode转码。
>
>   使用`loads`方法时，属性名必须使用双引号。
>
>   使用`dumps`方法时,属性名是否是双引号不影响

```python
import json

# 这是一个字符串 loads() ==> 将字符串转化为字典
data = '{"data":"张三","age":18}'
d1 = json.loads(data)
print(d1)  # {'data': '张三', 'age': 18}

# 这也是一个字符串  loads() ==> 将字符串转化为字典
# 想要转换为字典，那么属性名必须是双引号
data2 = "{'data': '张三', 'age': 18}"
d2 = json.loads(data2)
print(d2)  # 报错：Expecting property name enclosed in double quotes: line 1 column 2 (char 1)原因是没有用双引号

# dumps() ==> 将字典类型转换为json,属性名是否是双引号不影响
data3 = {"data": "张三", "age": 18}  # type(data3)结果是<class 'dict'>，字典类型
d2 = json.dumps(data3)
print(type(d2))  # <class 'str'> 结果是json格式  打印结果为：{"data": "\u5f20\u4e09", "age": 18} 
```

### JSON文件读写

`dump( )`方法 ==> 写入json
`load( )`方法 ==> 读取json

```python
import json

data = {'name':'张三','age':18}
with open("write.json","w",encoding="utf-8")as f:
    json.dump(data,f,ensure_ascii=False) # {"name": "张三", "age": 18}
    

with open ("write.json","r",encoding="utf-8")as f:
    data = json.load(f)
    print(data)  # {'name': '张三', 'age': 18} 
```







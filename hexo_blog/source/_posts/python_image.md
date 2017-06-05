layout: three
title: Python使用PIL破解验证码
date: 2016-11-18 00:12:05
tags: 重点
comments: true
categories: Python

---
## 破解验证码DEMO
本文原项目地址为[破解验证码](https://github.com/kouyalong/captcha_crack)，刷新请求不同的验证码，后台破解验证码同时把结果返回。

* 主要破解代码段
```
def break_image(image):
    """
    先把图片根据字符和空白RGB值，把图片转换成model=1的灰度模式
    然后根据灰度模式，黑的为0， 白的为1，把图片转换成0，1字符串
    """
    image = image.convert("RGB")
    image2_dict = {}
    word_count = 4     # 观察验证码字符个数
    start = 4       # 观察图片验证码开始的位置
    one_word_length = 12        # 观察图片验证码每个字符是多少长度
    i = 0
    while i < word_count:
        image2 = Image.new("1", (one_word_length, image.size[1]), 255)
        for x in range(start, start + one_word_length):
            for y in range(image.size[1]):
                pix = image.getpixel((x, y))
                if pix[1] < 255:    # 根据图片验证码上面字符的颜色筛选
                    image2.putpixel((x - start, y), 0)
        image2_dict[i] = image2
        start += one_word_length
        i += 1

    tem_dict = {}
    for key in image2_dict.keys():
        tem_list = []
        for x in range(image2_dict.get(key).size[0]):
            tem_str = ''
            for y in range(image2_dict.get(key).size[1]):
                pix = image2_dict.get(key).getpixel((x, y))
                if pix != 0:
                    pix = 1
                else:
                    pix = 0
                tem_str += str(pix)
            tem_list.append(tem_str + "\n")
        tem_dict[key] = tem_list
    return tem_dict, image.size[1]
```
---

title: flask_ihome项目
date: 2023/7/31 11:46:25
comments: true
tags: 
- flask
- item
categories: 
- 计算机
- flask
- itme
---

# nginx配置

```python
server {
        listen       8080;
        listen [::]:8080;
        server_name  dwz.feelapple.cn;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;


        location / {
            proxy_pass http://127.0.0.1:5000;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
        root   html;
        }
}
```

# 数据库

## 数据库表设计

![数据迁移](../images/ihome/数据库表设计.png)

## 数据库实现

```python
from werkzeug.security import generate_password_hash, check_password_hash
from ihome.utils import constants
from ihome.utils.models import BaseModel
from . import db


class User(BaseModel,db.Model):
    """用户"""

    __tablename__ = "ih_user_profile"

    name = db.Column(db.String(32), unique=True, nullable=False)  # 用户暱称
    password_hash = db.Column(db.String(128), nullable=False)  # 加密的密码
    mobile = db.Column(db.String(11), unique=True, nullable=False)  # 手机号
    real_name = db.Column(db.String(32))  # 真实姓名
    id_card = db.Column(db.String(20))  # 身份证号
    avatar_url = db.Column(db.String(128))  # 用户头像路径
    houses = db.relationship("House", backref="user")  # 用户发布的房屋
    orders = db.relationship("Order", backref="user")  # 用户下的订单

    # 加上property装饰器后，会把函数变为属性，属性名即为函数名
    @property
    def password(self):
        """读取属性的函数行为"""
        # print(user.password)  # 读取属性时被调用
        # 函数的返回值会作为属性值
        # return "xxxx"
        raise AttributeError("这个属性只能设置，不能读取")

    # 使用这个装饰器, 对应设置属性操作
    @password.setter
    def password(self, value):
        """
        设置属性  user.passord = "xxxxx"
        :param value: 设置属性时的数据 value就是"xxxxx", 原始的明文密码
        :return:
        """
        self.password_hash = generate_password_hash(value)

    # def generate_password_hash(self, origin_password):
    #     """对密码进行加密"""
    #     self.password_hash = generate_password_hash(origin_password)

    def check_password(self, passwd):
        """
        检验密码的正确性
        :param passwd:  用户登录时填写的原始密码
        :return: 如果正确，返回True， 否则返回False
        """
        return check_password_hash(self.password_hash, passwd)

    def to_dict(self):
        """将对象转换为字典数据"""
        user_dict = {
            "user_id": self.id,
            "name": self.name,
            "mobile": self.mobile,
            "avatar": constants.QINIU_URL_DOMAIN + self.avatar_url if self.avatar_url else "",
            "create_time": self.create_time.strftime("%Y-%m-%d %H:%M:%S")
        }
        return user_dict

    def auth_to_dict(self):
        """将实名信息转换为字典数据"""
        auth_dict = {
            "user_id": self.id,
            "real_name": self.real_name,
            "id_card": self.id_card
        }
        return auth_dict


class Area(BaseModel, db.Model):
    """城区"""

    __tablename__ = "ih_area_info"

    # 区域编号
    name = db.Column(db.String(32), nullable=False)  # 区域名字
    houses = db.relationship("House", backref="area")  # 区域的房屋

    def to_dict(self):
        """将对象转换为字典"""
        d = {
            "aid": self.id,
            "aname": self.name
        }
        return d


# 房屋设施表，建立房屋与设施的多对多关系
house_facility = db.Table(
    "ih_house_facility",
    db.Column("house_id", db.Integer, db.ForeignKey("ih_house_info.id"), primary_key=True),  # 房屋编号
    db.Column("facility_id", db.Integer, db.ForeignKey("ih_facility_info.id"), primary_key=True)  # 设施编号
)


class House(BaseModel, db.Model):
    """房屋信息"""

    __tablename__ = "ih_house_info"

    # 房屋编号
    user_id = db.Column(db.Integer, db.ForeignKey("ih_user_profile.id"), nullable=False)  # 房屋主人的用户编号
    area_id = db.Column(db.Integer, db.ForeignKey("ih_area_info.id"), nullable=False)  # 归属地的区域编号
    title = db.Column(db.String(64), nullable=False)  # 标题
    price = db.Column(db.Integer, default=0)  # 单价，单位：分
    address = db.Column(db.String(512), default="")  # 地址
    room_count = db.Column(db.Integer, default=1)  # 房间数目
    acreage = db.Column(db.Integer, default=0)  # 房屋面积
    unit = db.Column(db.String(32), default="")  # 房屋单元， 如几室几厅
    capacity = db.Column(db.Integer, default=1)  # 房屋容纳的人数
    beds = db.Column(db.String(64), default="")  # 房屋床铺的配置
    deposit = db.Column(db.Integer, default=0)  # 房屋押金
    min_days = db.Column(db.Integer, default=1)  # 最少入住天数
    max_days = db.Column(db.Integer, default=0)  # 最多入住天数，0表示不限制
    order_count = db.Column(db.Integer, default=0)  # 预订完成的该房屋的订单数
    index_image_url = db.Column(db.String(256), default="")  # 房屋主图片的路径
    facilities = db.relationship("Facility", secondary=house_facility)  # 房屋的设施
    images = db.relationship("HouseImage")  # 房屋的图片
    orders = db.relationship("Order", backref="house")  # 房屋的订单

    def to_basic_dict(self):
        """将基本信息转换为字典数据"""
        house_dict = {
            "house_id": self.id,
            "title": self.title,
            "price": self.price,
            "area_name": self.area.name,
            "img_url": constants.QINIU_URL_DOMAIN + self.index_image_url if self.index_image_url else "",
            "room_count": self.room_count,
            "order_count": self.order_count,
            "address": self.address,
            "user_avatar": constants.QINIU_URL_DOMAIN + self.user.avatar_url if self.user.avatar_url else "",
            "ctime": self.create_time.strftime("%Y-%m-%d")
        }
        return house_dict

    def to_full_dict(self):
        """将详细信息转换为字典数据"""
        house_dict = {
            "hid": self.id,
            "user_id": self.user_id,
            "user_name": self.user.name,
            "user_avatar": constants.QINIU_URL_DOMAIN + self.user.avatar_url if self.user.avatar_url else "",
            "title": self.title,
            "price": self.price,
            "address": self.address,
            "room_count": self.room_count,
            "acreage": self.acreage,
            "unit": self.unit,
            "capacity": self.capacity,
            "beds": self.beds,
            "deposit": self.deposit,
            "min_days": self.min_days,
            "max_days": self.max_days,
        }

        # 房屋图片
        img_urls = []
        for image in self.images:
            img_urls.append(constants.QINIU_URL_DOMAIN + image.url)
        house_dict["img_urls"] = img_urls

        # 房屋设施
        facilities = []
        for facility in self.facilities:
            facilities.append(facility.id)
        house_dict["facilities"] = facilities

        # 评论信息
        comments = []
        orders = Order.query.filter(Order.house_id == self.id, Order.status == "COMPLETE", Order.comment != None)\
            .order_by(Order.update_time.desc()).limit(constants.HOUSE_DETAIL_COMMENT_DISPLAY_COUNTS)
        for order in orders:
            comment = {
                "comment": order.comment,  # 评论的内容
                "user_name": order.user.name if order.user.name != order.user.mobile else "匿名用户",  # 发表评论的用户
                "ctime": order.update_time.strftime("%Y-%m-%d %H:%M:%S")  # 评价的时间
            }
            comments.append(comment)
        house_dict["comments"] = comments
        return house_dict


class Facility(BaseModel, db.Model):
    """设施信息"""

    __tablename__ = "ih_facility_info"

    # 设施编号
    name = db.Column(db.String(32), nullable=False)  # 设施名字


class HouseImage(BaseModel, db.Model):
    """房屋图片"""

    __tablename__ = "ih_house_image"

    house_id = db.Column(db.Integer, db.ForeignKey("ih_house_info.id"), nullable=False)  # 房屋编号
    url = db.Column(db.String(256), nullable=False)  # 图片的路径


class Order(BaseModel, db.Model):
    """订单"""

    __tablename__ = "ih_order_info"

    # 订单编号
    user_id = db.Column(db.Integer, db.ForeignKey("ih_user_profile.id"), nullable=False)  # 下订单的用户编号
    house_id = db.Column(db.Integer, db.ForeignKey("ih_house_info.id"), nullable=False)  # 预订的房间编号
    begin_date = db.Column(db.DateTime, nullable=False)  # 预订的起始时间
    end_date = db.Column(db.DateTime, nullable=False)  # 预订的结束时间
    days = db.Column(db.Integer, nullable=False)  # 预订的总天数
    house_price = db.Column(db.Integer, nullable=False)  # 房屋的单价
    amount = db.Column(db.Integer, nullable=False)  # 订单的总金额
    status = db.Column(  # 订单的状态
        db.Enum(   # 枚举     # django choice
            "WAIT_ACCEPT",  # 待接单,
            "WAIT_PAYMENT",  # 待支付
            "PAID",  # 已支付
            "WAIT_COMMENT",  # 待评价
            "COMPLETE",  # 已完成
            "CANCELED",  # 已取消
            "REJECTED"  # 已拒单
        ),
        default="WAIT_ACCEPT", index=True)    # 指明在mysql中这个字段建立索引，加快查询速度
    comment = db.Column(db.Text)  # 订单的评论信息或者拒单原因
    trade_no = db.Column(db.String(80))  # 交易的流水号 支付宝的

    def to_dict(self):
        """将订单信息转换为字典数据"""
        order_dict = {
            "order_id": self.id,
            "title": self.house.title,
            "img_url": constants.QINIU_URL_DOMAIN + self.house.index_image_url if self.house.index_image_url else "",
            "start_date": self.begin_date.strftime("%Y-%m-%d"),
            "end_date": self.end_date.strftime("%Y-%m-%d"),
            "ctime": self.create_time.strftime("%Y-%m-%d %H:%M:%S"),
            "days": self.days,
            "amount": self.amount,
            "status": self.status,
            "comment": self.comment if self.comment else ""
        }
        return order_dict

class Person(BaseModel,db.Model):
    __tablename__ = 'ih_id_card'

    address_code = db.Column(db.String(10))
    abandoned = db.Column(db.Integer)
    address = db.Column(db.String(100))
    age = db.Column(db.Integer)
    birthday_code = db.Column(db.Date)
    constellation = db.Column(db.String(20))
    chinese_zodiac = db.Column(db.String(20))
    sex = db.Column(db.Integer)
    length = db.Column(db.Integer)
    check_bit = db.Column(db.String(1))
    # 定义与User的一对一关联
    user = db.relationship("User", back_populates="person", uselist=False)

    def __init__(self, address_code, abandoned, address, age, birthday_code, constellation, chinese_zodiac, sex, length,
                 check_bit):
        self.address_code = address_code
        self.abandoned = abandoned
        self.address = address
        self.age = age
        self.birthday_code = datetime.strptime(birthday_code, '%Y-%m-%d').date()
        self.constellation = constellation
        self.chinese_zodiac = chinese_zodiac
        self.sex = sex
        self.length = length
        self.check_bit = check_bit
```

**公共模型类**

```python
from ihome import db


class BaseModel(db.Model):
    """模型基类，为每个模型补充创建时间与更新时间"""
    __abstract__ = True
    id = db.Column(db.Integer, primary_key=True)
    create_time = db.Column(db.DateTime, default=datetime.now)  # 记录的创建时间
    update_time = db.Column(db.DateTime, default=datetime.now, onupdate=datetime.now)  # 记录的更新时间

    def save(self):
        db.session.add(self)
        db.session.commit()

    def delete(self):
        db.session.delete(self)
        db.session.commit()
```

# 静态文件接口

## 提供静态文件蓝图编写

web_html.py

```python
from flask import Blueprint,current_app

html = Blueprint('web_html',__name__)

# 127.0.0.1:5000/()
# 127.0.0.1:5000/(index.html)
# 127.0.0.1:5000/ register.html
# 127.0.0.1:5000/favicon.ico#浏览器认为的网站标识,
@html.route("/<re(r'.*'):html_file_name>")
def get_html(html_file_name):
    """提供html文件"""
    # 如果html_file_name为"",表示访问的路径是 /，请求的是主页
    if not html_file_name:
        html_file_name = "index.html"
    if html_file_name != "favicon.ico":
        html_file_name = "html/" + html_file_name
    # flask提供的返回静态文件的方法
    return current_app.send_static_file(html_file_name)
```

/ihome/utils/commons.py

```python
from werkzeug.routing import BaseConverter


# 定义re转换器
class ReConverter(BaseConverter):
    """"""
    def __init__(self,url_map,regex):
        super(ReConverter,self).__init__(url_map)
        self.regex = regex
```

# csrf防护机制

![数据迁移](../images/ihome/csrf防护过程.png)



![数据迁移](../images/ihome/未开启csrf防护时的攻击.png)





![数据迁移](../images/ihome/开启csrf防护机制.png)

```python
from flask import Blueprint,current_app,make_response
from flask_wtf import csrf

html = Blueprint('web_html',__name__)


# 127.0.0.1:5000/()
# 127.0.0.1:5000/(index.html)
# 127.0.0.1:5000/ register.html
# 127.0.0.1:5000/favicon.ico#浏览器认为的网站标识,
@html.route("/<re(r'.*'):html_file_name>")
def get_html(html_file_name):
    """提供html文件"""
    # 如果html_file_name为"",表示访问的路径是 /，请求的是主页
    if not html_file_name:
        html_file_name = "index.html"
    if html_file_name != "favicon.ico":
        html_file_name = "html/" + html_file_name

    # 创建csrf——token值
    csrf_token = csrf.generate_csrf()
    # flask提供的返回静态文件的方法
    res = make_response(current_app.send_static_file(html_file_name))
    # 设置cookie值
    res.set_cookie('csrf_token',csrf_token)
    return res

```

# 图片验证码接口

## 图片验证码流程

![数据迁移](../images/ihome/图片验证码流程.png)

## 验证码图片生成接口

```python
from flask import current_app,jsonify,make_response

from . import api
from ihome.utils.captcha.captcha import captcha
from ihome import redis_link
from ihome.utils.constants import IMAGE_CODE_REDIS_EXPIRES
from ihome.utils.response_code import RET


@api.route("/get_image_codes/<image_code_id>")
def get_image_code(image_code_id):
    """
    # 获取图片验证码
    # http://127.0.0.1:5000/api/v1.0/image_codes/<image_code_id>
    :params image_code_id：图片验证码编号
    :return: 正常：验证码图片   错误:返回json
    """
    # 业务逻辑处理
    # 1.生成验证码图片
    # 名字  文本   图片
    name, text, image_data = captcha.generate_captcha()
    # 2.将验证码真实值与编号保存到redis中
    # 将验证码真实值与编号保存到redis中，设置有效期
    # redis: 字符串  列表  哈希  set
    # "key": XXX
    # 使用哈希维护有效期的时候只能整体设置
    # "image_codes":{"id1":"abc","":""}  哈希  hset("image_codes","id1","abc") hget("image_codes","id1")
    # 单条维护记录，选用字符串
    # "image_code_编号1":“真实值"
    try:
        redis_link.setex("image_code_%s" % image_code_id, IMAGE_CODE_REDIS_EXPIRES,text)
    except Exception as e:
        current_app.logger.error('redis连接错误',e)
        return jsonify(error=RET.DBERR,errmsg='save image code is failed')
    # 返回图片
    res = make_response(image_data)
    res.headers["Content-Type"] = "image/jpg"
    return res
```

验证码实现

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

# refer to `https://bitbucket.org/akorn/wheezy.captcha`

import random
import string
import os.path
from io import BytesIO

from PIL import Image
from PIL import ImageFilter
from PIL.ImageDraw import Draw
from PIL.ImageFont import truetype


class Bezier:
    def __init__(self):
        self.tsequence = tuple([t / 20.0 for t in range(21)])
        self.beziers = {}

    def pascal_row(self, n):
        """ Returns n-th row of Pascal's triangle
        """
        result = [1]
        x, numerator = 1, n
        for denominator in range(1, n // 2 + 1):
            x *= numerator
            x /= denominator
            result.append(x)
            numerator -= 1
        if n & 1 == 0:
            result.extend(reversed(result[:-1]))
        else:
            result.extend(reversed(result))
        return result

    def make_bezier(self, n):
        """ Bezier curves:
            http://en.wikipedia.org/wiki/B%C3%A9zier_curve#Generalization
        """
        try:
            return self.beziers[n]
        except KeyError:
            combinations = self.pascal_row(n - 1)
            result = []
            for t in self.tsequence:
                tpowers = (t ** i for i in range(n))
                upowers = ((1 - t) ** i for i in range(n - 1, -1, -1))
                coefs = [c * a * b for c, a, b in zip(combinations,
                                                      tpowers, upowers)]
                result.append(coefs)
            self.beziers[n] = result
            return result


class Captcha(object):
    def __init__(self):
        self._bezier = Bezier()
        self._dir = os.path.dirname(__file__)
        # self._captcha_path = os.path.join(self._dir, '..', 'static', 'captcha')

    @staticmethod
    def instance():
        if not hasattr(Captcha, "_instance"):
            Captcha._instance = Captcha()
        return Captcha._instance

    def initialize(self, width=200, height=75, color=None, text=None, fonts=None):
        # self.image = Image.new('RGB', (width, height), (255, 255, 255))
        self._text = text if text else random.sample(string.ascii_uppercase + string.ascii_uppercase + '3456789', 4)
        self.fonts = fonts if fonts else \
            [os.path.join(self._dir, 'fonts', font) for font in ['Arial.ttf', 'Georgia.ttf', 'actionj.ttf']]
        self.width = width
        self.height = height
        self._color = color if color else self.random_color(0, 200, random.randint(220, 255))

    @staticmethod
    def random_color(start, end, opacity=None):
        red = random.randint(start, end)
        green = random.randint(start, end)
        blue = random.randint(start, end)
        if opacity is None:
            return red, green, blue
        return red, green, blue, opacity

    # draw image

    def background(self, image):
        Draw(image).rectangle([(0, 0), image.size], fill=self.random_color(238, 255))
        return image

    @staticmethod
    def smooth(image):
        return image.filter(ImageFilter.SMOOTH)

    def curve(self, image, width=4, number=6, color=None):
        dx, height = image.size
        dx /= number
        path = [(dx * i, random.randint(0, height))
                for i in range(1, number)]
        bcoefs = self._bezier.make_bezier(number - 1)
        points = []
        for coefs in bcoefs:
            points.append(tuple(sum([coef * p for coef, p in zip(coefs, ps)])
                                for ps in zip(*path)))
        Draw(image).line(points, fill=color if color else self._color, width=width)
        return image

    def noise(self, image, number=50, level=2, color=None):
        width, height = image.size
        dx = width / 10
        width -= dx
        dy = height / 10
        height -= dy
        draw = Draw(image)
        for i in range(number):
            x = int(random.uniform(dx, width))
            y = int(random.uniform(dy, height))
            draw.line(((x, y), (x + level, y)), fill=color if color else self._color, width=level)
        return image

    def text(self, image, fonts, font_sizes=None, drawings=None, squeeze_factor=0.75, color=None):
        color = color if color else self._color
        fonts = tuple([truetype(name, size)
                       for name in fonts
                       for size in font_sizes or (65, 70, 75)])
        draw = Draw(image)
        char_images = []
        for c in self._text:
            font = random.choice(fonts)
            c_width, c_height = draw.textsize(c, font=font)
            char_image = Image.new('RGB', (c_width, c_height), (0, 0, 0))
            char_draw = Draw(char_image)
            char_draw.text((0, 0), c, font=font, fill=color)
            char_image = char_image.crop(char_image.getbbox())
            for drawing in drawings:
                d = getattr(self, drawing)
                char_image = d(char_image)
            char_images.append(char_image)
        width, height = image.size
        offset = int((width - sum(int(i.size[0] * squeeze_factor)
                                  for i in char_images[:-1]) -
                      char_images[-1].size[0]) / 2)
        for char_image in char_images:
            c_width, c_height = char_image.size
            mask = char_image.convert('L').point(lambda i: i * 1.97)
            image.paste(char_image,
                        (offset, int((height - c_height) / 2)),
                        mask)
            offset += int(c_width * squeeze_factor)
        return image

    # draw text
    @staticmethod
    def warp(image, dx_factor=0.27, dy_factor=0.21):
        width, height = image.size
        dx = width * dx_factor
        dy = height * dy_factor
        x1 = int(random.uniform(-dx, dx))
        y1 = int(random.uniform(-dy, dy))
        x2 = int(random.uniform(-dx, dx))
        y2 = int(random.uniform(-dy, dy))
        image2 = Image.new('RGB',
                           (width + abs(x1) + abs(x2),
                            height + abs(y1) + abs(y2)))
        image2.paste(image, (abs(x1), abs(y1)))
        width2, height2 = image2.size
        return image2.transform(
            (width, height), Image.QUAD,
            (x1, y1,
             -x1, height2 - y2,
             width2 + x2, height2 + y2,
             width2 - x2, -y1))

    @staticmethod
    def offset(image, dx_factor=0.1, dy_factor=0.2):
        width, height = image.size
        dx = int(random.random() * width * dx_factor)
        dy = int(random.random() * height * dy_factor)
        image2 = Image.new('RGB', (width + dx, height + dy))
        image2.paste(image, (dx, dy))
        return image2

    @staticmethod
    def rotate(image, angle=25):
        return image.rotate(
            random.uniform(-angle, angle), Image.BILINEAR, expand=1)

    def captcha(self, path=None, fmt='PNG'):
        """Create a captcha.

        Args:
            path: save path, default None.
            fmt: image format, PNG / JPEG.
        Returns:
            A tuple, (name, text, StringIO.value).
            For example:
                ('fXZJN4AFxHGoU5mIlcsdOypa', 'JGW9', '\x89PNG\r\n\x1a\n\x00\x00\x00\r...')

        """
        image = Image.new('RGB', (self.width, self.height), (255, 255, 255))
        image = self.background(image)
        image = self.text(image, self.fonts, drawings=['warp', 'rotate', 'offset'])
        image = self.curve(image)
        image = self.noise(image)
        image = self.smooth(image)
        name = "".join(random.sample(string.ascii_lowercase  + string.ascii_uppercase  + '3456789', 24))
        text = "".join(self._text)
        out = BytesIO()
        # image.save(out, format=fmt)
        image.save(out, format=fmt)
        if path:
            image.save(os.path.join(path, name), fmt)
        return name, text, out.getvalue()

    def generate_captcha(self):
        self.initialize()
        return self.captcha("")


captcha = Captcha.instance()

if __name__ == '__main__':
    captcha.generate_captcha()
```

## 单元测试

```python
import unittest
from flask import Flask
from flask.testing import FlaskClient
from your_module import api

class APITestCase(unittest.TestCase):
    def setUp(self):
        self.app = Flask(__name__)
        self.client = self.app.test_client()
        self.app.config['TESTING'] = True

    def test_get_image_code(self):
        with self.app.test_request_context("/get_image_codes/123"):
            response = self.client.get("/get_image_codes/123")
            self.assertEqual(response.status_code, 200)
            self.assertEqual(response.headers["Content-Type"], "image/jpg")

    def tearDown(self):
        pass

if __name__ == '__main__':
    unittest.main()
```

## 问题

图片验证码使用内存保存时候验证码出不来

先导入`from io import BytesIO`,然后将`out = StringIO()`改成`out = BytesIO()`

```python
Traceback (most recent call last):
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 2464, in __call__
    return self.wsgi_app(environ, start_response)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 2450, in wsgi_app
    response = self.handle_exception(e)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 1867, in handle_exception
    reraise(exc_type, exc_value, tb)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/_compat.py", line 39, in reraise
    raise value
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 2447, in wsgi_app
    response = self.full_dispatch_request()
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 1952, in full_dispatch_request
    rv = self.handle_user_exception(e)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 1821, in handle_user_exception
    reraise(exc_type, exc_value, tb)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/_compat.py", line 39, in reraise
    raise value
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 1950, in full_dispatch_request
    rv = self.dispatch_request()
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/app.py", line 1936, in dispatch_request
    return self.view_functions[rule.endpoint](**req.view_args)
  File "/home/feelapple/flask/flask-Ihome/ihome/api_v1_0/verify_code.py", line 22, in get_image_code
    name, text, image_data = captcha.generate_captcha()
  File "/home/feelapple/flask/flask-Ihome/ihome/utils/captcha/captcha.py", line 219, in generate_captcha
    return self.captcha("")
  File "/home/feelapple/flask/flask-Ihome/ihome/utils/captcha/captcha.py", line 212, in captcha
    image.save(out, format=fmt)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/PIL/Image.py", line 2432, in save
    save_handler(self, fp, filename)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/PIL/PngImagePlugin.py", line 1294, in _save
    fp.write(_MAGIC)
TypeError: string argument expected, got 'bytes'
```

## 接口文档

```
[TOC]

##### 简要描述

- 获取图片验证码接口

##### 请求URL
- ` http://dwz.feelapple.cn/api/v1.0/get_image_codes/1 `
  
##### 请求方式
- get 

##### 参数

|参数名|        必选 |类型    |说明|
|:----         |:---|:----- |-----   |
|image_code_id |是  |string |验证码图片编号   |


 ##### 返回示例
​``` 
  {
    "error": 4001,
	"errormsg": "save image code is failed",
    "data": {
      "uid": "1",
      "username": "12154545",
      "name": "吴系挂",
      "groupid": 2 ,
      "reg_time": "1436864169",
      "last_login_time": "0",
    }
  }
​```
 ##### 返回参数说明 

|参数名|类型|说明|
|:-----  |:-----|----- |
|error |str   |错误代码  |
|errmsg |str   |错误内容  |
|res |image   |返回验证码图片  |

 ##### 备注

- 更多返回错误代码请看首页的错误代码描述
```

# 状态码

```python
	RET.OK: u"成功",                   OK = "0"                                
    RET.DBERR: u"数据库查询错误",        DBERR = "4001"
    RET.NODATA: u"无数据", 		     NODATA = "4002"
    RET.DATAEXIST: u"数据已存在",	    DATAEXIST = "4003"
    RET.DATAERR: u"数据错误",	         DATAERR = "4004"
    RET.SESSIONERR: u"用户未登录",       SESSIONERR = "4101"
    RET.LOGINERR: u"用户登录失败",       LOGINERR = "4102"
    RET.PARAMERR: u"参数错误",           PARAMERR = "4103"
    RET.USERERR: u"用户不存在或未激活",    USERERR = "4104"
    RET.ROLEERR: u"用户身份错误",         ROLEERR = "4105"
    RET.PWDERR: u"密码错误",		     PWDERR = "4106"
    RET.REQERR: u"非法请求或请求次数受限",	REQERR = "4201"
    RET.IPERR: u"IP受限",				  IPERR = "4202"
    RET.THIRDERR: u"第三方系统错误",	  THIRDERR = "4301"
    RET.IOERR: u"文件读写错误",		   IOERR = "4302"
    RET.SERVERERR: u"内部错误",			SERVERERR = "4500"
    RET.UNKOWNERR: u"未知错误",			UNKOWNERR = "4501"
```

# 发送邮件验证码接口

send_email.py

```python
# get
# http://127.0.0.1:5000/api/v1.0/get_email_code/<email>?image_code=xxx&image_code_id=xxx
@api.route("/get_email_codes/<email>")# <re(r'[0-9a-zA-Z]\w*@[0-9a+zA-Z][0-9a-zA-Z.]*(.net|.com|.edu|.ort)$'):email>
def get_email_code(email):
    # 获取参数
    image_code = request.args.get("image_code")
    image_code_id = request.args.get("image_code_id")
    # 检验参数
    if not all([image_code, image_code_id]):
        return jsonify(errno=RET.PARAMERR, errmsg='args is incompleteness')
    # 业务逻辑
    # 从redis中取出真实的图片验证码
    try:
        real_image_code = redis_link.get("image_code_%s" % image_code_id)
        print(real_image_code)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg='database error')
    # 判断图片验证码是否过期
    if real_image_code is None:
        # 表示图片验证码没有或者过期
        return jsonify(errno=RET.NODATA, errmsg='no img code data')
    # 删除redis中的图片验证码，防止用户使用同一个图片验证码验证多次
    try:
        redis_link.delete("image_code_%s" % image_code_id)
    except Exception as e:
        current_app.logger.error(e)
    # 与用户填写的值进行对比
    if real_image_code.upper() != image_code.encode().upper():
        return jsonify(errno=RET.DATAERR, errmsg='code comparison failed')

    # 判断手机号的操作，在60秒之内有没有操作，如果有，则返回60内只能发送一次邮件
    try:
        send_flag = redis_link.get("email_code_time_%s" % email)

    except Exception as e:
        current_app.logger.error(e)
    else:
        if send_flag is not None:
            return jsonify(errno=RET.REQERR, errmsg='You can only send an email once within 60 seconds')
    # 判断邮箱是否存在
    try:
        user = User.query.filter(email=email).first()
    except Exception as e:
        current_app.logger.error(e)
    else:
        if user is not None:
            return jsonify(errno=RET.DATAEXIST, errmsg='data exist')
    # 如果手机号不存在，则生成短信验证码
    # 6位，数字和字母的组成
    source = string.digits * 6
    captcha = random.sample(source, 6)
    # 列表变成字符串
    captcha = "".join(captcha)  # 965056
    print(captcha)
    # 保存真实的短信验证码
    try:
        redis_link.setex("email_code_%s" % email, SMS_CODE_REDIS_EXPIRES, captcha)
        # 保存发送给这个手机号的记录，防止用户在60s内再次出发发送短信的操作
        redis_link.setex("email_code_time_%s" % email,SEND_SMS_CODE_INTERVAL,1)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg='save code abnormal')
    # 发送短信
    result = send_email(accept_qq=email, captcha=captcha)
    # 返回值
    if result == 0:
        return jsonify(errno=RET.OK, errmsg='send success')
    return jsonify(errno=RET.THIRDERR, errmsg='send fail')
```

send_msg.py

```python
from flask import current_app, jsonify
from flask_mail import Message
from ihome import mail
from ihome.utils import constants



def send_email(accept_qq, captcha):
    try:
        # sender 发送方，recipients 接收方列表
        msg = Message("验证码", sender=constants.QQ, recipients=[accept_qq])  # '1329934054@qq.com'
        # 邮件内容
        msg.body = captcha
        # 发送邮件
        mail.send(msg)
        print("Mail sent")
        current_app.logger.info('已成功发送信息给qq:%s' % accept_qq)
        return 0
    except Exception as e:
        current_app.logger.error('发送验证码失败', e)
        return 1
```

# 注册接口

passport.py

```python
@api.route("/users", methods=["POST"])
def register():
    """
    注册
    args: email, email_code, 密码
    格式: json
    :return:
    """
    # 获取请求的json数据,返回字典
    req_dict = request.get_json()
    email = req_dict.get("email")
    email_code = req_dict.get("email_code")
    password = req_dict.get("password")
    confirm_password = req_dict.get("confirm_password")
    # 校验参数
    if not all([email, email_code, password, confirm_password]):
        return jsonify(errno=RET.PARAMERR, errmsg='参数不完整')
    # 校验邮箱格式
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if not re.match(pattern, email):
        return jsonify(errno=RET.DATAERR, errmsg='邮箱格式错误')
    # 密码校验
    if password != confirm_password:
        return jsonify(errno=RET.PARAMERR, errmsg='两次密码不一致')
    # 校验email发送的code
    # redis 拿取数据
    try:
        redis_email_code = redis_link.get("email_code_%s" % email)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="读取真实短信验证码失败")
    # 判断短信验证码是否过期
    if redis_email_code is None:
        return jsonify(errno=RET.NODATA, errmsg="验证码失效")
    # 判断用户填写短信验证码的正确性
    if redis_email_code != email_code.encode():
        return jsonify(errno=RET.DATAERR, errmsg="验证码填写错误")
    # 判断用户的email是否注册过
    # 保存用户的注册数据到数据库中
    user = User(name=email, email=email)
    # 保存密码
    user.password = password
    try:
        db.session.add(user)
        db.session.commit()
    except IntegrityError as e:
        # 数据库操作错误后的回滚
        db.session.rollback()
        # 表示邮箱以注册
        current_app.logger.error(e)
        return jsonify(errno=RET.DATAEXIST, errmsg="email以存在")
    except Exception as e:
        db.session.rollback()
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库异常")
    # 保存登录状态到session中
    session["name"] = email
    session["email"] = email
    session["user_id"] = user.id
    # 返回结果
    return jsonify(errno=RET.OK, errmsg="注册成功")
```

register.js

```js
function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}

var imageCodeId = "";

$(".form-register").submit(function(e){
        // 阻止浏览器对于表单的默认自动提交行为
        e.preventDefault();
        var email = $("#email").val();
        var email_code = $("#phonecode").val();
        var password = $("#password").val();
        var confirm_password = $("#password2").val();
        if (!email) {
            $("#mobile-err span").html("请填写正确的手机号！");
            $("#mobile-err").show();
            return;
        } 
        if (!email_code) {
            $("#phone-code-err span").html("请填写短信验证码！");
            $("#phone-code-err").show();
            return;
        }
        if (!password) {
            $("#password-err span").html("请填写密码!");
            $("#password-err").show();
            return;
        }
        if (password != confirm_password) {
            $("#password2-err span").html("两次密码不一致!");
            $("#password2-err").show();
            return;
        }
// 调用ajax向后端发送注册请求
        var req_data = {
            email:email,
            email_code:email_code,
            password:password,
            confirm_password:confirm_password,
        }
        var req_json = JSON.stringify(req_data);
        $.ajax({
            url:"/api/v1.0/users",
            type:"post",
            data: req_json,
            contentType:"application/json",
            dataType:"json",
            headers: {
              "X-CSRFToken":  getCookie("csrf_token")
            },
            success:function (res) {
                if (res.errno == "0"){
                    location.href = "/index.html";
                }else {
                    $("#password-err span").html(res.errmsg);
                    $("#password-err span").show();
                }
            }
        })
    });
})
```

# 用户登录接口

passport.py

```python
@api.route("/sessions", methods=["POST"])
def login():
    """
    用户登录
    args  email ,password   json
    :return:
    """
    # 获取参数
    req_dict = request.get_json()
    email = req_dict.get("email")
    password = req_dict.get("password")
    # 校验参数
    if not all([email, password]):
        return jsonify(errno=RET.PARAMERR, errmsg='参数不完整')
    # email格式
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if not re.match(pattern, email):
        return jsonify(errno=RET.DATAERR, errmsg='邮箱格式错误')
    # 判断错误次数是否超过限制,如果超过限制，则返回
    # redis :"access_nums_userIp":num
    user_ip = request.remote_user  # user_ip
    try:
        access_nums = redis_link.get("access_nums_%s" % user_ip)
    except Exception as e:
        current_app.logger.error(e)
    else:
        if access_nums is not None and int(access_nums) >= LOGIN_ERROR_MAX_TIMES:
            return jsonify(errno=RET.REQERR, errmsg='超过请求次数,请稍后尝试')
    # 从数据库中根据email查询用户的数据对象
    try:
        user = User.query.filter_by(email=email).first()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg='获取用户信息失败')
    # 用数据库的密码与用户填写的密码进行对比验证
    if user is None or user.check_password(password) is False:
        # 如果验证失败，记录错误次数，返回信息
        try:
            redis_link.incr("access_nums_%s" % user_ip)
            redis_link.expire("access_nums_%s" % user_ip, LOGIN_ERROR_FORBID_TIME)
        except:
            current_app.logger.error(e)
        return jsonify(errno=RET.DATAERR, errmsg='用户名或者密码错误')
    # 如果验证相同成功,保存登录状态,在session中
    session["name"] = email
    session["email"] = email
    session["user_id"] = user.id
    return jsonify(errno=RET.OK, errmsg="登录成功")
```

login.js

```js
function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}

$(document).ready(function() {
    $("#email").focus(function(){
        $("#mobile-err").hide();
    });
    $("#password").focus(function(){
        $("#password-err").hide();
    });
    $(".form-login").submit(function(e){
        e.preventDefault();
        email = $("#email").val();
        password = $("#password").val();
        if (!email) {
            $("#mobile-err span").html("请填写正确的email！");
            $("#mobile-err").show();
            return;
        } 
        if (!password) {
            $("#password-err span").html("请填写密码!");
            $("#password-err").show();
            return;
        }
        // 调用ajax向后端发送注册请求
        var req_data = {
            email:email,
            password:password,
        }
        var req_json = JSON.stringify(req_data);
        $.ajax({
            url:"/api/v1.0/sessions",
            type:"post",
            data: req_json,
            contentType:"application/json",
            dataType:"json",
            headers: {
              "X-CSRFToken":  getCookie("csrf_token")
            },
            success:function (res) {
                if (res.errno == "0"){
                    location.href = "/index.html";
                }else {
                    $("#password-err span").html(res.errmsg);
                    $("#password-err span").show();
                }
            }
        })
    });
})
```

# 检查用户登录状态和退出接口

检查用户登录状态

```python
@api.route("/sessions", methods=["GET"])
def check_login():
    """
    查询登录状态
    :return:
    """
    name = session.get("name")
    if name is not None:
        return jsonify(errno=RET.OK, errmsg="true", data={"name": name})
    else:
        return jsonify(errno=RET.SESSIONERR, errmsg="false")

```

```js
$.get("/api/v1.0/sessions",function (res) {
        if ("0" == res.errno){
            $(".top-bar>.user-info>.user-name").html(res.data.name)
            $(".top-bar>.user-info").show();
        }else {
            $(".top-bar>.register-login").show();
        }
    },"json")
```

退出

```python
@api.route("/sessions", methods=["DELETE"])
def logout():
    """
    登出
    :return:
    """
    # 清除session数据
    csrf_token = session.get("csrf_token")
    session.clear()
    session['csrf_token'] = csrf_token
    return jsonify(errno=RET.OK, errmsg="OK")
```

```js
function logout() {
    $.ajax({
        url:"/api/v1.0/sessions",
        type:"delete",
        dataType:"json",
        headers: {
          "X-CSRFToken":  getCookie("csrf_token")
        },
        success:function (res) {
            if (res.errno == "0"){
                location.href = "/index.html";
            }
        }
    })
}
```

# 登录验证装饰器

```python
from flask import session,jsonify,g
from ihome.utils.response_code import RET
import functools

# 登录装饰器
def login_required(view_func):
    @functools.wraps(view_func)
    def wrapper(*args,**kwargs):
        # 判断用户的登录状态
        user_id = session.get("user_id")
        # 如果用户是登录的，执行视图函数
        if user_id is not None:
            # 将user id保存到g对象中，在视图函数中可以通过g对象获取保存数据
            g.user_id = user_id
            return view_func(*args,**kwargs)
        # 如果未登录, 返回未登录的信息
        else:
            return jsonify(errno=RET.SESSIONERR, errmsg="用户未登录")
    return wrapper
```

# FastDFS 快速分布式文件存储系统调用接口

```
pip install fdfs-client-py3
创建fdfs包
创建配置文件
fdfs_client.conf
```

```
# connect timeout in seconds
# default value is 30s
connect_timeout=30

# network timeout in seconds
# default value is 30s
network_timeout=60

# the base path to store log files
#base_path=/home/tarena/django-project/cc_shop1/cc_shop1/logs

# tracker_server can ocur more than once, and tracker_server format is
#  "host:port", host can be hostname or ip address
tracker_server=192.168.1.109:22122

#standard log level as syslog, case insensitive, value list:
### emerg for emergency
### alert
### crit for critical
### error
### warn for warning
### notice
### info
### debug
log_level=info

# if use connection pool
# default value is false
# since V4.05
use_connection_pool = false

# connections whose the idle time exceeds this time will be closed
# unit: second
# default value is 3600
# since V4.05
connection_pool_max_idle_time = 3600

# if load FastDFS parameters from tracker server
# since V4.05
# default value is false
load_fdfs_parameters_from_tracker=false

# if use storage ID instead of IP address
# same as tracker.conf
# valid only when load_fdfs_parameters_from_tracker is false
# default value is false
# since V4.05
use_storage_id = false

# specify storage ids filename, can use relative or absolute path
# same as tracker.conf
# valid only when load_fdfs_parameters_from_tracker is false
# since V4.05
storage_ids_filename = storage_ids.conf


#HTTP settings
http.tracker_server_port=80

#use "#include" directive to include HTTP other settiongs
##include http.conf
```

创建fdfs.py

```python
import traceback
from fdfs_client.client import *


class FDFS(object):
    def __init__(self, client_file="fdfs_client.conf"):
        self.client_file = client_file
        self.client = self.create_client()

    def create_client(self):
        try:
            print("Client file:", self.client_file)  # 打印客户端配置文件路径
            client = Fdfs_client(self.client_file)
            print("Client object:", client)  # 打印客户端对象
            return client
        except Exception as e:
            print("FastDFS Create file fail, {0}, {1}".format(e, traceback.print_exc()))
            return None

    def download(self, file_name, file_id):
        try:
            ret_download = self.client.download_to_file(file_name, file_id)
            print(ret_download)
            return True
        except Exception as e:
            print("FastDFS download file fail, {0}, {1}".format(e, traceback.print_exc()))
            return False

    def upload(self, file_name):
        try:
            ret_upload = self.client.upload_by_filename(file_name)
            print(ret_upload)
            return ret_upload
        except Exception as e:
            print("FastDFS upload file fail, {0}, {1}".format(e, traceback.print_exc()))
            return False

    def delete(self, file_id):
        try:
            ret_delete = self.client.delete_file(file_id)
            print(ret_delete)
            return True
        except Exception as e:
            print("FastDFS delete file fail, {0}, {1}".format(e, traceback.print_exc()))
            return False
```

# 保存用户头像接口

profile.py

```python
import os
from flask import g, current_app, jsonify, request

from . import api
from ihome.utils.commons import login_required
from ihome.utils.response_code import RET
from ihome.utils.fdfs.fdfs import FDFS
from ihome.models import User
from ihome import db
from ihome.utils.constants import FASTdfs_URL


@api.route("/users/avatar", methods=["POST"])
@login_required
def set_user_avatar():
    """
    设置用户图像
    参数：图片  用户id(g.user_id)
    :return:
    """
    # 装饰器的代码中已经将user_id保存到g对象中，所以视图中可以直接读取
    user_id = g.user_id
    # 获取图片
    image_file = request.files.get("avatar")

    if image_file is None:
        return jsonify(errno=RET.NODATA, errmsg="未传图片")

    # 图片保存的文件夹路径
    output_folder = "../static/images/user_image"

    # 创建目标文件夹（如果不存在）
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)

    # 保存图片到目标文件夹
    image_path = os.path.join(output_folder, image_file.filename)
    image_file.save(image_path)

    # 关闭图片文件
    image_file.close()

    fdfs = FDFS("ihome/utils/fdfs/fdfs_client.conf")
    image_upload = fdfs.upload(image_path)
    if image_upload == "False":
        return jsonify(errno=RET.OK, errmsg="图片上传失败")

    # 保存image_url到数据库
    avatar_image_url = FASTdfs_URL + image_upload["Remote file_id"]
    try:
        User.query.filter_by(id=user_id).update({"avatar_url": avatar_image_url})
        db.session.commit()
    except Exception as e:
        db.session.rollback()
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库保存图片失败")
    return jsonify(errno=RET.OK, errmsg="图片保存成功", data={"avatar_image_url": avatar_image_url})
```

前端 profile.js

```js
function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}

$(document).ready(function () {
    $("#form-avatar").submit(function (e) {
        e.preventDefault();
        $(this).ajaxSubmit({
            url: "/api/v1.0/users/avatar",
            type: "post",
            dataType: "json",
            headers: {
                "X-CSRFToken": getCookie("csrf_token")
            },
            success: function (res) {
                if (res.errno == "0") {
                    var avatar_image_url = res.data.avatar_image_url;
                    $("#user-avatar").attr("src", avatar_image_url);
                } else {
                    alert(res.errmsg);
                }
            }
        })

    })
})
```

# 保存用户昵称接口

profile.py

```python
@api.route("/users/nickname", methods=["POST"])
@login_required
def set_user_nickname():
    # 装饰器的代码中已经将user_id保存到g对象中，所以视图中可以直接读取
    user_id = g.user_id
    # 获取参数
    req_dict = request.get_json()
    nickname = req_dict.get("nickname")

    # 校验参数
    if not nickname:
        return jsonify(errno=RET.PARAMERR, errmsg='未填写昵称')
    # 保存数据
    try:
        User.query.filter_by(id=user_id).update({"name": nickname})
        db.session.commit()
    except Exception as e:
        db.session.rollback()
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库保存昵称失败")
    return jsonify(errno=RET.OK, errmsg="昵称保存成功", data={"nickname": nickname})
```

profile.js

```js
$("#form-name").submit(function (e) {
        e.preventDefault();
        var nickname = $("#user-name").val();
        var req_data = {
            nickname: nickname
        }
        var req_json = JSON.stringify(req_data);
        $.ajax({
            url: "/api/v1.0/users/nickname",
            type: "post",
            data: req_json,
            contentType: "application/json",
            dataType: "json",
            headers: {
                "X-CSRFToken": getCookie("csrf_token")
            },
            success: function (res) {
                if (res.errno == "0") {
                    alert(res.errno);
                } else {
                    alert(res.errmsg);
                }
            }
        })

    })
```

# 个人信息显示接口

profile.py

```python
@api.route("/users/user_info", methods=["GET"])
@login_required
def get_user_info():
    # 装饰器的代码中已经将user_id保存到g对象中，所以视图中可以直接读取
    user_id = g.user_id
    # 通过id查询
    user = User.query.filter_by(id=user_id).first()
    # 调取models中的函数to_dict
    to_dict = user.to_dict()
    data = {"name": to_dict["name"], "email": to_dict["email"], "avatar_url": to_dict["avatar"]}
    print(data)
    return jsonify(errno=RET.OK, errmsg="数据获取成功", data=data)
```

my.js

```js
$(document).ready(function(){
    $.get("/api/v1.0/users/user_info",function (res) {
        if ("0" == res.errno){
            $("#user-avatar").attr("src",res.data.avatar_url)
            $("#user-name").html(res.data.name)
            $("#user-email").html(res.data.email)
        }
    },"json")
})
```

# 实名认证接口

**python中re模块匹配身份证 python中身份证验证**

```
# 首先，要安装这个库，windows+R键运行cmd，打开命令行窗口，输入下面的命令：

pip install id_validator

from id_validator import validator
# 校验身份证，返回False，校验身份证合法性失败，返回True，校验身份证合法性成功：
validator.is_valid('440308199901111512')  #大陆18位身份证
validator.is_valid('610104620927690')  #大陆15位身份证
validator.is_valid('810000199408230021') #港澳18位身份证
validator.is_valid('830000199201300022') #台湾18位身份证

# 得到身份证具体信息
validator.get_info('330221199306084914')
```

auth.py

```python
from flask import g, current_app, jsonify, request
from id_validator import validator

from . import api
from ihome.utils.commons import login_required
from ihome.utils.response_code import RET
from ihome.models import User, Person
from ihome import db


@api.route("/users/auths", methods=["POST"])
@login_required
def set_user_auth():
    # 装饰器的代码中已经将user_id保存到g对象中，所以视图中可以直接读取
    user_id = g.user_id
    # 获取参数
    req_dict = request.get_json()
    real_name = req_dict.get("real_name")
    id_card = req_dict.get("id_card")

    # 校验参数
    if not all([real_name, id_card]):
        return jsonify(errno=RET.PARAMERR, errmsg='未填写用户名或姓名')
    validator_id_card_result = validator.is_valid(id_card)
    if validator_id_card_result == "False":
        return jsonify(errno=RET.DATAERR, errmsg='身份证无效')
    # 得到身份证具体信息  保存到person表中
    validator_id_card_info = validator.get_info(id_card)
    # validator_id_card_info.pop("address_tree")
    # 保存数据
    try:
        # 查询 User 表中指定 id 的记录
        user = User.query.filter_by(id=user_id).first()
        # 创建一个新的 Person 实例
        # 设置 Person 实例的属性
        # person = Person(**validator_id_card_info)
        person = Person(address_code=validator_id_card_info['address_code'],
                abandoned=validator_id_card_info['abandoned'],
                address=validator_id_card_info['address'],
                age=validator_id_card_info['age'],
                birthday_code=validator_id_card_info['birthday_code'],
                constellation=validator_id_card_info['constellation'],
                chinese_zodiac=validator_id_card_info['chinese_zodiac'],
                sex=validator_id_card_info['sex'],
                length=validator_id_card_info['length'],
                check_bit=validator_id_card_info['check_bit'])
        # 将 Person 实例与 User 关联
        user.person = person
        db.session.commit()
    except Exception as e:
        db.session.rollback()
        current_app.logger.error(e)
    try:
        user.real_name = real_name
        user.id_card = id_card
        db.session.commit()
    except Exception as e:
        db.session.rollback()
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库保存失败")
    return jsonify(errno=RET.OK, errmsg="实名认证成功", data={"real_name": real_name,"id_card": id_card})
```

auth.js

```js
$(document).ready(function () {
    $("#form-auth").submit(function (e) {
        e.preventDefault();
        var real_name = $("#real-name").val();
        var id_card = $("#id-card").val();
        var req_data = {
            real_name: real_name,
            id_card: id_card,
        }
        var req_json = JSON.stringify(req_data);
        $.ajax({
            url: "/api/v1.0/users/auths",
            type: "post",
            data: req_json,
            contentType: "application/json",
            dataType: "json",
            headers: {
                "X-CSRFToken": getCookie("csrf_token")
            },
            success: function (res) {
                if (res.errno == "0") {
                    alert(res.errmsg);
                } else {
                    alert(res.errmsg);
                }
            }
        })
    })


})
```

# 实名认证显示信息接口

auth.py

```python
@api.route("/users/real_info", methods=["GET"])
@login_required
def get_user_real_info():
    # 装饰器的代码中已经将user_id保存到g对象中，所以视图中可以直接读取
    user_id = g.user_id
    # 通过id查询
    try:
        user = User.query.filter_by(id=user_id).first()
    except Exception as e:
        db.session.rollback()
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库保存失败")
    # 调取models中的函数to_dict
    to_dict = user.auth_to_dict()
    if to_dict["real_name"] == "":
        return jsonify(errno=RET.NODATA, errmsg="用户还未实名认证")
    # data = {"user_id": to_dict["user_id"],"real_name": to_dict["real_name"], "id_card": to_dict["id_card"]}
    return jsonify(errno=RET.OK, errmsg="数据获取成功", data=to_dict)
```

auth.js

```js
    $.get("/api/v1.0/users/real_info", function (res) {
        if ("0" == res.errno) {
            $("#real-name").val(res.data.real_name).prop("readonly", true);
            $("#id-card").val(res.data.id_card).prop("readonly", true);
        } else {
            $(".error-msg span").html(res.errmsg);
            $(".error-msg").show();
        }
    }, "json")
```

# 城区信息接口



```python
# 给Area表插入数据
	us1 = Area(name='瑶海区')
    us2 = Area(name='庐阳区')
    us3 = Area(name='蜀山区')
    us4 = Area(name='包河区')
    us5 = Area(name='长丰县')
    us6 = Area(name='肥东县')
    us7 = Area(name='肥西县')
    us8 = Area(name='庐江县')
    us9 = Area(name='巢湖市')
    db.session.add_all([us1, us2, us3, us4,us5, us6, us7, us8, us9])
    db.session.commit()
    
# 给Facility表插入数据    
    us1 = Facility(name='无线网络')
    us2 = Facility(name='热水淋浴')
    us3 = Facility(name='空调')
    us4 = Facility(name='暖气')
    us5 = Facility(name='允许吸烟')
    us6 = Facility(name='饮水设备')
    us7 = Facility(name='牙具')
    us8 = Facility(name='香皂')
    us9 = Facility(name='拖鞋')
    us10 = Facility(name='手纸')
    us11 = Facility(name='毛巾')
    us12 = Facility(name='沐浴露、洗发露')
    us13 = Facility(name='冰箱')
    us14 = Facility(name='洗衣机')
    us15 = Facility(name='电梯')
    us16 = Facility(name='允许做饭')
    us17 = Facility(name='允许带宠物')
    us18 = Facility(name='允许聚会')
    us19 = Facility(name='门禁系统')
    us20 = Facility(name='停车位')
    us21 = Facility(name='有线网络')
    us22 = Facility(name='电视')
    us23 = Facility(name='浴缸')
    db.session.add_all([us1,us2,us3,us4,us5,us6,us7,us8,us9,us10,
                        us11,us12,us13,us14,us15,us16,us17,us18,us19,
                        us20,us21,us22,us23])
    db.session.commit()
```

## 城区信息补充缓存

![数据迁移](../images/ihome/使用缓存的过程.png)

```
缓存数据的同步问题:
	保证mysql与redis数据的一致相同问题
	1.在操作mysql的时候，删除缓存数据
	2.给redis缓存数据设置有效期，保证过了有效期，缓存数据会被删除
```

```python
import json
from flask import g, current_app, jsonify

from . import api
from ihome.utils.response_code import RET
from ihome.models import Area
from ihome import redis_link
from ihome.utils.constants import AREA_INFO_REDIS_CACHE_EXPIRES


@api.route("/areas")
def get_area_info():
    """
    # 获取城区信息
    :return:
    """
    # 尝试从redis中读取数据
    try:
        res_json = redis_link.get("area_info")
    except Exception as e:
        current_app.logger.error(e)
    else:
        if res_json is not None:
            # redis有缓存数据
            current_app.logger.info("hit redis area_info")
            return res_json, 200, {"Content_Type": "application/json"}
    # 查询数据库，读取城区信息
    try:
        area_li = Area.query.all()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmag="数据库异常")
    area_dict_li = []
    for area in area_li:
        #     "aid":area.id,
        #     "aname": area.name
        d = area.to_dict()
        area_dict_li.append(d)
    # 将数据转换为json字符串
    res_dict = dict(errno=RET.OK, errmsg="OK", data=area_dict_li)
    res_json = json.dumps(res_dict)
    # 将数据保存到redis中
    try:
        redis_link.setex("area_info", AREA_INFO_REDIS_CACHE_EXPIRES, res_json)
    except Exception as e:
        current_app.logger.error(e)
    return res_json, 200, {"Content_Type": "application/json"}
```

# 保存房屋基本信息接口

```python
@api.route("/houses/info", methods=["POST"])
@login_required
def save_house_info():
    """
    保存房屋基本信息
    :return:
    前端发送过来的json数据
    {
    "title":"",
    "price":"",
    "area_id":"",
    "room_count":"",
    "acreage":"",
    "unit":"",
    "capacity":"",
    "beds":"",
    "deposit":"",
    "min_days":"",
    "max_days":"",
    "facility":["7","8"]
    }
    """
    # 获取数据
    user_id = g.user_id
    house_dict = request.get_json()
    title = house_dict.get("title")
    address = house_dict.get("address")
    price = house_dict.get("price")
    area_id = house_dict.get("area_id")
    room_count = house_dict.get("room_count")
    acreage = house_dict.get("acreage")  # 房屋面积
    unit = house_dict.get("unit")  # 房屋布局
    capacity = house_dict.get("capacity")  # 房屋容纳人数
    beds = house_dict.get("beds")  # 房屋卧床数目
    deposit = house_dict.get("deposit")  # 押金
    min_days = house_dict.get("min_days")
    max_days = house_dict.get("max_days")

    # 校验参数
    if not all([title, price, area_id, room_count, acreage, unit, capacity, beds, deposit, min_days, max_days]):
        return jsonify(errno=RET.PARAMERR, errmsg="参数不完整")
    # 判断金额是否正确
    try:
        price = int(float(price) * 100)
        deposit = int(float(deposit) * 100)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.PARAMERR, errmsg="参数错误")
    # 判断城区id是否存在
    try:
        area = Area.query.get(area_id)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据异常")
    if area is None:
        return jsonify(errno=RET.NODATA, errmsg="城区信息有误")
    # 保存房屋的信息
    house = House(
        user_id=user_id,
        area_id=area_id,
        title=title,
        price=price,
        address=address,
        room_count=room_count,
        acreage=acreage,
        unit=unit,
        capacity=capacity,
        beds=beds,
        deposit=deposit,
        min_days=min_days,
        max_days=max_days
    )
    try:
        db.session.add(house)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="保存数据异常")

    # 处理房屋设施信息
    facilitys = house_dict.get("facility")
    # 如果用户勾选设施信息，再保存数据库
    if facilitys:
        try:
            facilities = Facility.query.filter(Facility.id.in_(facilitys)).all()
        except Exception as e:
            current_app.logger.error(e)
            return jsonify(errno=RET.DBERR, errmsg="数据异常")
        if facilities:
            # 表示有合法的数据
            # 保存数据
            house.facilities = facilities
        try:
            db.session.add(house)
            db.session.commit()
        except Exception as e:
            db.session.rollback()
            current_app.logger.error(e)
            return jsonify(errno=RET.DBERR, errmsg="保存数据异常")
        return jsonify(errno=RET.OK, errmsg="OK", data={"house_id": house.id})
```

newhouse.js

```js
function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}

$(document).ready(function () {
    // 获取areas数据
    $.get("/api/v1.0/areas", function (res) {
        if (res.errno == "0") {
            var areas = res.data;
            // for (i = 0; i < areas.length; i++) {
                // var area = areas[i];
                // $("#area-id").append('<option value="'+ area.aid +'">'+area.aname+'</option>')
            // }
            // 使用js模版
            var html = template("areas-tmp",{areas:areas})
            $("#area-id").html(html)
        } else {
            alert(res.errmsg)
        }
    }, "json")
    var data = {}
    $("#form-house-info").submit(function (e) {
        e.preventDefault();
        //这段代码是使用jQuery库中的serializeArray()方法将表单 
        //#form-house-info中的所有字段和值转化为一个包含对象的数组。
        //然后通过map()方法遍历这个数组，将每个对象的name属性作为键，
        //value属性作为值，将其添加到一个名为data的对象中。
        //最终，data对象将包含所有表单字段的名字和对应的值。
        $("#form-house-info").serializeArray().map(function (x){
            data[x.name]=x.value
        })
        // 收集设施id信息
        var facility = [];
        
        //这段代码是使用jQuery库中的选择器和方法来获取所有被选中的具有name属性为facility的复选框。
        //代码中使用了:checked选择器来选择被选中的复选框元素。
        //然后，使用each()方法遍历选中的复选框元素。在每次遍历中，
        //index参数表示当前遍历的索引，x参数表示当前遍历的复选框元素。
        //在遍历过程中，代码将每个被选中的复选框的值($(x).val())
        //存储在一个名为facility的数组中，使用index作为索引。
        //最终，facility数组将包含所有被选中的复选框的值。
        $(":checked[name=facility]").each(function (index, x) {
            facility[index] = $(x).val()
        })
        data["facility"]=facility
        var req_json = JSON.stringify(data);
        // 向后端发送请求
        $.ajax({
            url:"/api/v1.0/houses/info",
            type:"post",
            data: req_json,
            contentType:"application/json",
            dataType:"json",
            headers: {
              "X-CSRFToken":  getCookie("csrf_token")
            },
            success:function (res) {
                if (res.errno == "4101"){
                    location.href = "/login.html";
                }else if(res.errno == "0") {
                    // 隐藏基本信息表单
                    $("#form-house-info").hide();
                    // 显示图片表单
                    $("#form-house-image").show();
                    // 设置图片表单的house_id
                    $("#house-id").val(res.data.house_id)
                }
            }
        })
    });
})
```

# 保存房屋照片接口

```python
@api.route("/houses/images", methods=["POST"])
@login_required
def save_house_image():
    """
    保存房屋图片
    agrs :image,house_id
    :return:
    """
    image_file = request.files.get("house_image")
    house_id = request.form.get("house_id")
    if not all([image_file, house_id]):
        return jsonify(errno=RET.PARAMERR, errmsg="参数错误")

    # 判断house_id正确性
    try:
        house = House.query.get(house_id)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库异常")
   	if house is None:
       return jsonify(errno=RET.NODATA, errmsg="房屋不存在")
    # 保存图片到fastdfs中
    # 图片保存的文件夹路径
    output_folder = "../static/images/user_image"

    # 创建目标文件夹（如果不存在）
    if not os.path.exists(output_folder):
        os.makedirs(output_folder)

    # 保存图片到目标文件夹
    image_path = os.path.join(output_folder, image_file.filename)
    image_file.save(image_path)

    # 关闭图片文件
    image_file.close()

    fdfs = FDFS("ihome/utils/fdfs/fdfs_client.conf")
    image_upload = fdfs.upload(image_path)
    if image_upload == "False":
        return jsonify(errno=RET.OK, errmsg="图片上传失败")

    # 保存image_url到数据库
    image_url = FASTdfs_URL + image_upload["Remote file_id"]
    house_image = HouseImage(house_id=house_id, url=image_url)
    # 处理房屋的主图片
    if not house.index_image_url:
        house.index_image_url = image_url
        db.session.add(house)
    db.session.add(house_image)
    try:
        db.session.commit()
    except Exception as e:
        db.session.rollback()
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库保存图片失败")
    return jsonify(errno=RET.OK, errmsg="图片保存成功", data={"image_url": image_url})
```

newhouse.js

```js
$("#form-house-image").submit(function (e) {
        e.preventDefault();
        $(this).ajaxSubmit({
            url: "/api/v1.0/houses/images",
            type: "post",
            dataType: "json",
            headers: {
                "X-CSRFToken": getCookie("csrf_token")
            },
            success: function (res) {
                if (res.errno == "4101"){
                    location.href = "/login.html";
                } else if (res.errno == "0") {
                    var image_url = res.data.image_url;
                    $(".house-image-cons").append('<img src="'+image_url+'">');
                } else {
                    alert(res.errmsg);
                }
            }
        })

    })
```

# 房东发布房源信息条目接口

```python
@api.route("/user/houses", methods=["GET"])
@login_required
def get_user_houses():
    """
    获取房东发布房源信息条目
    :return:
    """
    user_id = g.user_id

    try:
        user = User.query.get(user_id)
        houses = user.houses
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="获取数据失败")
    # 将查询到的房屋信息转换为字典存放到列表中
    houses_list = []
    if houses:
        for house in houses:
            houses_list.append(house.to_basic_dict())
    return jsonify(errno=RET.OK, errmag="OK", data={"houses": houses_list})


@api.route("/houses/index", methods=["GET"])
def get_house_index():
    """
    获取主页幻灯片展示的房屋基本信息
    :return:
    """
    try:
        ret = redis_link.get("home_page_data")
    except Exception as e:
        current_app.logger.error(e)
        ret = None
    if ret:
        current_app.logger.info("hit redis house_index_info")
        # 解码 Redis 中存储的数据
        decoded_data = json.loads(ret)
        return jsonify(errno=RET.OK, errmag="OK", data=decoded_data)

    else:
        try:
            # 查询数据库,返回房屋订单数目最多的5条数据
            houses = House.query.order_by(House.order_count.desc()).limit(HOME_PAGE_MAX_HOUSES)
        except Exception as e:
            current_app.logger.error(e)
            return jsonify(errno=RET.DBERR, errmsg="查询数据失败")

        if not houses:
            return jsonify(errno=RET.DBERR, errmsg="查询无数据")

        houses_list = []

        for house in houses:
            # 如果房屋未设置主图片,则跳过
            if not house.index_image_url:
                continue
            houses_list.append(house.to_basic_dict())
        # 将数据转换为json，并保存到redis缓存
        json_houses_list = json.dumps(houses_list)
        try:
            redis_link.setex("home_page_data", HOME_PAGE_DATA_REDIS_EXPIRES, json_houses_list)
        except Exception as e:
            current_app.logger.error(e)
        return jsonify(errno=RET.OK, errmag="OK", data=houses_list)
```

myhouse.js

```js
$(document).ready(function(){
    // $(".auth-warn").show();
    $.get("/api/v1.0/users/real_info",function (res) {
        if("4101"==res.errno){
            // 用户未登录
            location.href = "/login.html"
        }else if("4002" == res.errno){
            $(".auth-warn").show();
            return;
        }else if("0" == res.errno){
            // 认证用户，请求其之前发布的房源信息
            $.get("/api/v1.0/user/houses",function (res){
                if("0"==res.errno){
                    $("#houses-list").html(template("houses-list-tmp",{houses:res.data.houses}));
                }else {
                    $("#houses-list").html(template("houses-list-tmp",{houses:[]}));
                }
            })
        }
    })

})
```

# 获取房屋详情接口

```python
@api.route("/houses/<int:house_id>", methods=["GET"])
def get_house_detail(house_id):
    """
    获取房屋详情
    前端在房屋详情页面展示时，如果浏览页面的用户不是该房屋的房东，则展示预定按钮，否则不展示，
    所以需要后端返回登录用户的user_id

    :param house_id:
    :return:
    """
    # 尝试获取用户登录的信息，若登录，则返回给前端登录用户的user_id，否则返回user_id = -1
    user_id = session.get("user_id", "-1")

    # 校验数据
    if not house_id:
        return jsonify(errno=RET.PARAMERR, errmsg="参数确实")

    # redis获取数据
    try:
        ret = redis_link.get("house_info_%s" % house_id)
    except Exception as e:
        current_app.logger.error(e)
        ret = None
    if ret:
        current_app.logger.info("hit house_info redis")
        # 解码 Redis 中存储的数据
        decoded_data = json.loads(ret)
        data = {
            "house": decoded_data,
            "user_id": user_id
        }
        return jsonify(errno=RET.OK, errmag="OK", data=data)

    # 查询数据库
    try:
        house = House.query.get(house_id)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库查询失败")
    if not house:
        return jsonify(errno=RET.NODATA, errmsg="房屋不存在")
    try:
        house_data = house.to_full_dict()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库查询失败")
    json_house_data = json.dumps(house_data)
    try:
        redis_link.setex("house_info_%s" % house_id, HOUSE_DETAIL_REDIS_EXPIRE_SECOND, json_house_data)
    except Exception as e:
        current_app.looger.error(e)
    data = {
        "house": house_data,
        "user_id": user_id
    }
    return jsonify(errno=RET.OK, errmag="OK", data=data)
```

deteil.js

```js
function hrefBack() {
    history.go(-1);
}

// 解析提取url中的查询字符串参数
function decodeQuery() {
    var search = decodeURI(document.location.search);
    return search.replace(/(^\?)/, '').split('&').reduce(function (result, item) {
        values = item.split('=');
        result[values[0]] = values[1];
        return result;
    }, {});
}

$(document).ready(function () {
    // 获取详情页面要展示的房屋编号
    var queryData = decodeQuery();
    var houseId = queryData["id"]
    // 获取该房屋的详细信息
    $.get("/api/v1.0/houses/" + houseId, function (res) {
        if ("0" == res.errno) {
            $(".swiper-container").html(template("swiper-image-tmp", {img_urls: res.data.house.img_urls,price:res.data.house.price}))
            $(".detail-con").html(template("house-info-tmp", {house: res.data.house}))
            // res.user_id为访问页面用户，res.data.user_id为房东
            if (res.data.user_id != res.data.house.user_id) {
                $(".book-house").attr("href", "/booking.html?hid=" + res.data.house.hid);
                $(".book-house").show();
            }
            var mySwiper = new Swiper('.swiper-container', {
                loop: true,
                autoplay: 2000,
                autoplayDisableOnInteraction: false,
                pagination: '.swiper-pagination',
                paginationType: 'fraction'
            })
        }
    })
})
```



# map使用

```
li = [1, 2, 4, 5, 6]
li1 = [2, 4, 6, 8, 10]


def func(x, y):
    return x + y

def func1(x):
    return x + 1

data = map(func, li, li1)
print(list(data))


val = map(func1, li)
print(list(val))
```

# 邮件发送使用celery

问题

```python
Traceback (most recent call last):
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/celery/app/trace.py", line 477, in trace_task
    R = retval = fun(*args, **kwargs)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/celery/app/trace.py", line 760, in __protected_call__
    return self.run(*args, **kwargs)
  File "/home/feelapple/flask/flask-Ihome/ihome/tasks/task_email.py", line 34, in send_sms
    send_email(email, captcha)
  File "/home/feelapple/flask/flask-Ihome/ihome/tasks/task_email.py", line 20, in send_email
    current_app.logger.error('发送验证码失败', e)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/werkzeug/local.py", line 347, in __getattr__
    return getattr(self._get_current_object(), name)
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/werkzeug/local.py", line 306, in _get_current_object
    return self.__local()
  File "/home/feelapple/Virtual-Workspace/flask/lib/python3.10/site-packages/flask/globals.py", line 52, in _find_app
    raise RuntimeError(_app_ctx_err_msg)
RuntimeError: Working outside of application context.

This typically means that you attempted to use functionality that needed
to interface with the current application object in some way. To solve
this, set up an application context with app.app_context().  See the
documentation for more information.
```

verify_code.py

```
from ihome.tasks.task_email import send_sms

def get_email_code(email):
修改
	# 发送短信
    # 使用celery异步发送短信
    send_sms.delay(email, captcha)
```

task_email.py

```python
from celery import Celery
from ihome.utils.send_email import send_email

celery = Celery("ihome", broker="redis://127.0.0.1:6379/1")


@celery.task
def send_sms(email, captcha):
    """
    发送邮件的异步任务
    :return:
    """
    send_email(email, captcha)
```

send_email.py

```python
from flask import current_app, jsonify
from flask_mail import Message
from ihome import mail,create_app
from ihome.utils import constants


def send_email(accept_qq, captcha):
    # 解决问题
    # 创建Flask应用实例
    app = create_app("develop")
    # 设置应用程序上下文
    with app.app_context():
        try:
            # sender 发送方，recipients 接收方列表
            msg = Message("验证码", sender=constants.QQ, recipients=[accept_qq])  # '1329934054@qq.com'
            # 邮件内容
            msg.body = captcha
            # 发送邮件
            mail.send(msg)
            print("Mail sent")
            current_app.logger.info('已成功发送信息给qq:%s' % accept_qq)
            return 0
        except Exception as e:
            current_app.logger.error('发送验证码失败', e)
            return 1
```

```
这是一个运行Celery任务队列的命令。让我为你解释其中的各个部分：
- `celery`: 这是运行Celery命令的关键字。
- `-A ihome.tasks.task_email`: `-A`参数指定了Celery应用程序的模块路径。在这个例子中，它指定了`ihome.tasks.task_email`模块作为应用程序。
- `worker`: `worker`是Celery的一个命令，用于启动一个工作进程，负责执行任务队列中的任务。
- `-l info`: `-l`参数指定了日志级别，这里设置为`info`，表示输出信息级别的日志。
综上所述，该命令将启动一个Celery的工作进程，使用`ihome.tasks.task_email`模块作为应用程序，并输出信息级别的日志。此进程将执行任务队列中的任务，其中包括发送邮件的任务。
# 启动celery任务
celery -A ihome.tasks.task_email worker -l info
```

# 获取房东发布房源信息接口

```python
@api.route("/user/houses", methods=["GET"])
@login_required
def get_user_houses():
    """
    获取房东发布房源信息条目
    :return:
    """
    user_id = g.user_id

    try:
        user = User.query.get(user_id)
        houses = user.houses
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="获取数据失败")
    # 将查询到的房屋信息转换为字典存放到列表中
    houses_list = []
    if houses:
        for house in houses:
            houses_list.append(house.to_basic_dict())
    return jsonify(errno=RET.OK, errmag="OK", data={"houses": houses_list})
```

myhouse.html

```html
<ul id="houses-list" class="houses-list">
            </ul>
             <script type="text/html" id="houses-list-tmp">
                    <li>
                        <div class="new-house">
                            <a href="/newhouse.html">发布新房源</a>
                        </div>
                    </li>
                    {{ each houses as house }}
                        <li>
                            <a href="/detail.html?id={{house.house_id}}">
                                <div class="house-title">
                                    <h3>房屋ID:{{house.house_id}} —— {{house.title}}</h3>
                                </div>
                                <div class="house-content">
                                    <img src="{{house.img_url}}">
                                    <div class="house-text">
                                        <ul>
                                            <li>位于：{{house.area_name}}</li>
                                            <li>价格：￥{{(house.price/100.0).toFixed(0)}}/晚</li>
                                            <li>发布时间：{{house.ctime}}</li>
                                        </ul>
                                    </div>
                                </div>
                            </a>
                        </li>
                    {{ /each }}
             </script>
        </div>
```

myhouse.js

```js
$(document).ready(function(){
    // $(".auth-warn").show();
    $.get("/api/v1.0/users/real_info",function (res) {
        if("4101"==res.errno){
            // 用户未登录
            location.href = "/login.html"
        }else if("4002" == res.errno){
            $(".auth-warn").show();
            return;
        }else if("0" == res.errno){
            // 认证用户，请求其之前发布的房源信息
            $.get("/api/v1.0/user/houses",function (res){
                if("0"==res.errno){
                    $("#houses-list").html(template("houses-list-tmp",{houses:res.data.houses}));
                }else {
                    $("#houses-list").html(template("houses-list-tmp",{houses:[]}));
                }
            })
        }
    })
})
```

# 房屋列表页接口(搜索)

houses.py

```python
# GET /api/v1.0/houses?start_date=2023-03-11&end_date=2023-07-11&area_id=3&sort_key=new&page=1
@api.route("/houses", methods=["GET"])
def get_house_list():
    """
    获取房屋的列表信息(搜索页面)
    :return:
    """
    start_date = request.args.get("start_date")  # 租房的起始时间
    end_date = request.args.get("end_date")  # 租房的结束时间
    area_id = request.args.get("area_id")  # 区域编号
    sort_key = request.args.get("sort_key", "new")  # 排序关键字
    page = request.args.get("page")  # 当前页数

    # 处理时间
    try:
        if start_date:
            # 将字符串解析为对应的datetime对象
            start_date = datetime.strptime(start_date, "%Y-%m-%d")
        if end_date:
            end_date = datetime.strptime(end_date, "%Y-%m-%d")

        if start_date and end_date:
            assert start_date <= end_date
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.PARAMERR, errmsg="日期参数有误")

    # 判断区域id
    if area_id:
        try:
            area = Area.query.get(area_id)
        except Exception as e:
            current_app.logger.error(e)
            return jsonify(errno=RET.PARAMERR, errmsg="区域参数有误")

    # 处理页数
    try:
        page = int(page)
    except Exception as e:
        current_app.logger.error(e)
        page = 1

    # 过滤参数的列表容器
    filter_params = []

    # 填充过滤参数
    # 时间条件
    conflict_orders = None
    try:
        if start_date and end_date:
            # 查询冲突房子
            # select * from order where order.begin_date <= end_date and order.end_date >= start_date
            conflict_orders = Order.query.filter(Order.begin_date <= end_date, Order.end_date >= start_date).all()
        elif start_date:
            conflict_orders = Order.query.filter(Order.end_date >= start_date).all()
        elif end_date:
            conflict_orders = Order.query.filter(Order.begin_date <= end_date).all()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库异常")
    if conflict_orders:
        # 提取订单冲突的房屋id
        conflict_order_ids = [conflict_order.house_id for conflict_order in conflict_orders]
        # 如果冲突的房屋id不为空，向查询参数中添加条件
        if conflict_order_ids:
            filter_params.append(House.id.notin_(conflict_order_ids))

    # 区域条件查询
    if area_id:
        filter_params.append(House.area_id == area_id)

    # 查询数据库
    # 补充排序条件
    if sort_key == "booking":
        house_query = House.query.filter(*filter_params).order_by(House.order_count.desc())
    elif sort_key == "price-inc":
        house_query = House.query.filter(*filter_params).order_by(House.price.asc())
    elif sort_key == "price-des":
        house_query = House.query.filter(*filter_params).order_by(House.price.desc())
    else:
        house_query = House.query.filter(*filter_params).order_by(House.create_time.desc())
    try:
        # 处理分页                          当前页数          每页数据量                      错误输出
        page_obj = house_query.paginate(page=page, per_page=HOUSE_LIST_PAGE_CAPACITY, error_out=False)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库异常")
    # 获取页面数据
    houses_li = page_obj.items
    houses = []
    for house in houses_li:
        houses.append(house.to_basic_dict())
    # 获取总页数
    total_pages = page_obj.pages
    return jsonify(errno=RET.OK, errmag="OK", data={"total_pages": total_pages, "houses": houses, "current_page": page})
```

## sqlalchemy 双等号参数说明

```
filter_params.append(House.area_id == area_id) # 列表条件
>>> li = []
>>> a = 1
>>> li.append(a == 1)
>>> li
[True]
在这个例子中，li是一个空列表。在第一个例子中，我们通过li.append(a == 1)将一个布尔表达式a == 1添加到了列表中。由于a的值为1，并与常量1进行比较，所以结果为True。因此，True被添加到了li列表中。

>>> from ihome.models import *
>>> li = []
>>> li.append(House.area_id == 1)
>>> li
[<sqlalchemy.sql.elements.BinaryExpression object at 0x7fe8c2a6d120>]
>>> House.area_id.__eq__(1)
<sqlalchemy.sql.elements.BinaryExpression object at 0x7fe8c2a6d7b0>


在第二个例子中，我们导入了ihome.models模块，并将一个BinaryExpression对象House.area_id == 1添加到了列表li中。这个对象表示了一个用于查询的条件表达式，即House表的area_id列等于1的条件。这是SQLAlchemy库对比较操作符进行了重写，以生成对应的条件表达式对象。
```

## 添加redis 

```
# 数据存储设计
# redis_link
# "house_起始结束_区域id_排序_页数"
# errno=RET.OK, errmag="OK", data={"total_pages": total_pages, "houses": houses, "current_page": page}

# " house_起始_结束_区域id_排序": hash
# {
#     "1":"{}",
#     "2":"{}"
# }
```

```python
# GET /api/v1.0/houses?start_date=2023-03-11&end_date=2023-07-11&area_id=3&sort_key=new&page=1
@api.route("/houses", methods=["GET"])
def get_house_list():
    """
    获取房屋的列表信息(搜索页面)
    :return:
    """
    start_date = request.args.get("start_date", "")  # 租房的起始时间
    end_date = request.args.get("end_date", "")  # 租房的结束时间
    area_id = request.args.get("area_id", "")  # 区域编号
    sort_key = request.args.get("sort_key", "new")  # 排序关键字
    page = request.args.get("page")  # 当前页数

    # 处理时间
    try:
        if start_date:
            # 将字符串解析为对应的datetime对象
            start_date = datetime.strptime(start_date, "%Y-%m-%d")
        if end_date:
            end_date = datetime.strptime(end_date, "%Y-%m-%d")

        if start_date and end_date:
            assert start_date <= end_date
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.PARAMERR, errmsg="日期参数有误")

    # 判断区域id
    if area_id:
        try:
            area = Area.query.get(area_id)
        except Exception as e:
            current_app.logger.error(e)
            return jsonify(errno=RET.PARAMERR, errmsg="区域参数有误")

    # 处理页数
    try:
        page = int(page)
    except Exception as e:
        current_app.logger.error(e)
        page = 1

    # 使用redis拿取数据
    redis_key = "house_%s_%s_%s_%s" % (start_date, end_date, area_id, sort_key)
    try:
        res_json = redis_link.hget(redis_key,page)
    except Exception as e:
        current_app.logger.error(e)
    else:
        if res_json:
            decoded_data = json.loads(res_json)
            return jsonify(errno=RET.OK, errmag="OK", data=decoded_data)
        
    # 过滤参数的列表容器
    filter_params = []

    # 填充过滤参数
    # 时间条件
    conflict_orders = None
    try:
        if start_date and end_date:
            # 查询冲突房子
            # select * from order where order.begin_date <= end_date and order.end_date >= start_date
            conflict_orders = Order.query.filter(Order.begin_date <= end_date, Order.end_date >= start_date).all()
        elif start_date:
            conflict_orders = Order.query.filter(Order.end_date >= start_date).all()
        elif end_date:
            conflict_orders = Order.query.filter(Order.begin_date <= end_date).all()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库异常")
    if conflict_orders:
        # 提取订单冲突的房屋id
        conflict_order_ids = [conflict_order.house_id for conflict_order in conflict_orders]
        # 如果冲突的房屋id不为空，向查询参数中添加条件
        if conflict_order_ids:
            filter_params.append(House.id.notin_(conflict_order_ids))

    # 区域条件查询
    if area_id:
        filter_params.append(House.area_id == area_id)

    # 查询数据库
    # 补充排序条件
    if sort_key == "booking":
        house_query = House.query.filter(*filter_params).order_by(House.order_count.desc())
    elif sort_key == "price-inc":
        house_query = House.query.filter(*filter_params).order_by(House.price.asc())
    elif sort_key == "price-des":
        house_query = House.query.filter(*filter_params).order_by(House.price.desc())
    else:
        house_query = House.query.filter(*filter_params).order_by(House.create_time.desc())
    try:
        # 处理分页                          当前页数          每页数据量                      错误输出
        page_obj = house_query.paginate(page=page, per_page=HOUSE_LIST_PAGE_CAPACITY, error_out=False)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库异常")
    # 获取页面数据
    houses_li = page_obj.items
    houses = []
    for house in houses_li:
        houses.append(house.to_basic_dict())
    # 获取总页数
    total_pages = page_obj.pages
    res_dict = dict(errno=RET.OK, errmag="OK", data={"total_pages": total_pages, "houses": houses, "current_page": page})
    res_json = json.dumps(res_dict)

    # 设置缓存数据
    if page <= total_pages:
        redis_key = "house_%s_%s_%s_%s" % (start_date,end_date,area_id,sort_key)
        # 哈希
        try:
            #redis_link.hset(redis_key, page, res_json)
            # redis设置有效期
            #redis_link.expire(redis_key, HOUES_LIST_PAGE_REDIS_CACHE_EXPIRES)
            pipeline = redis_link.pipeline()
            pipeline.multi()
            pipeline.hset(redis_key, page, res_json)
            pipeline.expire(redis_key, HOUES_LIST_PAGE_REDIS_CACHE_EXPIRES)
            # 执行语句
            pipeline.execute()
        except Exception as e:
            current_app.logger.error(e)
            return jsonify(errno=RET.DBERR, errmsg="数据库异常")

    return jsonify(errno=RET.OK, errmag="OK", data={"total_pages": total_pages, "houses": houses, "current_page": page})
```

# 保存订单接口

```python
@api.route("/orders", methods=["POST"])
@login_required
def save_order():
    """
    保存订单
    :return:
    """
    user_id = g.user_id

    # 获取参数
    order_data = request.get_json()
    if not order_data:
        return jsonify(errno=RET.PARAMERR, errmsg='参数错误')

    house_id = order_data.get("house_id")
    start_date_str = order_data.get("start_date")
    end_date_str = order_data.get("end_date")
    print(house_id, start_date_str, end_date_str)
    # 参数检查
    if not all((house_id, start_date_str, end_date_str)):
        return jsonify(errno=RET.PARAMERR, errmsg='参数错误')

    # 日期格式检查
    try:
        start_date = datetime.datetime.strptime(start_date_str, "%Y-%m-%d")
        end_date = datetime.datetime.strptime(end_date_str, "%Y-%m-%d")
        assert start_date <= end_date
        # 计算预定的天数
        days = (end_date - start_date).days + 1  # datetime.timedelta
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.PARAMERR, errmsg="日期格式错误")

    # 查询房屋是否存在
    try:
        house = House.query.get(house_id)
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="获取房屋信息失败")
    if not house:
        return jsonify(errno=RET.NODATA, errmsg="房屋不存在")

    # 预订的房屋是否是房东自己的
    if user_id == house.user_id:
        return jsonify(errno=RET.ROLEERR, errmsg="不能预订自己的房屋")

    # 确保用户预订的时间内，房屋没有被别人下单
    try:
        count = Order.query.filter(Order.house_id == house_id, Order.begin_date <= end_date,
                                   Order.end_date >= start_date).count()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="检查出错，请稍候重试")
    if count > 0:
        return jsonify(errno=RET.DATAERR, errmsg="房屋已被预订")

    # 订单总额
    amount = days * house.price

    # 保存订单数据
    order = Order(
        house_id=house_id,
        user_id=user_id,
        begin_date=start_date,
        end_date=end_date,
        days=days,
        house_price=house.price,
        amount=amount
    )
    try:
        db.session.add(order)
        db.session.commit()
    except Exception as e:
        current_app.logger.error(e)
        db.session.rollback()
        return jsonify(errno=RET.DBERR, errmsg="保存订单失败")
    return jsonify(errno=RET.OK, errmsg="OK", data={"order_id": order.id})
```

booking.js

```python
function hrefBack() {
    history.go(-1);
}

function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}

function decodeQuery(){
    var search = decodeURI(document.location.search);
    return search.replace(/(^\?)/, '').split('&').reduce(function(result, item){
        values = item.split('=');
        result[values[0]] = values[1];
        return result;
    }, {});
}

function showErrorMsg() {
    $('.popup_con').fadeIn('fast', function() {
        setTimeout(function(){
            $('.popup_con').fadeOut('fast',function(){}); 
        },1000) 
    });
}

$(document).ready(function(){
    // 判断用户是否登录
    $.get("/api/v1.0/sessions", function(res) {
        if ("0" != res.errno) {
            location.href = "/login.html";
        }
    }, "json");
    $(".input-daterange").datepicker({
        format: "yyyy-mm-dd",
        startDate: "today",
        language: "zh-CN",
        autoclose: true
    });
    $(".input-daterange").on("changeDate", function(){
        var startDate = $("#start-date").val();
        var endDate = $("#end-date").val();

        if (startDate && endDate && startDate > endDate) {
            showErrorMsg();
        } else {
            var sd = new Date(startDate);
            var ed = new Date(endDate);
            days = (ed - sd)/(1000*3600*24) + 1;
            var price = $(".house-text>p>span").html();
            var amount = days * parseFloat(price);
            $(".order-amount>span").html(amount.toFixed(2) + "(共"+ days +"晚)");
        }
    });
    var queryData = decodeQuery();
    var houseId = queryData["hid"];
    $.get("/api/v1.0/houses/" + houseId, function(res) {
        if (0 == res.errno) {
            $(".house-info>img").attr("src", res.data.house.img_urls[0]);
            $(".house-text>h3").html(res.data.house.title);
            $(".house-text>p>span").html((res.data.house.price/100.0).toFixed(0));
        }
    })
    // 订单提交
    $(".submit-btn").on("click",function (e){
        if ($(".order-amount>span").html()) {
            $(this).prop("disabled", true);
            var startDate = $("#start-date").val();
            var endDate = $("#end-date").val();
            var data = {
                "house_id":houseId,
                "start_date":startDate,
                "end_date":endDate
            };
            console.log(data)
            $.ajax({
                url:"/api/v1.0/orders",
                type:"POST",
                data: JSON.stringify(data),
                contentType: "application/json",
                dataType: "json",
                headers: {
                  "X-CSRFToken":  getCookie("csrf_token")
                },
                success: function (res) {
                    if ("4101" == res.errno) {
                        location.href = "/login.html";
                    } else if ("4004" == res.errno) {
                        showErrorMsg("房间已被抢定，请重新选择日期！");
                    } else if ("0" == res.errno) {
                        location.href = "/orders.html";
                    }
                }
            });
        }
    })

})
```

# 查询用户的订单信息接口

```python
# /api/v1.0/user/orders?role=custom     role=landlord
@api.route("/user/orders", methods=["GET"])
@login_required
def get_user_orders():
    """
    查询用户的订单信息
    :return:
    """
    user_id = g.user_id

    # 用户的身份，用户想要查询作为房客预订别人房子的订单，还是想要作为房东查询别人预订自己房子的订单
    role = request.args.get("role", "")

    # 查询订单数据
    try:
        if "landlord" == role:
            # 以房东的身份查询订单
            # 先查询属于自己的房子有哪些
            houses = House.query.filter(House.user_id == user_id).all()
            houses_ids = [house.id for house in houses]
            # 再查询所有预订了自己房子的订单
            orders = Order.query.filter(Order.house_id.in_(houses_ids)).order_by(Order.create_time.desc()).all()
        else:
            # 以房客的身份查询订单， 查询自己预订的订单
            orders = Order.query.filter(Order.user_id == user_id).order_by(Order.create_time.desc()).all()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="查询订单信息失败")

    # 将订单对象转换为字典数据
    orders_dict_list = []
    if orders:
        for order in orders:
            orders_dict_list.append(order.to_dict())

    return jsonify(errno=RET.OK, errmsg="OK", data={"orders": orders_dict_list})
```

orders.js

```js
//模态框居中的控制
function centerModals(){
    $('.modal').each(function(i){   //遍历每一个模态框
        var $clone = $(this).clone().css('display', 'block').appendTo('body');    
        var top = Math.round(($clone.height() - $clone.find('.modal-content').height()) / 2);
        top = top > 0 ? top : 0;
        $clone.remove();
        $(this).find('.modal-content').css("margin-top", top-30);  //修正原先已经有的30个像素
    });
}

function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}

$(document).ready(function(){
    $('.modal').on('show.bs.modal', centerModals);      //当模态框出现的时候
    $(window).on('resize', centerModals);
    // 查询房客订单
    $.get("/api/v1.0/user/orders?role=custom", function(res){
        if ("0" == res.errno) {
            $(".orders-list").html(template("orders-list-tmp", {orders:res.data.orders}));
            $(".order-pay").on("click", function () {
                var orderId = $(this).parents("li").attr("order-id");
                $.ajax({
                    url: "/api/v1.0/orders/" + orderId + "/payment",
                    type: "post",
                    dataType: "json",
                    headers: {
                        "X-CSRFToken": getCookie("csrf_token"),
                    },
                    success: function (res) {
                        if ("4101" == res.errno) {
                            location.href = "/login.html";
                        } else if ("0" == res.errno) {
                            // 引导用户跳转到支付宝连接
                            location.href = res.data.pay_url;
                        }
                    }
                });
            });
            $(".order-comment").on("click", function(){
                var orderId = $(this).parents("li").attr("order-id");
                $(".modal-comment").attr("order-id", orderId);
            });
            $(".modal-comment").on("click", function(){
                var orderId = $(this).attr("order-id");
                var comment = $("#comment").val()
                if (!comment) return;
                var data = {
                    order_id:orderId,
                    comment:comment
                };
                // 处理评论
                $.ajax({
                    url:"/api/v1.0/orders/"+orderId+"/comment",
                    type:"PUT",
                    data:JSON.stringify(data),
                    contentType:"application/json",
                    dataType:"json",
                    headers:{
                        "X-CSRFToken":getCookie("csrf_token"),
                    },
                    success:function (res) {
                        if ("4101" == res.errno) {
                            location.href = "/login.html";
                        } else if ("0" == res.errno) {
                            $(".orders-list>li[order-id="+ orderId +"]>div.order-content>div.order-text>ul li:eq(4)>span").html("已完成");
                            $("ul.orders-list>li[order-id="+ orderId +"]>div.order-title>div.order-operate").hide();
                            $("#comment-modal").modal("hide");
                        }
                    }
                });
            });
        }
    });
});
```

接单、拒单接口

```python
@api.route("/orders/<int:order_id>/status", methods=["PUT"])
@login_required
def accept_reject_order(order_id):
    """
    接单、拒单
    :param order_id:
    :return:
    """
    user_id = g.user_id

    # 获取参数
    req_data = request.get_json()
    if not req_data:
        return jsonify(errno=RET.PARAMERR, errmsg="参数错误")

    # action参数表明客户端请求的是接单还是拒单的行为
    action = req_data.get("action")
    if action not in ("accept", "reject"):
        return jsonify(errno=RET.PARAMERR, errmsg="参数错误")

    try:
        # 根据订单号查询订单，并且要求订单处于等待接单状态
        order = Order.query.filter(Order.id == order_id, Order.status == "WAIT_ACCEPT").first()
        house = order.house
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="无法获取订单数据")

    # 确保房东只能修改属于自己房子的订单
    if not order or house.user_id != user_id:
        return jsonify(errno=RET.REQERR, errmsg="操作无效")

    if action == "accept":
        # 接单，将订单状态设置为等待评论
        order.status = "WAIT_PAYMENT"
    elif action == "reject":
        # 拒单，要求用户传递拒单原因
        reason = req_data.get("reason")
        if not reason:
            return jsonify(errno=RET.PARAMERR, errmsg="参数错误")
        order.status = "REJECTED"
        order.comment = reason

    try:
        db.session.add(order)
        db.session.commit()
    except Exception as e:
        current_app.logger.error(e)
        db.session.rollback()
        return jsonify(errno=RET.DBERR, errmsg="操作失败")

    return jsonify(errno=RET.OK, errmsg="OK")
```

保存订单评论信息接口

```python
@api.route("/orders/<int:order_id>/comment", methods=["PUT"])
@login_required
def save_order_comment(order_id):
    """
    保存订单评论信息
    :param order_id:
    :return:
    """
    user_id = g.user_id
    # 获取参数
    req_data = request.get_json()
    comment = req_data.get("comment")  # 评价信息

    # 检查参数
    if not comment:
        return jsonify(errno=RET.PARAMERR, errmsg="参数错误")

    try:
        # 需要确保只能评论自己下的订单，而且订单处于待评价状态才可以
        order = Order.query.filter(Order.id == order_id, Order.user_id == user_id,
                                   Order.status == "WAIT_COMMENT").first()
        house = order.house
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="无法获取订单数据")

    if not order:
        return jsonify(errno=RET.REQERR, errmsg="操作无效")

    try:
        # 将订单的状态设置为已完成
        order.status = "COMPLETE"
        # 保存订单的评价信息
        order.comment = comment
        # 将房屋的完成订单数增加1
        house.order_count += 1
        db.session.add(order)
        db.session.add(house)
        db.session.commit()
    except Exception as e:
        current_app.logger.error(e)
        db.session.rollback()
        return jsonify(errno=RET.DBERR, errmsg="操作失败")

    # 因为房屋详情中有订单的评价信息，为了让最新的评价信息展示在房屋详情中，所以删除redis中关于本订单房屋的详情缓存
    try:
        redis_link.delete("house_info_%s" % order.house.id)
    except Exception as e:
        current_app.logger.error(e)

    return jsonify(errno=RET.OK, errmsg="OK")
```

# 订单支付接口

pay.py

```python
import os

from flask import g, current_app, jsonify, request
from alipay import AliPay

from . import api
from ihome.utils.commons import login_required
from ihome.models import Order
from ihome.utils.response_code import RET
from ihome.utils import constants
from ihome import db


@api.route("/orders/<int:order_id>/payment", methods=["POST"])
@login_required
def order_pay(order_id):
    """
    发起支付宝支付
    :return:
    """
    user_id = g.user_id

    # 判断订单状态
    try:
        order = Order.query.filter(Order.id == order_id, Order.user_id == user_id,
                                   Order.status == "WAIT_PAYMENT").first()
    except Exception as e:
        current_app.logger.error(e)
        return jsonify(errno=RET.DBERR, errmsg="数据库异常")

    if order is None:
        return jsonify(errno=RET.NODATA, errmsg="无此订单数据")
    app_private_key_string = open(os.path.join(os.path.dirname(__file__), "keys/app_private_key.pem")).read()
    alipay_public_key_string = open(os.path.join(os.path.dirname(__file__), "keys/alipay_public_key.pem")).read()
    # 创建支付宝sdk工具对象
    alipay = AliPay(
        appid="2021000122630104",
        app_notify_url=None,
        app_private_key_string=app_private_key_string,
        alipay_public_key_string=alipay_public_key_string,
        sign_type="RSA2",
        debug=True
    )

    # 手机网站支付，需要跳转到https://openapi.alipaydev.com/gateway.do? + order_string
    order_string = alipay.api_alipay_trade_wap_pay(
        out_trade_no=order_id,  # 订单编号
        total_amount=str(order.amount / 100.0),  # 总金额
        subject="爱家租房 %s" % order_id,  # 订单标题
        quit_url="http://dwz.feelapple.cn:8080/payComplete.html",
        return_url="http://dwz.feelapple.cn:8080/payComplete.html",
        notify_url=None
    )

    # 构建让用户跳转的支付连接地址
    pay_url = constants.ALIPAY_URL_PREFIX + order_string
    return jsonify(errno=RET.OK, errmsg="OK", data={"pay_url": pay_url})
```

# 保存订单支付结果

```python
@api.route("/orders/payment", methods=["PUT"])
def save_order_payment_result():
    """
    保存订单支付结果
    :return:
    """
    alipay_data = request.form.to_dict()
    # 支付宝数据进行数据分离   提取出支付宝的签名参数sign 和剩下的其他数据
    alipay_sign = alipay_data.pop("sign")

    # 创建支付宝sdk工具对象
    app_private_key_string = open(os.path.join(os.path.dirname(__file__), "keys/app_private_key.pem")).read()
    alipay_public_key_string = open(os.path.join(os.path.dirname(__file__), "keys/alipay_public_key.pem")).read()
    alipay = AliPay(
        appid="2021000122630104",
        app_notify_url=None,
        app_private_key_string=app_private_key_string,
        alipay_public_key_string=alipay_public_key_string,
        sign_type="RSA2",
        debug=True
    )

    # 借助工具验证参数的合法性
    # 如果确定参数是支付宝的, 返回True，否则返回false
    result = alipay.verify(alipay_data, alipay_sign)

    if result:
        order_id = alipay_data.get("out_trade_no")
        try:
            Order.query.filter_by(id=order_id).update(
                {
                    "status": "WAIT_COMMENT",
                    "trade_no": alipay_data.get("trade_no")
                }
            )
            db.session.commit()
        except Exception as e:
            current_app.logger.error(e)
            db.session.rollback()

    return jsonify(errno=RET.OK, errmsg="OK")
```

payComplete.html

```python
<!DOCTYPE html>
<html>
<head> 
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
    <title>爱家-支付成功</title>
    <link href="/static/plugins/bootstrap/css/bootstrap.min.css" rel="stylesheet">
    <link href="/static/plugins/font-awesome/css/font-awesome.min.css" rel="stylesheet">
    <link href="/static/css/reset.css" rel="stylesheet">
    <link href="/static/plugins/bootstrap-datepicker/css/bootstrap-datepicker.min.css" rel="stylesheet">
    <link href="/static/css/ihome/main.css" rel="stylesheet">
    <link href="/static/css/ihome/payComplete.css" rel="stylesheet">
</head>
<body>
    <div class="container">
        <div class="top-bar">
            <div class="nav-bar">
                <h3 class="page-title">支付完成</h3>
                <a class="nav-btn fl" href="/my.html"><span><i class="fa fa-angle-left fa-2x"></i></span></a>
            </div>
        </div>
        <div class="pay-con">
            <p>支付已完成</p>
            <a href="/orders.html">回到我的订单</a>
        </div>
        <div class="footer">
            <p><span><i class="fa fa-copyright"></i></span>爱家租房&nbsp;&nbsp;享受家的温馨</p>
        </div> 
    </div>
    
    <script src="/static/js/jquery.min.js"></script>
    <script src="/static/plugins/bootstrap/js/bootstrap.min.js"></script>
    <script src="/static/plugins/bootstrap-datepicker/js/bootstrap-datepicker.min.js"></script>
    <script src="/static/plugins/bootstrap-datepicker/locales/bootstrap-datepicker.zh-CN.min.js"></script>
    <script src="/static/js/template.js"></script>
    <script src="/static/js/ihome/orders.js"></script>
    <script type="text/javascript">
        function getCookie(name) {
            var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
            return r ? r[1] : undefined;
        }
        // 解析提取url中的查询字符串参数
        var alipayData = document.location.search.substring(1);
        $.ajax({
            url:"/api/v1.0/orders/payment",
            type:"PUT",
            data: alipayData,
            headers:{
                "X-CSRFToken":getCookie("csrf_token"),
            }

        })

    </script>
</body>
</html>
```


### 围绕二房东管理出租房、交房、交租、租客入住、收租、退租、退房的场景
- 耗时一个月的下班时间
- 前端采用小程序原生组件和weui-miniprogram扩展组件
- 后端采用微信云开发
    - 数据库：一个既可在小程序前端操作，也能在云函数中读写的 JSON 文档型数据库
    - 文件存储：在小程序前端直接上传/下载云端文件，在云开发控制台可视化管理
    - 云函数：在云端运行的代码，微信私有协议天然鉴权，开发者只需编写业务逻辑代码



- [ ] 以后将完善更多资料，路过的小伙伴帮忙点个star吧
#### [体验前请仔细阅读使用手册](https://docs.qq.com/doc/DSWtVT0F2UE15eEJU)
https://docs.qq.com/doc/DSWtVT0F2UE15eEJU

### 请扫码体验该小程序

![微信图片_20211016190016.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f899e7993a5473badb9443ecba58446~tplv-k3u1fbpfcp-watermark.image?)
### 数据库设计
####  房屋表（house）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  alia_name  |   string  | 是   |  小蜜公寓37-2403 | 自动拼接生成的房屋别名:如果是整租，则拼接name+floor;如果是单间，则拼接name+floor+room   |
|  desc  |   string  | 否   |  设施齐全  | 房屋的描述|
|  ele_price  |   number  | 否   |  1  | 每度电费的价格|
|  ele_scale  |   number  | 否   |  0  | 电表读数|
|  end_date  |   date  | 否   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间)  | 交租到期日|
|  fix_price  |   number  | 否   | 1 | 固定费用/月|
|  floor  |   string  | 是   | 2403 | 房号|
|  gas_price  |   number  | 否   | 1 | 每立方燃气的价格|
|  gas_scale |   number  | 否   | 0 | 燃气表读数|
|  house_owner_no |   string  | 否   | 18ed0968616a6b040044bbc0239c660d | 房主的_id|
|  images |   array  | 否   | ['cloud://a.png'] | 房屋图片url|
|  label_ids |   array  | 否   | ['cd045e'] | 房屋标签_id|
|  mid_resource |   boolean  | 是   | true | 二房东是否代缴水电燃费|
|  name |   string  | 是   | 小蜜公寓37 | 房屋名称，由小区+楼号构成|
|  ple_price |   number  | 是   | 2000 | 押金|
|  rent_date |   date  | 否   | Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 租客入住日期|
|  rent_deadline |   date  | 否   | Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 租金到期日|
|  rent_price |   number  | 是   | 2000| 租金/月|
|  rent_status |   string  | 是   | use| use:已出租；no_use:未出租|
|  rent_type |   string  | 是   | all| all:整租；part:单间|
|  room |   string  | 是   | 三室一厅一卫| 房间描述|
|  start_date |   date  | 否   | Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 交房日期|
|  tenant_no |   string  | 否   | 18ed0968616a6b040044bbc0239c660d | 租客的_id|
|  water_price  |   number  | 否   | 1 | 每吨水的价格|
|  water_scale |   number  | 否   | 0 | 水表读数|
|  wy_price |   number  | 是   | 1 | 物业费/月|

####  房主表（house_owner）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  phone_no |   string  | 是   | 15388888888 | 手机号|
|  real_name |   string  | 是   | 小蜜 | 真实姓名|
|  house_no |   string  | 是   | 0 | 房屋_id|
|  card_images |   array  | 否   | ['cloud://a.png'] | 身份证图片url|
|  pact_images |   array  | 否   | ['cloud://a.png'] | 合同图片url|
|  card_no |   string  | 是   | 320724 | 身份证号码|
|  desc |   string  | 否   | 男 | 房主描述|
|  start_date  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 交房日期   |

####  租客表（tenant）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  phone_no |   string  | 是   | 15388888888 | 手机号|
|  real_name |   string  | 是   | 小蜜 | 真实姓名|
|  house_no |   string  | 是   | 0 | 房屋_id|
|  card_images |   array  | 否   | ['cloud://a.png'] | 身份证图片url|
|  pact_images |   array  | 否   | ['cloud://a.png'] | 合同图片url|
|  card_no |   string  | 是   | 320724 | 身份证号码|
|  desc |   string  | 否   | 男 | 租客描述|
|  rent_date  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 入住日期   |

####  标签表（label）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  label  |   string  | 是   | 有电梯  | 标签描述   |
|  type  |   string  | 是   | system  | system:系统标签;self:自定义标签   |

####  二房东表（mid_owner）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  phone_no |   string  | 是   | 15388888888 | 手机号|
|  real_name |   string  | 是   | 小蜜 | 真实姓名|
|  use_deadline  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 使用到期日   |

####  交租表（mid_owner_payment）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  desc  |   string  | 否   |  顺利 | 交租描述   |
|  end_date  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 交租到期日   |
|  house_no |   string  | 是   | 0 | 房屋_id|
|  house_owner_no |   string  | 是   | 0 | 房主_id|
|  images |   array  | 否   | ['cloud://a.png'] | 交租图片url|
|  origin_end_date  |   date  | 否   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 原交租到期日   |
|  other_price_count  |   number  | 否   |  1 | 实付其他费用   |
|  pay_way  |   string  | 否   |  微信 | 支付方式   |
|  ple_price_count  |   number  | 否   |  1 | 实付押金   |
|  price_count  |   number  | 是   |  1 | 实付总计   |
|  rent_month_count  |   number  | 是   |  1 | 交租月数   |
|  rent_price_count  |   number  | 是   |  1 | 实付租金   |
|  wy_price_count  |   number  | 是   |  1 | 实付物业费   |

####  收租表（tenant_payment）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  confirm_time  |   date  | 否   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 确认账单时间   |
|  desc  |   string  | 否   |  顺利 | 收租描述   |
|  water_price  |   number  | 否   |  1 | 收租时水费单价   |
|  water_price_count  |   number  | 否   |  1 | 应收水费   |
|  water_scale  |   number  | 否   |  1 | 收租时水表读数   |
|  ele_price  |   number  | 否   |  1 | 收租时电费单价   |
|  ele_price_count  |   number  | 否   |  1 | 应收电费   |
|  ele_scale  |   number  | 否   |  1 | 收租时电表读数   |
|  fix_price  |   number  | 是   |  1 | 固定费用/月   |
|  fix_price_count  |   number  | 是   |  1 | 应收固定费用   |
|  gas_price  |   number  | 否   |  1 | 收租时燃气费单价   |
|  gas_price_count  |   number  | 否   |  1 | 应收燃气费   |
|  gas_scale  |   number  | 否   |  1 | 收租时燃气表读数   |
|  house_no |   string  | 是   | 0 | 房屋_id|
|  tenant_no |   string  | 是   | 0 | 租客_id|
|  images |   array  | 否   | ['cloud://a.png'] | 收款图片url|
|  mid_resource |   boolean  | 是   | true | 收租时的房屋是否代缴水电燃费用 |
|  origin_ele_scale  |   number  | 否   |  1 | 原电表读数   |
|  origin_gas_scale  |   number  | 否   |  1 | 原燃气表读数   |
|  origin_water_scale  |   number  | 否   |  1 | 原水表读数   |
|  origin_rent_deadline  |   date  | 否   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 原收租到期日   |
|  other_price_count  |   number  | 否   |  0 | 应收其他费用   |
|  pay_way  |   string  | 否   |  微信 | 支付方式   |
|  ple_price_count  |   number  | 否   |  1 | 应收押金   |
|  price_count  |   number  | 是   |  1 | 应收总计   |
|  rent_deadline  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 收租到期日   |
|  rent_month_count  |   number  | 是   |  1 | 收租月数   |
|  rent_price_count  |   number  | 是   |  1 | 应收租金   |
|  pay_rent_count  |   number  | 是   |  1 | 实收租金   |
|  rest_pay_rent_count  |   number  | 是   |  1 | 待收租金   |
|  real_pay_count  |   number  | 是   |  1 | 实收总计   |
|  wy_price  |   number  | 是   |  1 | 收租时物业费/月   |
|  wy_price_count  |   number  | 是   |  1 | 应收物业费   |

####  退租表（back_tenant_payment）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  desc  |   string  | 否   |  顺利 | 退租描述   |
|  back_price_count  |   number  | 是   |  1 | 退回金额总计   |
|  ele_price  |   number  | 否   |  1 | 退租时电费单价   |
|  ele_price_count  |   number  | 否   |  1 | 补收电费   |
|  ele_scale  |   number  | 否   |  1 | 退租时电表读数   |
|  gas_price  |   number  | 否   |  1 | 退租时燃气费单价   |
|  gas_price_count  |   number  | 否   |  1 | 补收燃气费   |
|  gas_scale  |   number  | 否   |  1 | 退租时燃气表读数   |
|  house_no |   string  | 是   | 0 | 房屋_id|
|  images |   array  | 否   | ['cloud://a.png'] | 退租图片url|
|  mid_resource |   boolean  | 是   | true | 退租时的房屋是否代缴水电燃费用 |
|  origin_ele_scale  |   number  | 否   |  1 | 原电表读数   |
|  origin_gas_scale  |   number  | 否   |  1 | 原燃气表读数   |
|  origin_water_scale  |   number  | 否   |  1 | 原水表读数   |
|  pay_way  |   string  | 否   |  微信 | 支付方式   |
|  ple_price_count  |   number  | 是   |  1 | 退回押金   |
|  price_count  |   number  | 是   |  1 | 补收金额总计   |
|  rent_deadline  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 退租日期   |
|  rent_price_count  |   number  | 是   |  1 | 退回租金   |
|  tenant  |   object  | 是   | {} | 退租时租客的信息   |
|  tenant_no  |   string  | 是   |  1 | 租客_id   |
|  water_price  |   number  | 否   |  1 | 退租时水费单价   |
|  water_price_count  |   number  | 否   |  1 | 补收水费   |
|  water_scale  |   number  | 否   |  1 | 退租时水表读数   |

####  退房表（back_mid_owner_payment）
| 字段名 | 类型 |是否必选|示例值|描述
| --- | --- |--- |--- |--- |
|  _id  |   string  | 是   | 18ed0968616a6b040044bbc0239c660d  | id索引   |
|  _openid  |   string  | 是   |  az_Au | 微信用户的_openid   |
|  create_time  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 创建时间   |
|  desc  |   string  | 否   |  顺利 | 退房描述   |
|  back_price_count  |   number  | 是   |  1 | 退回金额总计   |
|  end_date  |   date  | 是   |  Sat Oct 16 2021 14:02:44 GMT+0800 (中国标准时间) | 退房日期   |
|  house  |   object  | 是   |  {} | 退房时房屋的信息   |
|  house_no |   string  | 是   | 0 | 房屋_id|
|  house_owner  |   object  | 是   |  {} | 退房时房主的信息   |
|  images |   array  | 否   | ['cloud://a.png'] | 退房图片url|
|  pay_way  |   string  | 否   |  微信 | 支付方式   |
|  ple_price_count  |   number  | 是   |  1 | 退回押金   |
|  price_count  |   number  | 是   |  1 | 补收金额总计   |
|  rent_price_count  |   number  | 是   |  1 | 退回租金   |

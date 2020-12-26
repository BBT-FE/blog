---
title: 工作
date: 2020-10-10
tag:
 - 出云
photos:
 - https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1601048610347&di=3dd01bb770690eb4d298b41aea1d0a1c&imgtype=0&src=http%3A%2F%2Fwuxi.sinaimg.cn%2F2015%2F0716%2FU12148P1474DT20150716102645.jpg
---

<!-- 引言（简介） -->
  1. 外卖
  2. 审批
  3. hr
<!--more-->
### 外卖
#### 精选页: `src > pages > app > Pages > basic > Takeout_rewrite`
  页面缓存：蔡亦轩， 定位：陈亦好， 其他： 郝航宙
  后台： 张富生
  ###### 1. 定位逻辑
    - 点击精选 => 判断是否设置默认位置
      - 是： 精选页使用设置的位置信息
      - 否： 精选页定位
    - 扫物业码 => 点击图片 => 根据url上source判断从物业码进入，定位选择物业小区名称
    - 判断方法
  ```js
    async initData() {
      // 是否有缓存位置信息，有则取缓存位置
      let locationInfo = JSON.parse(localStorage.getItem('locationInfo'))
      // 判断是否需要取缓存位置， 在 Customer => index 存fromcustomer, 从点菜页返回到精选需要取之前地址
      let fromcustomer = localStorage.getItem('fromcustomer') 
      if (locationInfo && fromcustomer) {
        this.locationInfo = locationInfo
        this.addr = this.locationInfo.addr
        this.longitude = this.locationInfo.longitude
        this.latitude = this.locationInfo.latitude
        localStorage.removeItem('fromcustomer')
      } else {
        if (locationInfo) {
          localStorage.removeItem('locationInfo')
        }
        // 这个用在pc端展示， 主要用于调试
        if (!this.source || (!isMhApp() && !isWeiXin())) {
          this.searchCompanyName()
          this.getList()
          this.findStoreExternalCouponActivityDetail()
        } else {
          // 判断是否设置默认位置 if (data && checked === 'true') ? 则取默认设置位置 : 去定位resetPos()
          this.findConfigByKeyAndCompanyId()
        }
      }
    },
  ```
  ###### 2. 其他 就是list列表，休息就置灰， 后台返回字段

#### 精选页: `src > pages > Takeout > Pages > App > Customer > index`
  页面： 郝航宙
  后台： 王国兴
###### 1. 金额计算
  结算金额 => （单价 + 包装费） * 数量 - 满减
  - 菜品金额（含包装费）： computed => countPrice
  - 包装费 ： computed => packingFee

###### 2. 购物车
  - 购物车存在商品
    - 方法： findAllByCart（）
  - 再来一单 
    - 方法： getDetail（）
    
###### 3. 
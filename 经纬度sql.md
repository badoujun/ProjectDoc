# 1.概念

## 1.1经度

定义为地球表面某点随地球自转所形成的轨迹。

## 1.2纬度

经线也称子午线，和纬线一样是人类为度量方便而假设出来的辅助线，定义为地球表面连接南北两极的大圆线上的半圆弧。任两根经线的长度相等，相交于南北两极点。每一根经线都有其相对应的数值，称为经度。经线指示南北方向。

子午线命名的由来:"某一天体视运动轨迹中，同一子午线上的各点该天体在上中天(午)与下中天(子)出现的时刻相同。"不同的经线具有不同的地方时。偏东的地方时要比较早，偏西的地方时要迟。

## 1.3角度

把一个圆周平均分成360份，其中的每一份都是1o的角。这种以“度”作为单位来度量角度单位制叫做角度制。

## 1.4弧度

![1568856632388](E:\研究笔记\image\1568856632388.png)

长度为半径长的弧，所对的圆心角是 1 **弧度**（Radian），用符号**rad**表示。

正角度弧度数是一个正数，负角度弧度数是一个负数，零角度弧度数。半径为r的圆的圆心角α 所对的弧度长为l，那么角α 的弧度数的绝对值是 | α | = l / r。

## 1.5角度跟弧度转换

![1568857172906](image\1568857172906.png)

```shell
# 角度制与弧度制的换算
360° = 2π rad
180° = π rad
1° =（π / 180）rad ≈ 0.01745 rad
1 rad =（180 /π）o ≈ 57.30 o

α 度的角 =  α ·（π / 180）rad
```

# 2.经纬度互换

```shell
度(DDD)：E 108.90593度    N 34.21630度

    如何将度(DDD):： 108.90593度换算成度分秒(DMS)东经E 108度54分22.2秒?转换方法是将108.90593整数位不变取108(度),用0.90593*60=54.3558,取整数位54(分),0.3558*60=21.348再取整数位21(秒),故转化为108度54分21秒.

   同样将度分秒(DMS):东经E 108度54分22.2秒 换算成度(DDD)的方法如下:
   - 108度54分22.2秒=108+(54/60)+(22.2/3600)=108.90616度

因为计算时小数位保留的原因，导致正反计算存在一定误差，但误差影响不是很大。1秒的误差就是几米的样子。
# DDD转换成DMS时，D直接取D，然后小数位乘以60取整为分，分的小数位再乘以60取整为秒 
# DMS转换成DDD，分除以60
```

# 3.经纬度换上成米

```shell
纬度分为60分，每一分再分为60秒以及秒的小数。

纬度线投射在图上看似水平的平行线，但实际上是不同半径的圆。有相同特定纬度的所有位置都在同一个纬线上。 
赤道的纬度为0°，将行星平分为南半球和北半球。 
纬度是指某点与地球球心的连线和地球赤道面所成的线面角，其数值在0至90度之间。位于赤道以北的点的纬度叫北纬，记为N，位于赤道以南的点的纬度称南纬，记为S。
纬度数值在0至30度之间的地区称为低纬地区，纬度数值在30至60度之间的地区称为中纬地区，纬度数值在60至90度之间的地区称为高纬地区。
赤道、南回归线、北回归线、南极圈和北极圈是特殊的纬线。
纬度1秒的长度
# 地球的子午线总长度大约40008km。平均：
- 纬度1度 = 40075.04km/360°=111.31955km 
- 纬度1分 = 111.31955km/60'=1855.3m
- 纬度1秒 =  1855.3m/60=30.92m
```

# 4.任意两点间的距离

```shell
设第一点A的经 纬度为(LonA, LatA)，第二点B的经纬度为(LonB, LatB)
按照0度经线的基准
- 东经取经度的正值(Longitude)
- 西经取经度负值(-Longitude)
- 北纬取90-纬度值(90- Latitude)
- 南纬取90+纬度值(90+Latitude)
# 经过上述处理过后的两点被计为 (MLonA, MLatA)和(MLonB, MLatB)
# 计算两点距离的公式
- C = sin(LatA)*sin(LatB) + cos(LatA)*cos(LatB)*cos(MLonA-MLonB)
- Distance = R*Arccos(C)*Pi/180
```

# 5.参考案例

> 最近做一个项目：需要查询一个站点（已知该站点经纬度）500米范围内的其它站点。所以，我首先想到的是，对每条记录，去进行遍历，跟数据库中的每一个点进行距离计算，当距离小于500米时，认为匹配。这样做确实能够得到结果，但是效率极其低下，因为每条记录都要去循环匹配n条数据，其消耗的时间可想而知。
>
> 于是我就想到一个先过滤出大概的经纬度范围再进行计算。比方说正方形的四个点，于是我在网上搜索，意外的，查询到了一个关于这个计算附近地点搜索初探，里面使用Python实现了这个想法。所以参考了一下原文中的算法，使用Java进行了实现。
>
> 实现原理也是很相似的，先算出该点周围的矩形的四个点，然后使用经纬度去直接匹配数据库中的记录。

## 5.1设计思路

思路：首先算出“给定坐标附近500米”这个范围的坐标范围，过滤出范围外的数据。再对这些数据进行距离运算以及排序。

## 5.2数据过滤

虽然它是个圆，但我们可以先求出该圆的外接正方形，然后拿正方形的经纬度范围去搜索数据库。

```java
/** 
*过滤筛选范围代码，返回一个list，再对list处理
*longitude经度 latitude纬度
*/
public List<Property> findNeighPosition(double longitude,double latitude){
        //先计算查询点的经纬度范围
        double r = 6371;//地球半径千米
        double dis = 0.5;//0.5千米距离
        double dlng =  2*Math.asin(Math.sin(dis/(2*r))/Math.cos(latitude*Math.PI/180));
        dlng = dlng*180/Math.PI;//角度转为弧度
        double dlat = dis/r;
        dlat = dlat*180/Math.PI;        
        double minlat =latitude-dlat;
        double maxlat = latitude+dlat;
        double minlng = longitude -dlng;
        double maxlng = longitude + dlng;
        
        String hql = "from Property where longitude>=? and longitude =<? and latitude>=? latitude=<? and state=0";//预编译sql
        //传入的参数
        Object[] values = {minlng,maxlng,minlat,maxlat};
        //拿到查询结果集合
        List<Property> list = find(hql, values);
        return list;
    }
```

## 5.3计算距离

```java
import java.text.DecimalFormat;
//给定两个点，求距离
public class DistanceUtil {
    public static void main(String[] args) {
        //根据两点间的经纬度计算距离，单位：km
        String s = algorithm(115.21221, 1.5, 114.21221, 0);
        System.out.println(s);
    }
    private static double rad(double d) {
        return d * Math.PI / 180.00; //角度转换成弧度
    }
    /*
    * 根据经纬度计算两点之间的距离（单位米）
    * */
    public static String algorithm(double longitude1, double latitude1, double longitude2, double latitude2) {
        double Lat1 = rad(latitude1); // 纬度
        double Lat2 = rad(latitude2);
        double a = Lat1 - Lat2;//两点纬度之差
        double b = rad(longitude1) - rad(longitude2); //经度之差
        double s = 2 * Math.asin(Math.sqrt(Math.pow(Math.sin(a / 2), 2)
                   + Math.cos(Lat1) * Math.cos(Lat2) * Math.pow(Math.sin(b / 2), 2)));//计算距离
        s = s * 6378137.0;//弧长乘地球半径（半径为米）
        s = Math.round(s * 10000d) / 10000d;//精确距离的数值
        s = s/1000;//将单位转换为km，如果想得到以米为单位的数据 就不用除以1000
        //四舍五入 保留一位小数
        DecimalFormat df = new DecimalFormat("#.0");
        return df.format(s);
    }
}

```

## 5.4计算范围

```java
public static Map<String, String> getAround(String latStr, String lngStr, String raidus) {
        Map<String, String> map = new HashMap<String, String>();

        Double latitude = Double.parseDouble(latStr);// 传值给经度
        Double longitude = Double.parseDouble(lngStr);// 传值给纬度

        Double degree = (24901 * 1609) / 360.0; // 获取每度
        double raidusMile = Double.parseDouble(raidus);

        Double mpdLng = Double.parseDouble((degree * Math.cos(latitude * (Math.PI / 180))+"").replace("-", ""));
        Double dpmLng = 1 / mpdLng;
        Double radiusLng = dpmLng * raidusMile;
        //获取最小经度
        Double minLat = longitude - radiusLng;
        // 获取最大经度
        Double maxLat = longitude + radiusLng;

        Double dpmLat = 1 / degree;
        Double radiusLat = dpmLat * raidusMile;
        // 获取最小纬度
        Double minLng = latitude - radiusLat;
        // 获取最大纬度
        Double maxLng = latitude + radiusLat;

        map.put("minLat", minLat+"");
        map.put("maxLat", maxLat+"");
        map.put("minLng", minLng+"");
        map.put("maxLng", maxLng+"");

        return map;
    }
```





## 5.5优化分析

主要需要优化的内容：

1. 避免数据库的大量计算，这个已经实现。
2. 查询出数据后，需要计算出距离，并对距离排序，而且需要用到分页，每次不一次性把数据发送给前段。没有办法直接在数据库查询时就分页，因为查询时还没有计算出具体的距离，所以此时还没有排序。没有排序就无法达到需求中所要求的安距离升序排序。
3. 用户每次定位，就需要重新查询范围内的点，并计算点到点的距离。

```shell
1. 可以对距离进行把控，减少距离范围内的点来达到缩小数据量的目的
2. 可能距离设置不合理，导致范围内没有坐标点，就需要动态扩大搜索范围
3. 尽量减少用户访问数据库的次数，所以一次查询出全部范围内的点再存储起来
4. 但是每个用户就需要维护自己的一个空间来放查询出来的内容，这样需要的空间是非常大的
5. 所以用户自身的空间只需要存放点对应数据行的id即可，再通过这些id去redis或者mysql中查
```

# 6.sql方式

```shell
$Lat1=14.59742107259540;//我的

$Lng1=120.98306272292028;//我的
 //由小到大的距离排序，使用场景：查询和我距离最近的人或者店铺。把地球看作一个规则的球体。
 select 
    acos(cos($lat*pi()/180 )*cos(lat*pi()/180)*cos($lng*pi()/180 -lng*pi()/180)+sin($lat*pi()/180 )*sin(lat*pi()/180))*6370996.81/1000  as distance
    from x'x'x  order by distance asc 

单位：千米（公里）

距离计算还算可以 ，我跟同一公司的同事定位距离10米以内

```



```xml
  <select id="findAllByCondition" resultType="com.virtue.terminal.dto.response.BusinessDto">
        select b.id,b.business_name,b.subtitle,b.head_img,bs.star_class,b.remarks,bs.enrolment_number,b.course_label,b.address,
        acos(cos(#{userLat}*pi()/180 )*cos(b.lat*pi()/180)*cos(#{userLng}*pi()/180 -b.lng*pi()/180)+sin(#{userLat}*pi()/180 )*sin(b.lat*pi()/180))*6370996.81 as distance
        from `business` as b
        left join business_score as bs on b.id=bs.business_id
        <choose>
            <when test="searchStr!=null and searchStr!='' and categoryId==null">
                where
                 (business_name like concat('%',#{searchStr},'%') or subtitle like concat('%',#{searchStr},'%'))
                 and b.city=#{city}
                 and b.lat between #{minLat} and #{maxLat}
                 and b.lng between #{minLng} and #{maxLng}
            </when>
            <when test="searchStr==null and categoryId!=null">
                left join course as c on b.id=c.business_id
                where
                 c.catalog like concat('%.',#{categoryId},'.%')
                 and b.city = #{city}
                 and b.lat between #{minLat} and #{maxLat}
                 and b.lng between #{minLng} and #{maxLng}
            </when>
            <when test="searchStr!=null and searchStr!='' and categoryId!=null">
                left join course as c on b.id=c.business_id
                where
                  (business_name like concat('%',#{searchStr},'%') or subtitle like concat('%',#{searchStr},'%') )
                  and c.catalog like concat('%.',#{categoryId},'.%')
                  and b.city = #{city}
                  and b.lat between #{minLat} and #{maxLat}
                  and b.lng between #{minLng} and #{maxLng}
            </when>
            <otherwise>
                where b.city = #{city}
                and b.lat between #{minLat} and #{maxLat}
                and b.lng between #{minLng} and #{maxLng}
            </otherwise>
        </choose>
       order by ${orderStyle} desc
    </select>
```

```shell
1. select 开始 --结束 中可以获取外界参数
2. order by ${orderStyle} 排序要这么用，关键字上不能带单引号
```

# 7. 代码工具类

```java
package com.virtue.terminal.helper;

import com.virtue.commons.Assert;
import com.virtue.terminal.dto.request.LatAndLngRangeDto;

import java.text.DecimalFormat;


/**
 * @auther zhangHongLu
 * @date 2019/10/31 0031 17:15
 */
public class DistanceUtil {

    /**
     * 得到经纬度范围
     * @param latAndLngRangeDto
     * @param radiusStr
     */
    public static void getRange(LatAndLngRangeDto latAndLngRangeDto,String radiusStr){

        Double latitude = Double.parseDouble(latAndLngRangeDto.getUserLat());// 传值给经度
        Double longitude = Double.parseDouble(latAndLngRangeDto.getUserLng());// 传值给纬度
        Double degree = (24901 * 1609) / 360.0; // 获取每度
        double radius = Double.parseDouble(radiusStr);

        Double mpdLng = Double.parseDouble((degree * Math.cos(latitude * (Math.PI / 180))+"").replace("-", ""));
        Double dpmLng = 1 / mpdLng;
        Double radiusLng = dpmLng * radius;
        // 获取最小经度
        Double minLat = longitude - radiusLng;
        // 获取最大经度
        Double maxLat = longitude + radiusLng;
        Double dpmLat = 1 / degree;
        Double radiusLat = dpmLat * radius;
        // 获取最小纬度
        Double minLng = latitude - radiusLat;
        // 获取最大纬度
        Double maxLng = latitude + radiusLat;

        latAndLngRangeDto.setMinLat(minLat.toString());
        latAndLngRangeDto.setMaxLat(maxLat.toString());
        latAndLngRangeDto.setMinLng(minLng.toString());
        latAndLngRangeDto.setMaxLng(maxLng.toString());

    }

    /**
     * 角度转换成弧度
     * @param d
     * @return
     */
    private static double rad(double d) {
        return d * Math.PI / 180.00;
    }

    /**
     * 根据经纬度计算两点之间的距离（单位米）
     * @param longitude1
     * @param latitude1
     * @param longitude2
     * @param latitude2
     * @return
     */
    public static String countDistance(String longitude1, String latitude1, String longitude2, String latitude2) {

        if (longitude1==null||latitude1==null||longitude2==null||latitude2==null){

            throw new IllegalArgumentException("参数不能为空");

        }

        // 纬度
        double Lat1 = rad(Double.parseDouble(latitude1));
        double Lat2 = rad(Double.parseDouble(latitude2));
        //两点纬度之差
        double a = Lat1 - Lat2;
        //经度之差
        double b = rad(Double.parseDouble(longitude1)) - rad(Double.parseDouble(longitude2));
        //计算距离
        double s = 2 * Math.asin(Math.sqrt(Math.pow(Math.sin(a / 2), 2) + Math.cos(Lat1) * Math.cos(Lat2) * Math.pow(Math.sin(b / 2), 2)));
        //弧长乘地球半径（半径为米）
        s = s * 6378137.0;
        //精确距离的数值
        s = Math.round(s * 10000d) / 10000d;

        DecimalFormat df=null;
        // 如果是前面除以1000，保留小数位后两位
        if (s>=1000){
            s = s/1000;
            df = new DecimalFormat("#.00km");
        }
        //如果是m只保留整数
        df = new DecimalFormat("#m");
        return df.format(s);
    }
}

```


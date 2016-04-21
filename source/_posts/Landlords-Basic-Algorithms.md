---
title: 斗地主基础算法
date: 2016-04-20 14:44:31
categories:
- technology
- game
tags: ['Landlords', 'Game', 'Algorithms']
---
## 前言
这个月有幸被公司安排做斗地主游戏，特地在网上查了很多相关资料，有所感悟，因此写下这篇文章，来总结下斗地主的基本算法。
***
### 扑克定义
一副扑克有54张牌，分别是
* 方块 A-K
* 梅花 A-K
* 红桃 A-K
* 黑桃 A-K
* 大王和小王

从程序角度，我们都要把牌转化成数据表示。那么我们必须要先定义54张牌的数据，其中每一张牌包括花色和数值两方面信息。
按照面向对象的角度来看，好像可以这样定义
```
public class CardData
{
    private var value:int;//数值

    private var color:int;//花色
}
```
有没有觉得这样定义一张牌的信息，是不是太麻烦了？单单为了表示一张卡牌的信息，需要额外定义一个类来表示。
你有没有想到更好的方法呢？
能不能只用一个数值就能把花色和数值两个方面都能照顾到？这样可以考虑下使用位运算的做法：四种花色的A-K一共13张牌，那可以采用十六进制，让高地位分别表示花色和数值；低位足够表示1-13种数值，高位足够表示4种花色以及大小王。
0xAB，A位表示花色，按照正常逻辑分配，方块用0x00表示，梅花用0x10表示，红桃用0x20表示，黑桃用0x30表示，大小王用0x40表示；
0xAB，B位表示数值，按照升序原则，A-K对应十六进制里面的A-D；大小王对应十六进制里面的E、F
整副扑克牌数据如下：
```
public static const POKERS:Array = [
    0x01,0x02,0x03,0x04,0x05,0x06,0x07,0x08,0x09,0x0A,0x0B,0x0C,0x0D,//方块 A - K
    0x11,0x12,0x13,0x14,0x15,0x16,0x17,0x18,0x19,0x1A,0x1B,0x1C,0x1D,//梅花 A - K
    0x21,0x22,0x23,0x24,0x25,0x26,0x27,0x28,0x29,0x2A,0x2B,0x2C,0x2D,//红桃 A - K
    0x31,0x32,0x33,0x34,0x35,0x36,0x37,0x38,0x39,0x3A,0x3B,0x3C,0x3D,//黑桃 A - K
    0x4E,0x4F //大小王
];
```
这时候有人会问，数据是定义好了，那如何在一个数字中同时获取卡牌的花色和数字呢？
还记得我们定义卡牌的时候，使用了一个十六进制来表示花色和数值吧？高位是花色，低位是数值，那我们只需要做一个简单地位运算，就能计算出来。需要获取花色的时候，把低位去掉，高位不变；如要取数值的时候，把高位去掉，低位不变。这就刚好满足“与”位运算的特点，即必须满足两个位同时为1的时候才为1。也就是说，取花色只需要和0xF0做“与”位运算，就能获取到花色；取数值只需要和0x0F做“与”位运算，就能获取到数值；
```
/**
 * 获取花色
 * @param cardData
 * @return
 *
 */
public static function getCardColor(cardData:uint):uint
{
    return cardData & 0xF0;
}

/**
 * 获取数值
 * @param cardData
 * @return
 *
 */
public static function getCardValue(cardData:uint):uint
{
    return cardData & 0x0F;
}
```
### 拆牌
```
/**
 *
 * @param cardDataArr
 * @param cardCount
 * @return
 *
 */
public static function analyseCardData(cardDataArr:Array, cardCount:int):LandlordsAnalyseResult
{
    var analyseResult:LandlordsAnalyseResult = new LandlordsAnalyseResult();
    var logicValue:int;
    for (var i:int = 0; i < cardCount; i++)
    {
        var sameCount:int = 1;
        logicValue = getCardLogicValue(cardDataArr[i]);
        for (var j:int = i + 1; j < cardCount; j++)
        {
            if (getCardLogicValue(cardDataArr[j]) != logicValue)
                break;
            sameCount++;
        }

        var index:int;
        switch (sameCount)
        {
            case 1:
            {
                index = analyseResult.singleCount++;
                analyseResult.singleCardData[index * sameCount] = cardDataArr[i];
                break;
            }
            case 2:
            {
                index = analyseResult.doubleCount++;
                analyseResult.doubleCardData[index * sameCount] = cardDataArr[i];
                analyseResult.doubleCardData[index * sameCount + 1] = cardDataArr[i + 1];
                break;
            }
            case 3:
            {
                index = analyseResult.threeCount++;
                analyseResult.threeCardData[index * sameCount] = cardDataArr[i];
                analyseResult.threeCardData[index * sameCount + 1] = cardDataArr[i + 1];
                analyseResult.threeCardData[index * sameCount + 2] = cardDataArr[i + 2];
                break;
            }
            case 4:
            {
                index = analyseResult.fourCount++;
                analyseResult.fourCardData[index * sameCount] = cardDataArr[i];
                analyseResult.fourCardData[index * sameCount + 1] = cardDataArr[i + 1];
                analyseResult.fourCardData[index * sameCount + 2] = cardDataArr[i + 2];
                analyseResult.fourCardData[index * sameCount + 3] = cardDataArr[i + 3];
                break;
            }
        }
        i += sameCount - 1;
    }
    trace("");
    trace("======================");
    trace("analyseResult.fourCount: " + analyseResult.fourCount);
    trace("analyseResult.fourCardData: [" + analyseResult.fourCardData + "]");
    trace("analyseResult.threeCount: " + analyseResult.threeCount);
    trace("analyseResult.threeCardData: [" + analyseResult.threeCardData + "]");
    trace("analyseResult.doubleCount: " + analyseResult.doubleCount);
    trace("analyseResult.doubleCardData: [" + analyseResult.doubleCardData + "]");
    trace("analyseResult.singleCount: " + analyseResult.singleCount);
    trace("analyseResult.singleCardData: [" + analyseResult.singleCardData + "]");
    trace("======================");
    trace("");
    return analyseResult;
}
```
### 牌型
```
/**
 * 错误类型，CT缩写CARD_TYPE
 */
public static const CT_ERROR:String = "错误类型";

/**
 * 单牌类型
 */
public static const CT_SINGLE:String = "单牌类型";

/**
 * 对牌类型
 */
public static const CT_DOUBLE:String = "对牌类型";

/**
 * 三条类型
 */
public static const CT_THREE:String = "三条类型";

/**
 * 单连类型
 */
public static const CT_SINGLE_LINE:String = "单连类型";

/**
 * 对连类型
 */
public static const CT_DOUBLE_LINE:String = "对连类型";

/**
 * 三连类型
 */
public static const CT_THREE_LINE:String = "三连类型";

/**
 * 三带一单
 */
public static const CT_THREE_LINE_TAKE_ONE_SINGLE:String = "三带一单";

/**
 * 三带一对
 */
public static const CT_THREE_LINE_TAKE_ONE_PAIR:String = "三带一对";

/**
 * 四带两单
 */
public static const CT_FOUR_LINE_TAKE_TWO_SINGLE:String = "四带两单";

/**
 * 四带两对
 */
public static const CT_FOUR_LINE_TAKE_TWO_PAIR:String = "四带两对";

/**
 * 炸弹类型
 */
public static const CT_BOMB:String = "炸弹类型";

/**
 * 火箭类型
 */
public static const CT_MISSILE:String = "火箭类型";
```
# 判断某字符串是否是有效的十进制数
---
*问题来源于自 `leetcode` 有效字符算法题*

## 问题描述:
 验证给定的字符串是否可以解释为十进制数字。
 > 例如:
 > 
 > "0" => true
 > " 0.1 " => true
 > "abc" => false
 > "1 a" => false
 > "2e10" => true
 > " -90e3   " => true
 > " 1e" => false
 > "e3" => false
 > " 6e-1" => true
 > " 99e2.5 " => false
 > "53.5e93" => true
 > " --6 " => false
 > "-+3" => false
 > "95a54e53" => false
 > 
 > 说明: 我们有意将问题陈述地比较模糊。在实现代码之前，你应当事先思考所有可能的情况。这里给出一份可能存在于有效十进制数字中的字符列表：
 > 
 > 数字 0-9
 > 指数 - "e"
 > 正/负号 - "+"/"-"
 > 小数点 - "."
 > 当然，在输入中，这些字符的上下文也很重要。
 > 
 > 来源：力扣（LeetCode）
 > 链接：https://leetcode-cn.com/problems/valid-number 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解题思路:
  ### **有限状态题解思路**
  - 初步分析:  
  根据题意我们可以判断出只有一部分字符出现整体才可能返回true;  
  这部分字符是:`0~9` & `space` & `e` & `.` & `+` & `-` 共15个字符，除此之外其他字符一旦出现，直接返回`false`;
  首先我们把当先处理的各个状态列举出来,然后根据状态逐个推演穷举情况分类即可;  
      |     处理状态      |        说明        |
      | :---------------: | :----------------: |
      |    INIT = 0x00    |      初始状态      |
      |  FIND_NUM = 0x01  |  处理到了字符数字  |
      |  FIND_DOT = 0x02  |   处理到了小数点   |
      |   FIND_E = 0x04   |  处理到了字符`e`   |
      | FIND_SIGN = 0x08  | 处理到了`+` 和 `-` |
      | FIND_SPACE = 0x10 |    处理到了空格    |
  那么我们的函数也就很简单的可以分为以下步骤:
   1. 校验字符串是否结束
   2. 分门别类进入对应状态,非法字符直接返回 `false`
   3. 字符迭代,str++,执行步骤1
    ```c
    //C语言示范:
    typedef enum _STATUS
    {
        INIT = 0x00,
        FIND_NUM = 0x01,
        FIND_DOT = 0x02,
        FIND_E = 0x04,
        FIND_SIGN = 0x08,
        FIND_SPACE = 0x10,
    } STATUS ;

    bool isNumber(char * s){
        STATUS sta=INIT;
        for(;*s != 0;s++)
        {
            switch(*s)
            {
                case 'e':
                break;

                case '.':
                break;

                case '-':
                case '+':
                break;

                case ' ':
                break;

                case '0':
                case '1':
                case '2':
                case '3':
                case '4':
                case '5':
                case '6':
                case '7':
                case '8':
                case '9':
                break;

                default:
                    return false;
                break;
            }
        }
        if((sta&FIND_NUM) == 0)
            return false;
    return true;
    }
    ```
  - 相应处理:
  每一个 `case` 的核心思想都是 将sta状态置为这个`case`中应该要变成的状态; 像如果在 ```case 'e':``` 中,那么将需要执行操作 ```sta |= FIND_E;```  
  但是由此衍生出一个问题,如果连续出现两个字符 `e` 那么第二个 字符 `e` 就不能被识别而是直接返回 `false`  同理其他case 中也有出现相对应的问题;  
  在这个环节我基本就一直在试错,重复了N次,终于可以总结一下规律,由于是依据 **`leetcode`** 的算法题,因此也可以说是他的模糊规则;  
    | 规则                                             | 举例       | 返回值 |
    | :----------------------------------------------- | :--------- | :----: |
    | 小数点不能出现两次                               | "0.21.2"   | false  |
    | 自然底数 ***`e`*** 不能出现两次                  | 1.4e3e6    | false  |
    | 正负号不允许出现两次                             | "--0"      | false  |
    | 空格出现在最前或最后都为合理                     | "  1.3  "  |  true  |
    | 小数点前或后没有数字也可正确识别                 | ".3"&&"1." |  true  |
    | ***`e`*** 后不允许出现小树                       | "1.e0.5    | false  |
    | ***`e`*** 后允许出现正负数                       | "1.e-5     |  true  |
    | 单个字符 "***`e`***" "`space`" "`.`"等不允许出现 | "e","."... | false  |
  - 于是在 `case 'e':` 中 需要处理的问题就变成了:  
    1. 两次相同的字符e  
    2. e前一定要有数字  
    3. e 前不能有空格
    ```c
        case 'e':
        if(sta & FIND_E || (sta & FIND_NUM ) == 0  || sta & FIND_SPACE)
            return false;
        else
            sta = FIND_E;//字母 e 后需要跟一个正确的十进制整数,因此所有状态清零,重新写入(除去 find_e)
            sta &= (~FIND_NUM);      //数字置零从新开始
        break;
    ```  

  - 同理 `case '.':`中:  
    1. 小数点前不能有小数点
    2. 小数点前不能有字符 ***`e`*** 
    3. 小数点前不能有 `有效字符 "空格" ` 具体定义请参阅:*`空格字符的处理`*  
    ```c
    case '.':
            if(sta & FIND_DOT || sta & FIND_E || sta & FIND_SPACE)
                return false;
            else
            {
                sta |= FIND_DOT;//置为状态为寻找到了点
            }
            break;
    ```  

  - `case '+':` && `case '-':` 中:
  1. 正负号前不能有正负号
  2. 正负号前不能有数字
  3. 正负号前不能有小数点  
        ```c
        case '-':
        case '+':
            if(sta &(~FIND_E))
                return false;
            else
                sta |= FIND_SIGN;
        break;
        ```
  - `case ' ':` 中:( **`有效空格`** 定义为 非最开始和最末尾的空格)
    1. 该空格前有数字
    2. 该空格前有小数点
    3. 该空格前有符号
    4. 该空格前有字符 ***`e`***
    ```c
    case ' ':
    if(sta&FIND_NUM || (sta&FIND_DOT) || (sta&FIND_E)|| (sta & FIND_SIGN))
        sta |= FIND_SPACE;//只有满足上述4个条件才能算作有效空格
    break;
    ```
  - `case '0~9':` 中:
    1. 首先置为找到数字
    2. 数字前不能有空格(有效空格) 翻译:(找到空格和数字不能同时存在)
    ```c
    case '0':
    case '1':
    case '2':
    case '3':
    case '4':
    case '5':
    case '6':
    case '7':
    case '8':
    case '9':
        sta |= FIND_NUM;
        if((sta & (FIND_SPACE|FIND_NUM))==((FIND_SPACE|FIND_NUM))) //两条件如果同时存在 那么返回false
            return false;
    break;
    ```
  - 最后 如果上述case中的情况都满足,例如:,".","     "等字符,但是也是不符合要求的,因为他们之间并没有数字,因此,最后返回前需要判断是否寻找到数字,在`case 'e'` 虽然会重置状态,但是同理 字符 ***`e`*** 后也需要一个正确的整数,因此有以下最后一个判断:
    ```c
    if((sta&FIND_NUM) == 0)
        return false;
    ```

## 第一次分享自己的算法,有不对和难以理解的地方还请评论,多多讨论,共同进步~ :)

## 源码
```c
// C语言源码
typedef enum _STATUS
{
    INIT = 0x00,
    FIND_NUM = 0x01,
    FIND_DOT = 0x02,
    FIND_E = 0x04,
    FIND_SIGN = 0x08,
    FIND_SPACE = 0x10
} STATUS ;
bool isNumber(char * s){
    STATUS sta=INIT;
//    int numberCnt = 0;
    for(;*s != 0;s++)
    {
        switch(*s)
        {
            case 'e':
                if(sta & FIND_E || (sta & FIND_NUM ) == 0  || sta & FIND_SPACE)
                    return false;
                else
                    sta = FIND_E;//字母 e 后需要跟一个正确的十进制数,因此所有状态清零,重新写入(除去 find_e)
                    sta &= (~FIND_NUM);      //数字置零从新开始
                break;
            case '.':
                if(sta & FIND_DOT || sta & FIND_E || sta & FIND_SPACE)
                    return false;
                else
                {
                    sta |= FIND_DOT;
  //                  numberCnt++;

                }
                break;
            case '-':
            case '+':
                if(sta &(~FIND_E))
                    return false;
                else
                    sta |= FIND_SIGN;
                break;
            case ' ':
                if(sta&FIND_NUM || (sta&FIND_DOT) || (sta&FIND_E)|| (sta & FIND_SIGN))
                    sta |= FIND_SPACE;
                break;
            case '0':
            case '1':
            case '2':
            case '3':
            case '4':
            case '5':
            case '6':
            case '7':
            case '8':
            case '9':
                sta |= FIND_NUM;
                if((sta & (FIND_SPACE|FIND_NUM))==((FIND_SPACE|FIND_NUM)))
                    return false;
            break;
            default:
                return false;
            break;
        }
    }
    if((sta&FIND_NUM) == 0)
        return false;
    return true;
}
```

###### ***[小声逼逼] 第一次 0ms 还挺激动***
    




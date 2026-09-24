title: simhash算法介绍和实现
tags:
  - Java
  - Algorithm
categories: []
date: 2016-03-03 19:36:00
---
simhash是google用来处理海量文本去重的算法。 google出品，你懂的。 simhash最牛逼的一点就是将一个文档，最后转换成一个64位的字节，暂且称之为特征字，然后判断重复只需要判断他们的特征字的距离是不是<n,(根据经验这个n一般取值为3），就可以判断两个文档是否相似。

详细介绍参考:  
http://yanyiwu.com/work/2014/01/30/simhash-shi-xian-xiang-jie.html  
http://www.lanceyan.com/tech/arch/simhash_hamming_distance_similarity.html

代码实现:  
http://my.oschina.net/leejun2005/blog/150086

以下是我根据某位大神的代码修改而来的:   
IKAnalyer的jar在http://pan.baidu.com/s/1nu33Q5r  
luncene包使用maven下载   
其它两个依赖的java文件,自己去实现一下,一个是去除html tag的,另一个是读取文件中的内容的.   

```java
package cn.allydata.util;

import java.io.IOException;
import java.io.StringReader;
import java.math.BigInteger;
import java.util.HashMap;

import org.apache.lucene.analysis.Analyzer;
import org.apache.lucene.analysis.TokenStream;
import org.apache.lucene.analysis.tokenattributes.CharTermAttribute;
import org.wltea.analyzer.lucene.IKAnalyzer;

import com.wjb.util.common.WjbFileUtil;
import com.wjb.util.common.WjbHtmlUtil;
 
/**
 * Function: simHash 判断文本相似度，该示例程支持中文<br/>
 * date: 2013-8-6 上午1:11:48 <br/>
 * @author june
 * @version 0.1
 
 <dependency>
	<groupId>org.apache.lucene</groupId>
	<artifactId>lucene-core</artifactId>
	<version>3.6.0</version>
 </dependency>

 */
public class WjbSimHash {
 
    private String tokens;
 
    private BigInteger intSimHash;
 
    private String strSimHash;
 
    private int hashbits = 64;
 
    public WjbSimHash(String tokens) throws IOException {
        this.tokens = tokens;
        this.intSimHash = this.simHash();
    }
 
    public WjbSimHash(String tokens, int hashbits) throws IOException {
        this.tokens = tokens;
        this.hashbits = hashbits;
        this.intSimHash = this.simHash();
    }
 
    HashMap<String, Integer> wordMap = new HashMap<String, Integer>();
 
    public BigInteger simHash() throws IOException {
        // 定义特征向量/数组
        int[] v = new int[this.hashbits];
        // 英文分词
        // StringTokenizer stringTokens = new StringTokenizer(this.tokens);
        // while (stringTokens.hasMoreTokens()) {
        // String temp = stringTokens.nextToken();
        // }
        // 1、中文分词，分词器采用 IKAnalyzer3.2.8 ，仅供演示使用，新版 API 已变化。
        
		Analyzer analyzer = new IKAnalyzer(true);
		StringReader reader = new StringReader(this.tokens);
		TokenStream ts = analyzer.tokenStream("", reader);  
		CharTermAttribute term=ts.getAttribute(CharTermAttribute.class); 
		
        String word = null;
        while(ts.incrementToken()){
            word = term.toString();
            // 注意停用词会被干掉
            // System.out.println(word);
            // 2、将每一个分词hash为一组固定长度的数列.比如 64bit 的一个整数.
            BigInteger t = this.hash(word);
            for (int i = 0; i < this.hashbits; i++) {
                BigInteger bitmask = new BigInteger("1").shiftLeft(i);
                // 3、建立一个长度为64的整数数组(假设要生成64位的数字指纹,也可以是其它数字),
                // 对每一个分词hash后的数列进行判断,如果是1000...1,那么数组的第一位和末尾一位加1,
                // 中间的62位减一,也就是说,逢1加1,逢0减1.一直到把所有的分词hash数列全部判断完毕.
                if (t.and(bitmask).signum() != 0) {
                    // 这里是计算整个文档的所有特征的向量和
                    // 这里实际使用中需要 +- 权重，比如词频，而不是简单的 +1/-1，
                    v[i] += 1;
                } else {
                    v[i] -= 1;
                }
            }
        }
 
        BigInteger fingerprint = new BigInteger("0");
        StringBuffer simHashBuffer = new StringBuffer();
        for (int i = 0; i < this.hashbits; i++) {
            // 4、最后对数组进行判断,大于0的记为1,小于等于0的记为0,得到一个 64bit 的数字指纹/签名.
            if (v[i] >= 0) {
                fingerprint = fingerprint.add(new BigInteger("1").shiftLeft(i));
                simHashBuffer.append("1");
            } else {
                simHashBuffer.append("0");
            }
        }
        this.strSimHash = simHashBuffer.toString();
        return fingerprint;
    }
 
    private BigInteger hash(String source) {
        if (source == null || source.length() == 0) {
            return new BigInteger("0");
        } else {
            char[] sourceArray = source.toCharArray();
            BigInteger x = BigInteger.valueOf(((long) sourceArray[0]) << 7);
            BigInteger m = new BigInteger("1000003");
            BigInteger mask = new BigInteger("2").pow(this.hashbits).subtract(new BigInteger("1"));
            for (char item : sourceArray) {
                BigInteger temp = BigInteger.valueOf((long) item);
                x = x.multiply(m).xor(temp).and(mask);
            }
            x = x.xor(new BigInteger(String.valueOf(source.length())));
            if (x.equals(new BigInteger("-1"))) {
                x = new BigInteger("-2");
            }
            return x;
        }
    }
 
    public int hammingDistance(WjbSimHash other) {
 
        BigInteger x = this.intSimHash.xor(other.intSimHash);
        int tot = 0;
 
        // 统计x中二进制位数为1的个数
        // 我们想想，一个二进制数减去1，那么，从最后那个1（包括那个1）后面的数字全都反了，
        // 对吧，然后，n&(n-1)就相当于把后面的数字清0，
        // 我们看n能做多少次这样的操作就OK了。
 
        while (x.signum() != 0) {
            tot += 1;
            x = x.and(x.subtract(new BigInteger("1")));
        }
        return tot;
    }
 
    public int getDistance(String str1, String str2) {
        int distance;
        if (str1.length() != str2.length()) {
            distance = -1;
        } else {
            distance = 0;
            for (int i = 0; i < str1.length(); i++) {
                if (str1.charAt(i) != str2.charAt(i)) {
                    distance++;
                }
            }
        }
        return distance;
    }
 
    
    public BigInteger getIntSimHash()
    {
    	return this.intSimHash;
    }
    
    public String getStrSimHash()
    {
    	return this.strSimHash;
    }

 
    public static void main(String[] args) throws IOException {
        String s = WjbFileUtil.fromFile("d:/1.txt");
        s = WjbHtmlUtil.delHTMLTag(s);
        System.out.println(s);
        WjbSimHash hash1 = new WjbSimHash(s, 64);
 
        System.out.println("---------------------------------");
        // 删除首句话，并加入两个干扰串
        s = WjbFileUtil.fromFile("d:/2.txt",WjbFileUtil.GBK);
        s = WjbHtmlUtil.delHTMLTag(s);
        System.out.println(s);
        WjbSimHash hash2 = new WjbSimHash(s, 64);
 
//        System.out.println("============================");
// 
        int dis = hash1.getDistance(hash1.strSimHash, hash2.strSimHash);
        System.out.println(hash1.hammingDistance(hash2) + " " + dis);
    	long begin = System.currentTimeMillis();
//    	for(int i = 0 ; i < 10000000 ; i++)
//    	{
//    		hash1.hammingDistance(hash2);
//    	}
    	long end = System.currentTimeMillis();
    	System.out.println(end - begin);
    }
}
```
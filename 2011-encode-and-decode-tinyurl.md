# 535. Encode and Decode TinyURL

> TinyURL is a URL shortening service where you enter a URL such as`https://leetcode.com/problems/design-tinyurl`and it returns a short URL such as`http://tinyurl.com/4e9iAk`.
>
> Design the`encode`and`decode`methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

### 思路

1. 这道题目要把long url 缩短成1个short url，然后需要我们建立一个一一对应的关系，这样就容易decode出来了。建立这种映射关系的方法很多。
2. 这里用最简单的，就是把long url放入一个List中，然后short url就是其long url 对应在List中的index即可。但是该方法有弊端

### Code 1\)

该方法的问题：当List很长的时候，index很大，有可能就不是short url了，因为index很大，其实也是一个很长的URL

```java
public class Codec {
    List<String> list = new ArrayList();

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        list.add(longUrl);
        return String.valueOf(list.size() - 1);
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        int index = Integer.valueOf(shortUrl);
        return index < list.size() ? list.get(index) : "";
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```

### Code 2\)

1. 用最传统的方法，用2个map，一个map记录long URL -&gt; short URL 对应关系，可以快速找出重复的long URL对应的short URL的值。一个map记录short URL-&gt; long URL，可以用于decode short URL。
2. long URL 对应的short URL 就用hashCode\(\) function 来建立映射关系即可。

```java
public class Codec {
    HashMap<String, String> longUrlMap = new HashMap();
    HashMap<String, String> shortUrlMap = new HashMap();

    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        if(longUrlMap.containsKey(longUrl)) return longUrlMap.get(longUrl);
        String url = "http://tinyurl.com/" + longUrl.hashCode();
        longUrlMap.put(longUrl, url);
        shortUrlMap.put(url, longUrl);
        return url;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return shortUrlMap.getOrDefault(shortUrl, "");
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.decode(codec.encode(url));
```




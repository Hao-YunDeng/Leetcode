## HashTable Stack BitManip
### 535. Encode and Decode TinyURL
```java
public class Codec {
    HashMap<String, String> index = new HashMap(); //key, URL
    HashMap<String, String> revIndex = new HashMap(); //URL, key
    final String BASE_HOST = "http://tinyurl.com/";
    final String charSet = "abcdefghijklmnopqrstuvwxyz0123456789";
    // Encodes a URL to a shortened URL.
    public String encode(String longUrl) {
        if(revIndex.containsKey(longUrl)) {
            return revIndex.get(longUrl);
        }
        StringBuilder sb = new StringBuilder();
        while(true) {         
            for(int i = 0; i < 6; i++) {
                int pos = (int) (Math.random() * charSet.length());
                sb.append(charSet.charAt(pos));
            }
            
            if(!index.containsKey(sb.toString())) {
                break;
            }             
        }
        String key = sb.toString();
        revIndex.put(longUrl, key);
        index.put(key, longUrl);
        return BASE_HOST + key;
    }

    // Decodes a shortened URL to its original URL.
    public String decode(String shortUrl) {
        return index.get(shortUrl.substring(BASE_HOST.length()));
    }
}
```

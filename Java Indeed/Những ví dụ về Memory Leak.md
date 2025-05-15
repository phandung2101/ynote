# Những lý do chính gây ra Mem Leak ở Java
## 1. Static collections (Map, List, etc)
## 2. Không close resource (Files, DB Connection, Streams).
## 3. Listeners & Callback not removed (trong Swing, Android, etc.)
## 4. ThreadLocal Variables không được dọn dẹp
## 5. ClassLoader leak
Thường xuất hiện ở các webapp khi được redeploying
#### Example thực tế: mem leak xuất hiện ở caching system
```java
public class SessionCache {  
	private static final Map<Long, UserSession> cache = new HashMap<>();  
  
	public void addUserSession(Long userId, UserSession session) {  
		cache.put(userId, session); // Stored forever unless manually removed!  
	}  
	  
	public UserSession getSession(Long userId) {  
		return cache.get(userId);  
	}  
  
// Missing: A method to remove unused sessions!  
}
```
Rõ ràng trên ví dụ trên qua thời gian session sẽ tăng lên mãi nhưng không được xóa đi bớt.
#### Cách khắc phục
**1.Sử dụng `WeakHashMap (Auto-cleaning)`** 

```java 
public class SessionCache {  
	// WeakHashMap allows GC to remove unused entries  
	private static final Map<Long, UserSession> cache = new WeakHashMap<>();  
  
	public void addUserSession(Long userId, UserSession session) {  
		cache.put(userId, session);  
	}  
	  
	public UserSession getSession(Long userId) {  
		return cache.get(userId);  
	}  
}
```

Sử dụng `WeakHashMap` sử dụng weak references, vì thế nên với những UserSession nào không sử dụng nữa GC có thể dọn dẹp chúng.

**2. Xóa thủ công (Phù hợp cho Controller caching)**

```java
public class SessionCache {  
	private static final Map<Long, UserSession> cache = new HashMap<>();  
	  
	public void addUserSession(Long userId, UserSession session) {  
		cache.put(userId, session);  
	}  
	  
	public UserSession getSession(Long userId) {  
		return cache.get(userId);  
	}  
	  
	// Manually remove sessions when no longer needed  
	public void removeSession(Long userId) {  
		cache.remove(userId);  
	}  
}
```

#### Example Unclose Resource
**Cách tạo lỗi**
```java
public void readFile(String path) {  
	try {  
		BufferedReader reader = new BufferedReader(new FileReader(path));  
		String line = reader.readLine(); // What if an exception occurs?  
		// ... process file ...  
		// Missing: reader.close();  
	} catch (IOException e) {  
		e.printStackTrace();  
	}  
}
```
Trường hợp này xảy ra nếu có lỗi xuất hiện `Reader` sẽ không bao giờ được đóng gây ra leak

**Fix bằng auto closing resource**
Sử dụng try với resource `try(BufferedReader reader = new BufferedReader(new FileReader(path))` .
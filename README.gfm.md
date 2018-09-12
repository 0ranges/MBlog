<div align="center">
    <h1>
        MbBlog
    </h1>
    <img src="pics/8.gif">
</div>

<!-- GFM-TOC -->
    * [How to Use](#how-to-use)
    * [����](#����)
        * [�ȵ�΢��](#�ȵ�΢��)
        * [Reids �� Memcache](#reids-��-memcache)
        * [Redis ����](#redis-����)
        * [ʵ��](#ʵ��)
        * [ҵ���ϵ�����](#ҵ���ϵ�����)
        * [���л���ʽ��ѡ��](#���л���ʽ��ѡ��)
    * [���Ӽܹ�](#���Ӽܹ�)
        * [���Ӹ���](#���Ӹ���)
        * [��д����](#��д����)
        * [���Ӹ�������](#���Ӹ�������)
    * [�������](#�������)
        * [��¼ʱ�첽�޸��û�ͷ��](#��¼ʱ�첽�޸��û�ͷ��)
        * [�첽ɾ��](#�첽ɾ��)
    * [���ܲ���](#���ܲ���)
<!-- GFM-TOC -->


## How to Use

- �û�����root

- ���룺123

## ����

### �ȵ�΢��

΢�����ݾ��ж���д�ٵ����ԣ����ֳ������ر��ʺϽ����ݽ��л��档MBlog ʹ�� Redis �����ȵ�΢�����ݡ�

### Reids �� Memcache

����Ŀ��ʼ��ʱ�������� Redis �� Memcache ��ѡ�����⣬����֮�����Ҫ�������£�

- Redis �и��õķֲ�ʽ֧�֣��� Memcache ֻ���ڿͻ���ʹ��һ���� Hash ��֧�ֲַ�ʽ��
- Redis ���� RDB ���պ� AOF ��־���ֳ־û����ԣ��� Memcache û�г־û����ơ�
- Redis �� Value ֧���������ͣ��� Memcache �� Value ֻ���� String��
- Redis �Ὣ�ܾ�û�õ� KV �����������ϣ��� Memchache ������һֱ���ڴ��С�
- Memcache Ϊ����ȫȥ��������Ƭ��Ӱ�죬���ڴ�ָ���ض����ȵĿ����洢���ݣ��������ַ�ʽ�������ڴ������ʲ��ߡ������Ĵ�СΪ 128 bytes��ֻ�洢 100 bytes �����ݣ���ôʣ�µ� 28 bytes ���˷ѵ��ˡ�

���ǵ���Ŀ��Ҫʹ�ö�̨����������������ѡ Redis����Ȼ Spring ������ Redis��ʹ�� @Cacheable ��ע��Ϳ���ʹ�� Redis ���л��棬����Ϊ���û�����ɿأ����ѡ���Լ�ʵ�ֻ��湦�ܡ�

### Redis ����

������Ҫ�� Redis �������ã���Ҫ���������棺�ڴ����ʹ�����Լ�������̭���ԡ�

�ڴ����ʹ�����ڷ������ܽ��ܵķ�Χ��Խ��Խ�ã�һ��Ҫ���ȵ����ݴ�һЩ����Ϊ Reids ����Ҫ�����洢���ݣ����д� Redis ���й��̵����ݡ�

Redis �����ֻ�����̭���ԣ�Ϊ��ѡ��һ���ʺ���Ŀ�Ĳ��ԣ���Ҫ�ȶ�ÿ�ֲ��Խ���һ���˽⡣

NoEviction �� TTL��Time to Live�����ʺϱ���Ŀ�Ļ���ϵͳ����Ϊ����̭�͸��ݹ���ʱ�������̭�����ܱ�֤���ڻ����е����ݶ�������ʹ�ȵ����ݡ�Random Ҳ�͹���ʱ����أ���������������޷���֤�ȵ����ݡ�LRU��least recently used�� ���Խ��������ʹ�õ����ݽ�����̭�����ʹ�ô���������ݱ���Ϊ���ȵ����ݣ���˽��������ʹ�õ�������̭֮�����ںܴ�̶��ϱ�֤�ڻ����е����ݶ����ȵ����ݡ�

|      ����       |                         ����                         |
| :-------------: | :--------------------------------------------------: |
|  volatile-lru   | �������ù���ʱ������ݼ�����ѡ�������ʹ�õ�������̭ |
|  volatile-ttl   |   �������ù���ʱ������ݼ�����ѡ��Ҫ���ڵ�������̭   |
| volatile-random |      �������ù���ʱ������ݼ�������ѡ��������̭      |
|   allkeys-lru   |       ���������ݼ�����ѡ�������ʹ�õ�������̭       |
| allkeys-random  |          ���������ݼ�������ѡ�����ݽ�����̭          |
|   noeviction    |                     ��ֹ��������                     |

LRU ������ Redis �б�����������̭���ԣ����ںܶೡ�϶���ʹ�ã��������ϵͳ��ҳ���û��㷨����ʹ�� LRU��������Ϊҳ���û��㷨Ҳ�൱��һ��������̭�㷨��Java ����� LinkedHashMap ���Ա�������ֵ�Ե� LRU���� Java �����оͿ���ʹ�� LinkedHashMap ��ʵ�����ƵĻ�����̭���ܡ�

ʵ�� LRU ��ʵҲ�ܼ򵥣�����ͨ��һ��������ά��˳���ڷ���һ��Ԫ��ʱ���ͽ�Ԫ���Ƶ�����ͷ������ô����β����Ԫ�ؾ����������ʹ�õ�Ԫ�أ����Խ�����̭�������Լ�Ҳʵ����һ���򵥵� LRU��Դ�룺[LRU](https://github.com/CyC2018/Algorithm/blob/master/Caching/src/LRU.java)

### ʵ��

Ϊ��ʵ�ֻ��湦�ܣ���Ҫ�޸Ļ�ȡ΢�������΢����ʵ�ִ��롣

�ڻ�ȡ΢���Ĵ����У����ȴ� Redis �л�ȡ�������ȡʧ�ܾʹ����ݿ��л�ȡ��

���� BlogCacheDao ʵ���˻���Ļ�ȡ����ӹ��ܣ�CacheHitDao ������¼��������д�����δ���д���������Ϊ�˶�ϵͳ���м�أ��Ӷ��Ի�������Ż��������ܹ���ʱ���ֻ��洩͸�ͻ���ѩ�������⡣

�����΢�������ݿ��ͬʱҲҪ������ӵ� Redis �У�������Ϊ���ݿ�ʹ�õ������Ӽܹ���ʵ�ֵĶ�д���룬����ͬ��������Ҫһ����ʱ�䣬��һ��ʱ���������ݿ��ǲ�һ�µġ�����������͵������ݿ⣬��ô���޷���ȡ�����µ����ݡ������д��ͬʱ��������ӵ������У���ô���������ݵ�����Ͳ��ᷢ�͵��ӷ��������Ӷ�������������������ͬ���ڼ�Ĳ�һ�¡�

```java
@Override
public Blog getBlogByBlogId(int blogId)
{
    Blog blog = blogCacheDao.getBlog(blogId);
    if (blog != null) {
        cacheHitDao.hit();           /* �������� */
    } else {
        blog = blogMapper.selectByPrimaryKey(blogId);
        cacheHitDao.miss();          /* ����δ���� */
        blogCacheDao.addBlog(blog);  /* ���뻺�� */
    }
    return blog;
}

@Override
public void addBlog(int userId, String content)
{
    Blog blog = new Blog();
    blog.setUserid(userId);
    blog.setContent(content);
    blog.setPublishtime(new Date());
    blogMapper.insert(blog);
    blogCacheDao.addBlog(blog);
}
```

### ҵ���ϵ�����

���΢�����ݲ��ܱ��޸ģ���ô�Ϳ��Ա��⻺�治һ�µ����⣬�����Ļ��涼�ܱ�֤����Ч�Ļ��档

΢�����������Ǻܼ򵥵ģ��������֮�����д����·���һ�εĴ��۲���ܸߡ�����΢��������������ʧЧ�ԣ�Ҳ��������ֻ��ȥ�Ķ����ڵ�΢������ôһ���û���Ҫ�޸ĺܾ�֮ǰ��΢�����ݾ�û��̫������壬��Ϊ�������ܿ�����

����Ŀ��ҵ���Ͻ������У����ṩ�޸�΢���Ĺ��ܣ����ܴ�����ϵͳ�����ܡ�

���п��Է��֣���ʱ���޷��˷��ļ������⣬ͨ����ҵ���Ͻ��м򵥵��������������׾��ܽ����

### ���л���ʽ��ѡ��

��ʵ�� Redis ���湦��ʱ���ʼѡ��ʹ�� Java �Դ������л���ʽ��һ������ת�����ֽ�����Ȼ��洢�����Ǻ�����ʶ���������л��õ��������кܶ����ඨ������ݣ��ⲿ��������ȫû��Ҫ���뻺���У�ֻ��Ҫ�������ؼ��ֶ�ƴ�ӳ��ַ����洢���ɣ�ʵ�ִ������£�

```java
public static String writeBlogObject(Blog blog)
{
    StringBuilder s = new StringBuilder();
    s.append(blog.getUserid()).append(",");
    s.append(blog.getBlogid()).append(",");
    s.append(DateUtil.formatDate(blog.getPublishtime())).append(",");
    s.append(blog.getContent());
    return s.toString();
}

public static Blog readBlogObject(String s)
{
    Blog blog = new Blog();
    String[] token = s.split(",");
    blog.setUserid(Integer.valueOf(token[0]));
    blog.setBlogid(Integer.valueOf(token[1]));
    blog.setPublishtime(DateUtil.parseDate(token[2]));
    blog.setContent(token[3]);
    return blog;
}
```

Ϊ����֤�������л���ʽ��ʱ��Ϳռ��ϵĿ�����������������׼���ԣ����Դ����� com.cyc.benchmark.SerializeTest.java �У���Ϊ�Ƚϳ��Ͳ��������ˡ�

����ģ��һ��΢�����󣬴��һ����΢�����ݣ�Ȼ��ʹ�� Java �Դ������л���ʽ��ƴ�ӵķ�ʽ�ֱ����� 1000000 �ε����л��ͷ����л���ͳ�ƴ洢����Ҫ���ֽ�������ʱ�䣬���£�

|                | Java ���л� | �ֶ�ƴ�� |
| :------------: | :---------: | :------: |
| �洢�����ֽ��� |     298     |    48    |
|   ����ʱ��/s   |   14.301    |  4.113   |

���Է����ֶ�ƴ�ӷ�ʽʵ�ֵ����л���ʽ�������ڿռ��ϻ�����ʱ���϶��� Java �Դ������л���ʽҪ�úܶ࣬�����Ŀʹ���Լ�ʵ�ֵ��ֶ�ƴ�ӷ�����

���� JSON ���л���ʽ����Ϊ��Ҳ�洢���ֶε����ƣ���˺����׾���֪���ڿռ俪���ϱ��ֶ�ƴ�ӷ�ʽҪ�ߺܶࡣ



## ���Ӽܹ�

### ���Ӹ���

![](pics/5.png)

MySQL ���Ӹ�����Ҫ�漰�����̣߳�binlog �̡߳�I/O �̺߳� SQL �̡߳�

-   **binlog �߳�**  ���������������ϵ����ݸ���д��������ļ���binlog���С�
-   **I/O �߳�**  ����������������϶�ȡ��������־�ļ�����д���м���־�С�
-   **SQL �߳�**  �������ȡ�м���־���ط����е� SQL ��䡣

### ��д����

![](pics/6.png)

����������������д�����Լ����µĶ����󣬶��ӷ��������������������

��д���볣�ô���ʽ��ʵ�֣��������������Ӧ�ò㴫���Ķ�д����Ȼ�����ת�����ĸ���������

MySQL ��д������������ܵ�ԭ�����ڣ�

- ���ӷ�����������ԵĶ���д������̶Ȼ������������ã�
- �ӷ������������� MyISAM ���棬������ѯ�����Լ���Լϵͳ������
- �������࣬��߿����ԡ�

### ���Ӹ�������

#### ���������˺�

�����ӷ��������������ڸ��Ƶ��˺ţ������˺ű����� master-host �� slave-host ��������Ȩ��Ҳ����˵���µ�������Ҫ�����ӷ������϶�ִ��һ�Ρ�

```
mysql > grant all privileges on *.* to repl@'master-host' identified by 'password';
mysql > grant all privileges on *.* to repl@'slave-host' identified by 'password';
mysql > flush privileges;
```

��ɺ���ò���һ�����ӷ������Ƿ�����ͨ��

```
mysql -u repl -h host -p
```

#### ���� my.cnf �ļ�

��������

```
[root]# vi /etc/my.cnf

[mysqld]
log-bin  = mysql-bin
server-id = 10
```

�ӷ�����

```
[root]# vi /etc/my.cnf

[mysqld]
log-bin          = mysql-bin
server-id        = 11
relay-log        = /var/lib/mysql/mysql-relay-bin
log-slave-updates = 1
read-only         = 1
```

���� MySQL

```
[root]# service mysqld restart;
```

�������� MySQL�����ϵ������ļ���ʹ�õ����»��ߣ����� server_id��ʹ�����ַ�ʽ�ڵ�ǰ�汾�� MySQL �в���ʹ�á�

#### ��������

�Ȳ鿴���������Ķ������ļ�����

```
mysql > show master status;
```

```
+------------------+----------+--------------+------------------+
| File            | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000002 |      106 |              |                  |
+------------------+----------+--------------+------------------+
```

Ȼ�����ôӷ�������

```
mysql > change master to master_host='master-host',            > master_user='repl',
      > master_password='password',
      > master_log_file='mysql-bin.000002',
      > master_log_pos=0;
```

�ڴӷ��������������ƣ�

```
mysql > start slave
```

�鿴����״̬��Slave_IO_Running �� Slave_SQL_Running ���붼Ϊ Yes �ű�ʾ�ɹ���

```
mysql > show slave status\G;
*************************** 1. row ***************************
              Slave_IO_State: Waiting for master to send event
                  Master_Host:
                  Master_User: repl
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000002
          Read_Master_Log_Pos: 106
              Relay_Log_File: mysql-relay-bin.000006
                Relay_Log_Pos: 251
        Relay_Master_Log_File: mysql-bin.000002
            Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
            ...
```

## �������

### ��¼ʱ�첽�޸��û�ͷ��

Ϊ������û��ڵ�¼ʱ��ʹ�����飬ͨ�������û��������� focusout �¼��������¼�����ʱ��ͨ�� ajax �첽��ȡ�û�ͷ��

```js
var userNameInput = $("input[name=userName]");
userNameInput.focusout(function ()
{
    $.ajax({
            url: "getUserHeadPic.html",
            type: "post",
            dataType: "text",
            data: {
                userName: userNameInput.val()
            },
            success: function (headpic)
            {
                replaceHeadPic(headpic);
            }
    });
}
```

<div align="center">
    <img src="pics/9.gif">
</div>

### �첽ɾ��

���ǵ��û���ɾ������������ʣ�µ����ӣ���˲����첽�ķ�ʽ����ɾ����ɾ��֮����Ҫˢ��ҳ�档

ɾ���ɹ�֮�󣬻Ὣ���������ء�Ϊ�˸��õ����飬���ع���������һ�� 200 ������ӳ٣��Ӷ�����һ�����ݵ����ض���Ч����

```js
var deleteBlog = $("#delete-blog");

deleteBlog.on("click", function ()
{
    var blogid = deleteBlog.attr("blogid");
    var blogDiv = $("#blog-" + blogid.toString());
    $.ajax({
        url: "editBlog.html",
        type: "post",
        dataType: "text",
        data: {
            blogId: blogid
        },
        success: function ()
        {
            blogDiv.hide(200);
        }
    });
})
```





<div align="center">
    <img src="pics/10.gif">
</div>

## ���ܲ���

ʹ�� Apache �� ab ����������ѹ�����ԡ�

Ϊ�˷�ֹ����ʱ�ӵ�Ӱ�죬����ڷ����������� ab ���߽��в��ԡ�

ʹ������������ʹ�� ab ���ߣ����� -c ����Ϊ��������-n ����Ϊ��������-k ������ʾ�־����ӣ�http://localhost/dblog ���Ǵ����Ե���վ��

```
ab -c 1000 -n 5000 -k http://localhost/dblog
```

��ʹ�� Redis ���л����Լ�ʹ�����Ӽܹ���ʵ�ֶ�д����֮ǰ���������ϲ��Եõ��Ĳ��ֽ�����£����Կ�������ÿ��ƽ����������Ϊ 715.81��

```
Time taken for tests:  6.985 seconds
Total transferred:      2645529 bytes
HTML transferred:      1530306 bytes
Requests per second:    715.81 [#/sec] (mean)
```

����ʹ�� Redis �Լ����Ӽܹ�֮�󣬲��ԵĽ�����£�ÿ��ƽ�����������Լ���ߵ��� 4839.62������������վ����������

```
Time taken for tests:  1.033 seconds
Total transferred:      2696313 bytes
HTML transferred:      1559682 bytes
Requests per second:    4839.62 [#/sec] (mean)
```

# ��������

```
<dependency>
  <groupId>com.h2database</groupId>
  <artifactId>h2</artifactId>
</dependency>
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

```

# ����ʵ����

```Java
import java.io.Serializable;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

import lombok.Data;

@Entity
@Data
public class Config implements Serializable {

  private static final long serialVersionUID = 1L;

  private String name, description, value;

  @Id
  @GeneratedValue
  private long id;

  /**
   * ������
   */
  private long creator;
}
```

# ����Dao�� Repository ��

����ʹ�� `PagingAndSortingRepository` ���� `CrudRepository`��

������һ���������Ʋ��ҵķ�����

```
import org.springframework.data.repository.PagingAndSortingRepository;

import cn.xiaowenjie.beans.Config;

public interface ConfigDao extends PagingAndSortingRepository<Config, Long> {
  Config findByName(String name);
}
```

# �������ݿ�

����ʹ��h2���ݿ⣬�ڴ��͵�ʱ��urlΪ `jdbc:h2:mem:mydb` �� �ļ�����Ϊ��`jdbc:h2:file:~/mydb.h2`

```
spring:
  profiles:
    active: dev
  redis:
    host: localhost
    port: 6379
  datasource:
    url: jdbc:h2:file:~/mydb.h2
    #url: jdbc:h2:mem:mydb
    username: sa
    password: sa
    driverClassName: org.h2.Driver
  jpa:
    database: h2
    show-sql: true
    hibernate:
      ddl-auto: update
  h2:
    console:
      enabled: true
      path: /h2-console
      settings:
        web-allow-others: true
        trace: true
```

# ����guava�� �Ѳ�ѯ���תΪlist

ʹ��jpa��ѯ����У����ص�Ϊ iterable����Ҫת��Ϊlist���ظ�ǰ̨��

```
<dependency>
  <groupId>com.google.guava</groupId>
  <artifactId>guava</artifactId>
  <version>16.0.1</version>
</dependency>
```

```Java
import com.google.common.collect.Lists;

public Collection<Config> getAll() {
  // У��ͨ�����ӡ��Ҫ����־
  logger.info("getAll start ...");

  List<Config> data = Lists.newArrayList(dao.findAll());

  logger.info("getAll end, data size:" + data.size());

  return data;
}
```

# ʹ��h2��web console

���� http://127.0.0.1:8080/h2-console/

![](/pictures/h2-1.png)

����ѡ�����Ľ��棬�ޣ���д��url�����ӽ�ȥ��


![](/pictures/h2-2.png)

ʹ���ļ����ͺ󣬻����û���Ŀ¼�´������ݿ��ļ���trace�ļ��������ı���Ϊ���ݿ�����һЩ��־��ջ��


![](/pictures/h2-3.png)

>ע�⣺�����ȶ���ʱ��ע��ѿ���̨�ص���

# ʹ��jpa

ֱ��ע�� Repository `ConfigDao` ������ο����̴��� `ConfigService`��

```Java
package cn.xiaowenjie.services;

import static cn.xiaowenjie.common.utils.CheckUtil.check;
import static cn.xiaowenjie.common.utils.CheckUtil.notEmpty;
import static cn.xiaowenjie.common.utils.CheckUtil.notNull;

import java.util.Collection;
import java.util.List;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.google.common.collect.Lists;

import cn.xiaowenjie.beans.Config;
import cn.xiaowenjie.daos.ConfigDao;

/**
 * ����ҵ������
 * 
 * @author Ф�Ľ� https://github.com/xwjie/
 *
 */
@Service
public class ConfigService {

  private static final Logger logger = LoggerFactory.getLogger(ConfigService.class);

  @Autowired
  ConfigDao dao;

  public Collection<Config> getAll() {
    // У��ͨ�����ӡ��Ҫ����־
    logger.info("getAll start ...");

    List<Config> data = Lists.newArrayList(dao.findAll());

    logger.info("getAll end, data size:" + data.size());

    return data;
  }

  public long add(Config config) {
    // ����У��
    notNull(config, "param.is.null");
    notEmpty(config.getName(), "name.is.null");
    notEmpty(config.getValue(), "value.is.null");

    // У��ͨ�����ӡ��Ҫ����־
    logger.info("add config:" + config);

    // У���ظ�
    check(null == dao.findByName(config.getName()), "name.repeat");

    config = dao.save(config);

    // �޸Ĳ�����Ҫ��ӡ�������
    logger.info("add config success, id:" + config.getId());

    return config.getId();
  }

  public boolean delete(long id) {
    // ����У��
    check(id > 0L, "id.error", id);

    dao.delete(id);

    // �޸Ĳ�����Ҫ��ӡ�������
    logger.info("delete config success, id:" + id);

    return true;
  }
}

```

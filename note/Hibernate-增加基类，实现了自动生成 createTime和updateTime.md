# ���ӻ��࣬ʵ�����Զ����� createTime��updateTime

# ���ӻ���

��id�ʹ����޸�ʱ��ŵ��������档��Ҫʹ�� `@MappedSuperclass` ע�⡣

ʹ�� `@CreationTimestamp`  �� `@UpdateTimestamp` �Զ����ɶ�Ӧ��ʱ�䡣

```Java
package cn.xiaowenjie.beans;

import java.io.Serializable;
import java.util.Date;

import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.MappedSuperclass;

import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.UpdateTimestamp;

import lombok.Data;

/**
 * ����
 * 
 * @author Ф�Ľ� https://github.com/xwjie/
 */
@Data
@MappedSuperclass
public abstract class BaseEntity implements Serializable {

	@Id
	@GeneratedValue
	private long id;

	@CreationTimestamp
	private Date createTime;

	@UpdateTimestamp
	private Date updateTime;

}

```
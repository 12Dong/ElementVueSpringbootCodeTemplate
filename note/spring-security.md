# ��������

```XML
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```

# response ��������ͷ

```
X-Content-Type-Options:nosniff
X-Frame-Options:DENY
X-XSS-Protection:1; mode=block
```

# ָ����֤��ʽ

ʹ��basic��֤����ͨ��ҳ��������֤��ֻ��ѡһ�֣����򷵻�401��ʱ�򻹻��ض��򵽵�¼ҳ�档

* basis ��֤

```
.httpBasic().and()
```

* ҳ����֤

```Java
.formLogin().loginPage("/login").permitAll().and()
```

������

```Java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
    	// �ȷſ�
    	http.authorizeRequests().anyRequest().permitAll();
    	
//        http
//            .authorizeRequests()
//                .antMatchers("/", "/home","/resources/**", "/favicon.ico").permitAll()
//                .anyRequest()
//                .authenticated()
//                .and()
//                // basic����401 �������ܡ�����loginpage
//                .httpBasic()
//                .and()
//              // ����http��֤��ʱ�򣬷���302����Ҫ����loginPage����
////            .formLogin()
////                .loginPage("/login")
////                .permitAll()
////                .and()
//            .logout()
//                .permitAll();
        
        //http.csrf().disable();
    }

    @Autowired
    public void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth
            .inMemoryAuthentication()
                .withUser("xwjie").password("xwjie").roles("USER");
    }
}
```


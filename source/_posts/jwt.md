---
title: jwt
---

## 什么是jwt

​	JSON Web Token（JWT）是一个非常轻巧的规范。这个规范允许我们使用JWT在用户和服务器之间传递安全可靠的
信息。在Java世界中通过JJWT实现JWT创建和验证。

## jjwt快速尝鲜

1. 引入坐标

   ```xml
   <dependency>
   	<groupId>io.jsonwebtoken</groupId>
   	<artifactId>jjwt</artifactId>
   	<version>0.6.0</version>
   </dependency>
   ```

   上面使用的坐标是基于apache2.0 下的jwt实现

2. 使用并测试

   生成token

   ```java
   JwtBuilder jwt = Jwts.builder()
                   .setId("123")
                   .setSubject("老王")
                   .setIssuedAt(new Date())//发布时间
                   .setExpiration(new Date(System.currentTimeMillis()+6000))//到期时间
                   .signWith(SignatureAlgorithm.HS384,"XXXXX");//设置密钥
   String compact = jwt.compact();//签约生成token
   System.out.println(compact);
   ```

   解析token

   ```java
   Claims cla = Jwts.parser()
       .setSigningKey("XXXXX")//设置密钥
       .parseClaimsJws(key)//设置token
       .getBody();
   ```

   当token认证失败时会抛出ExpiredJwtException异常

   自定义claims

   生成

   ```java
   //为了方便测试，我们将过期时间设置为1分钟
   long now = System.currentTimeMillis();//当前时间
   long exp = now + 1000*60;//过期时间为1分钟
   JwtBuilder builder= Jwts.builder().setId("888")
   .setSubject("小白")
   .setIssuedAt(new Date())
   .signWith(SignatureAlgorithm.HS256,"itcast")
   .setExpiration(new Date(exp))
   .claim("roles","admin") //自定义claims存储数据
   .claim("logo","logo.png");
   System.out.println( builder.compact() );
   ```

   解析

   ```java
   String
   compactJws="eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiI4ODgiLCJzdWIiOiLlsI_nmb0iLCJpYXQiOjE1MjM0MT
   czMjMsImV4cCI6MTUyMzQxNzM4Mywicm9sZXMiOiJhZG1pbiIsImxvZ28iOiJsb2dvLnBuZyJ9.b11p4g4rE94r
   qFhcfzdJTPCORikqP_1zJ1MP8KihYTQ";
   Claims claims =
   Jwts.parser().setSigningKey("itcast").parseClaimsJws(compactJws).getBody();
   System.out.println("id:"+claims.getId());
   System.out.println("subject:"+claims.getSubject());
   System.out.println("roles:"+claims.get("roles"));
   System.out.println("logo:"+claims.get("logo"));
   SimpleDateFormat sdf=new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
   System.out.println("签发时间:"+sdf.format(claims.getIssuedAt()));
   System.out.println("过期时间:"+sdf.format(claims.getExpiration()));
   System.out.println("当前时间:"+sdf.format(new Date()) );
   ```

   
---
title: mybatis-collection-with-only-size-one
date: 2018-04-22 00:02:29
tags: mybatis, qna
---

Table A
| id | a_1 | a_2|
|---|---|---|
|1  | aaa | bbb|

Table B
| id | a_id | b_1 | b_2|
|---|---|---|---|
| 1  | 1 | ccc| ddd|
| 2  | 1 | eee| fff|

```xml
<resultMap id="fileInfo" type="com.phynx.fuds.beans.dto.FileInfo" autoMapping="true">
    <id column="id" property="id" jdbcType="BIGINT" javaType="java.lang.Long"/>
    <result column="a_1" property="a1" jdbcType="VARCHAR" javaType="java.lang.String"/>
    <result column="a_2" property="a2" jdbcType="VARCHAR" javaType="java.lang.String"/>
    <collection property="chunks" ofType="com.phynx.fuds.beans.dto.FileChunk" javaType="java.util.ArrayList" autoMapping="true">
        <id column="id" property="id" jdbcType="BIGINT" javaType="java.lang.Long"/>
        <result column="a_id" property="fileKey" jdbcType="BIGINT" javaType="java.lang.Long"/>
        <result column="b_1" property="a1" jdbcType="VARCHAR" javaType="java.lang.String"/>
        <result column="b_2" property="a2" jdbcType="VARCHAR" javaType="java.lang.String"/>
    </collection>
</resultMap>
<select id="getDetail" resultMap="fileInfo">
    SELECT a.id, a.a_1, a.a_2, b.a_id, b.b_1, b.b_2
    FROM a, b
    WHERE a.id=b.a_id
</select>
```
The result only contains a collection with size 1.

After check this is because I miss to config b.id as bid, and then the a.id is regard as the id of table B.



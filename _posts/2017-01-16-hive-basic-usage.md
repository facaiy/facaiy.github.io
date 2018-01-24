---
layout: post
title: "Hive示例整理"
description: ""
category: misc
tags: []
modify: 2017-01-16 08:00:00
---

update: 2017-01-16

by: 金凌，发才


### 常用操作
1. 查看表的属性

   ```sql
   show tblproperties yourTableName;
   show partitions tableName;
   ```

2. 建表

   ```sql
   CREATE EXTERNAL TABLE IF NOT EXISTS push_result_get_push_uid_tmp
   (is_default     string,
   mid             string,
   push_feed_uid   string)
   PARTITIONED BY (dt STRING)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
   STORED AS INPUTFORMAT 'org.apache.hadoop.mapred.TextInputFormat'
   OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
   LOCATION '/dw/obja/push_stat/push_result_get_push_uid_tmp';
   ```

3. 插表

   ```sql
   insert overwrite table tmp_uid partition (dt=20160515)
select a.push_uid from
   (select distinct push_uid from push_result_get_push_uid where dt=20160515 and length(push_uid)<=10)a
   left outer join
   (select distinct push_uid from push_result_get_push_uid where dt=20160418 and length(push_uid)<=10)b
   on a.push_uid=b.push_uid
   where b.push_uid is null;
   ```

4. 增加栏

   ```sql
   ALTER TABLE test1 ADD COLUMNS (access_count1 int,access_count2 date,access_count3 string, ...);
   ```

5. 修复表

   ```sql
   msck repair table push_data;
   ```

6. [创建多级目录](https://cwiki.apache.org/confluence/display/Hive/DynamicPartitions
)（动态分区）

   ```sql
   INSERT OVERWRITE TABLE T PARTITION (ds='2010-03-03', hr)
   SELECT key, value, ds, hr FROM srcpart WHERE ds is not null and hr>10;
   ```


### 基本设置

0. 设置队列

   ```sql
   set mapreduce.job.queuename=normal_production;
   ```

1. 设置内存

   ```sql
   set mapreduce.map.memory.mb=5120;
   set mapreduce.map.java.opts=-Xmx4608m;
   set mapreduce.reduce.memory.mb=5120;
   set mapreduce.reduce.java.opts=-Xmx4608m;
   set mapred.child.java.opts=-Xmx2048m;
   set mapred.reduce.tasks=50;
   set mapred.max.split.size=600000000;
   set mapred.min.split.size.per.node=600000000;
   set mapred.min.split.size.per.rack=600000000;
   ```
   [惯例](https://documentation.altiscale.com/heapsize-for-mappers-and-reducers): `mapreduce.{map|reduce}.java.opts = mapreduce.{map|reduce}.memory.mb x 0.9`

   ![内存分配图](https://documentation.altiscale.com/userfiles/1651/2522/ckfinder/images/MR_Mem_Alloc%20-%20java_opts%20vs%20mr.png?dc=201508172029-0)

2. [压缩结果数据](https://cwiki.apache.org/confluence/display/Hive/CompressedStorage)

   ```sql
   SET hive.exec.compress.output=true;
   -- SET mapred.output.compression.codec=org.apache.hadoop.io.compress.SnappyCodec;
   -- SET mapred.output.compression.type=BLOCK;
   -- SET io.seqfile.compression.type=BLOCK;
   ```

   > mapred.output.compression.type: I use block. This will make the compression slittable even for all compression formats (gzip, snappy, and bzip2) just make sure your using a splitable file format like sequence, RCFile, or Avro.

   [Common input format](http://comphadoop.weebly.com/)
   Compression format | Tool | Algorithm | File extention | Splittable
   --|--|--|--|--
   gzip | gzip | DEFLATE | .gz | No
   bzip2 | bizp2 | bzip2 | .bz2 | Yes
   LZO | lzop | LZO | .lzo | Yes if indexed
   Snappy | N/A | Snappy | .snappy | No

   ref: [Using Snappy for Hive Compression](http://www.cloudera.com/documentation/archive/cdh/4-x/4-3-0/CDH4-Installation-Guide/cdh4ig_topic_23_5.html)

3. 合并结果中的小文件

   ```sql
   set hive.merge.mapfiles=true;
   set hive.merge.mapredfiles=true;
   set hive.merge.size.per.task=256000000;
   set hive.merge.smallfiles.avgsize=256000000;
   ```

4. 自定义UDF

   ```sql
   add jar hive_udf.jar;
   create temporary function getJsonArray as 'com.weibo.hive.udtf.json.UDFGenerateJsonArray';
   ```

#京东云存储服务Java SDK
============
  [京东云存储服务](http://www.jcloud.com/)(Jingdong Storage Service)是京东商城对外提供海量、安全、低成本、高可用的
  云存储服务基础平台。开发者利用该工具包可以实现：
  1. 管理Bucket信息
  2. 上传与下载Object数据
  3. 生成可公开访问/私有访问的URL
  
##平台优势
  1.  <b>海量数据</b>，通过数据冗余、集中资源管理等方式将大规模的硬件整合为高可靠的海量虚拟存储资源，用户可以轻松享受多媒体分享网站、网盘、个人企业数据备份等海量数据的云存储服务。
  2.  <b>低成本</b>，无需提供服务器，也不用关心冗余系统，不必关心购买安装和维护用于数据资源的硬件，用户完全可以省下这笔成本，借助于JSS平台，就可以轻松地创建和管理数据资源。
  3.  <b>安全性</b>，平台采用数据隔离、访问控制策略来保证数据安全性，使用严格的安全措施，比如使用经过证明的加密算法对用户进行身份验证，有效防止用户信息和用户数据资源泄露。
  4.  <b>高可用性</b>，通过软件智能调度实现自动故障恢复来保证系统的高可用性，同时采用群集系统，快速消除单点故障,在任何时候都能够保证系统正常使用，对外提供云存储服务。

##使用  
  京东云存储服务Java SDK核心类为JingdongStorageService，开发者需通过该类提供的多种方法访问京东云存储服务。
###构建JingdongStorageService对象
  在使用Credential之前，需要创建Credential对象，该对象必须包含用户的AccessKeyId以及SecertAccessKeyId
```java
  Credential credential = new Credential("AccessKeyId", "SecertAccessKeyId");
```
  通过Credential对象来创建JingdongStorageService
```java
 JingdongStorageService service = new JingdongStorageService(credential);
```
JingdongStorageService对象内部维护一组HTTP连接池，在不使用该对象之前需要调用其shutdown方法关闭连接池，请注意，一旦调用shutdown方法，该对象就不能再次被使用，否则将会抛出异常。
```java
  service.shutdown();
```
###列出所有的Bucket
```java
  List<Bucket> list = service.listBucket();
  for(Bucket b:list) {
  	System.out.println(b.getName());
  }
```
###Bucket相关操作
创建一个Bucket，请注意，京东云存储所有的Bucket都是全局唯一的，每个用户最多只能创建100个Bucket，每个Bucket在京东云存储系统中都是唯一的，不能创建2个相同名字的Bucket。
```java
  service.bucket("bucketname").create();
```
```
删除Bucket,当Bucket中没有Object的时候，该Bucket才能被删除, 否则删除会失败。
```java
  service.bucket("bucketname").delete();
```
###Object相关操作
上传数据
```java
  service.bucket("bucketname").object("key").entity(new File("filename")).put();
```

下载数据
```java
  service.bucket("bucketname").object("key").get().toFile(new File("/export/test.txt"));

```
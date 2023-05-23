<!--
 * @Author: hcs
 * @Date: 2023-03-28 17:15:10
 * @LastEditTime: 2023-03-30 17:41:34
 * @LastEditors: Do not edit
 * @Description: Modify here please
 * @FilePath: \git_program\FEStudy\其他\JDBC.md
-->
### JDBC 基操


Connection conn = null;
Statement stmt = null;
ResultSet rs = null;
try {
    // 1 加载并注册 JDBC 驱动
    Class.forName("com.mysql.jdbc.Driver");
    // 2 创建数据库连接
    conn = DriverManager.getConnection(
            "jdbc:mysql://localhost:3306/myblog?useSSL=false&useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&allowPublicKeyRetrieval=true",
            "root",
            "123456"
    );
    // 3 创建 Statement 对象
    stmt = conn.createStatement();
    rs = stmt.executeQuery("select * from blogs");
    // 4 遍历查询结果
    while (rs.next()) {
        Integer id = rs.getInt(1);
        String title = rs.getString("title");
        String content = rs.getNString("content");
        System.out.println(title);
    }
} catch (Exception e) {
    e.printStackTrace();
} finally {
    // 5 关闭连接，释放资源
    if (rs != null) {
        rs.close();
    }
    if (stmt != null) {
        stmt.close();
    }
    if (conn != null && !conn.isClosed()) {
        conn.close();
    }
}


### 批处理
executeBatch()



### JDBC中Date日期对象的处理
在 JDBC 中， getDate 返回的是 java.sql.Date 对象， 继承自 java.util.Date, 所以在开发过程中，这两种是等价的

前端传入的日期 转化为 sql 的 data 数据进行存储
1 String 转换为 java.util.Date
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd")
myDate = sdf.parse("字符串data")


2 java.util.Date 转化为 java.sql.Date
long time = myDate.getTime()
java.sql.Date finDate = new java.sql.Date(time)


### 连接池
阿里巴巴 Druid

小知识
有的 工程师 喜欢将 Druid 配置中 初始值 跟最大值设置成一样， 这样初始就产生了这么多的资源， 最大限度的提供最优的数据库连接服务

initialSize=20
maxActive=20


### Apache  Commons DBUtils
  Apache Dbutils 大幅度简化数据提取过程
  // 事务
  propertyFile = new URLDecoder().decode(propertyFile, "UTF-8");
  properties.load(new FileInputStream(propertyFile));
  DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
  conn = dataSource.getConnection();
  conn.setAutoCommit(false);
  String sql = "update blogs set title= 'title12333' where id=? ";
  QueryRunner qr = new QueryRunner();
  qr.update(conn, sql, new Object[]{4});
  conn.commit();
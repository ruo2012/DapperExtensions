# DapperExtensions
Dapper拓展方法，需要Dapper版本≥1.50,本类库基于Dapper对五种数据库进行了拓展,分别为
SqlServer、MySQL、SQLite、PostgreSQL、Oracle(由于Oracle拓展后没有经过测试，有BUG还望指正)

使用方法
1、首先建立实体层
<pre>
<code>
using DapperExtensions;

Table("People")]  //表示表名为People，默认不填的话用类名作为表名
public class People
{
    [Key(true)]  //[Key(true)] 表示主键，并且是自增。 [Key(false)] //表示主键，非自增。
    public int id { get; set; } 
    public string name { get; set; } //这些不标注的属性，表示数据库字段
    public int sex { get; set; }
    public string age { get; set; }
    
    [Computed] //[Computed] 表示这个属性为非数据库字段，在拓展方法Insert Update等中，就不会把该属性传递进去
    public string ok { get; set; }
    [Computed]
    public string address { get; set; }
    [Computed]
    public string xxx { get; set; }
}
</code>
</pre>
2、开始使用，五种数据库拓展的命名空间分别为
<pre>
<code>
using DapperExtensions.SqlServerExt;
using DapperExtensions.MySQLExt;
using DapperExtensions.SqLiteExt;
using DapperExtensions.PostgreSQLExt;
using DapperExtensions.OracleExt;

选择你要访问的数据库进行命名空间的引用，这边我以Sqlserver为例子

using DapperExtensions.SqlServerExt;

public static IDbConnection GetConn() //获取sql数据库连接，这边你可以用MySql、SQLlite等五种数据库Connection
{
    string str = "server=.;database=test;uid=sa;pwd=123";
    SqlConnection conn = new SqlConnection(str);
    conn.Open();
    return conn;
}

using (var conn = GetConn()) 
{
    People p = new People();
    p.name = "张三";
    p.sex = 100;
    p.age = "我100岁了";
    dynamic result = conn.Insert(p); //拓展了Insert方法
    p = conn.GetById<People>(1);  //GetById方法
}
</code>
</pre>
对于conn总共进行了30几个拓展方法，修改，删除，查询，分页等等。就不一一演示了。

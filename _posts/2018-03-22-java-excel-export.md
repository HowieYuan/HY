---
layout:     post                    # 使用的布局（不需要改）
title:      Java 导入数据到Excel并提供文件下载接口              # 标题 
subtitle:   #副标题
date:       2018-03-22              # 时间
author:     Howie                      # 作者
header-img: img/shiro springboot.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Java
    - Excel

---
>最近的项目中遇到了一个将数据库的信息导入到一个 Excel 文件的需求，而且还要提供下载该 Excel 文件的接口 ，搞定之后，进行了一下总结，希望给大家带来帮助
>
> 源码： https://github.com/HowieYuan/Excel-Download

# 依赖
```
<!-- https://mvnrepository.com/artifact/net.sourceforge.jexcelapi/jxl -->
<dependency>
	<groupId>net.sourceforge.jexcelapi</groupId>
	<artifactId>jxl</artifactId>
	<version>2.6.12</version>
</dependency>
```
我们需要用到 jxl 包的类，而 jxl.jar 正是操作 excel 表格的工具类库，除了 jxl 以外，poi 包也是一个 操作 excel 的类库。
而对比两个包，jxl 更适用与数据量大的情况，而 poi 在数据量不高（大约5000以内）时，效率较高，但占用内存大，更容易内存溢出。

# 测试数据
```
private int id;
private String name;
private int age;
private String gender;
```
```
public List<Person> getPersonList() {
    List<Person> list = new ArrayList<>();
    list.add(new Person(1, "Howie", 20, "female"));
    list.add(new Person(2, "Wade", 25, "male"));
    list.add(new Person(3, "Duncan", 30, "male"));
    list.add(new Person(4, "Kobe", 35, "male"));
    list.add(new Person(5, "James", 40, "male"));
    return list;
}
```
# 1. 将数据全部导入到一张表格
```
//创建文件本地文件
//直接将文件创建在项目目录中
String filePath = "人员数据.xls";
File dbfFile = new File(filePath);
//使用 Workbook 类的工厂方法创建一个可写入的工作薄(Workbook)对象
WritableWorkbook wwb = Workbook.createWorkbook(dbfFile);
//如果文件不存在，则创建一个新的文件
if (!dbfFile.exists() || dbfFile.isDirectory()) {
    dbfFile.createNewFile();
}
//获得人员信息 list (PersonFactroy 类已经被依赖注入)
List<Person> list = personFactroy.getPersonList();
 //创建一个可写入的工作表
WritableSheet ws = wwb.createSheet("列表 1", 0); 
//添加excel表头
ws.addCell(new Label(0, 0, "序号"));
ws.addCell(new Label(1, 0, "姓名"));
ws.addCell(new Label(2, 0, "年龄"));
ws.addCell(new Label(3, 0, "性别"));
int index = 0;
for (Person person : list) {
    //将生成的单元格添加到工作表中
    //（这里需要注意的是，在Excel中，第一个参数表示列，第二个表示行）
    ws.addCell(new Label(0, index + 1, String.valueOf(person.getId())));
    ws.addCell(new Label(1, index + 1, person.getName()));
    ws.addCell(new Label(2, index + 1, String.valueOf(person.getAge())));
    ws.addCell(new Label(3, index + 1, person.getGender()));
    index++;
}
```
![导入数据后的 Excel 表](https://upload-images.jianshu.io/upload_images/8807674-d8d25996aa645bfb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2. 将数据导入到多个表格
```
//前面的代码一致

//每个工作表格最多存储2条数据（注：excel表格一个工作表可以存储65536条）
int mus = 2; 
//数据的总大小
int totle = list.size();
//总表格数
int avg = totle / mus + 1;
for (int i = 0; i < avg; i++) {
    //创建一个可写入的工作表
    WritableSheet ws = wwb.createSheet("列表" + (i + 1), i);  
    //添加excel表头
    ws.addCell(new Label(0, 0, "序号"));
    ws.addCell(new Label(1, 0, "姓名"));
    ws.addCell(new Label(2, 0, "年龄"));
    ws.addCell(new Label(3, 0, "性别"));
    int num = i * mus;
    int index = 0;
    for (int m = num; m < list.size(); m++) {
        //判断index == mus的时候跳出当前for循环
        if (index == mus) {
            break;
        }
        Person person = list.get(m);
        //将生成的单元格添加到工作表中
        //（这里需要注意的是，在Excel中，第一个参数表示列，第二个表示行）
        ws.addCell(new Label(0, index + 1, String.valueOf(person.getId())));
        ws.addCell(new Label(1, index + 1, person.getName()));
        ws.addCell(new Label(2, index + 1, String.valueOf(person.getAge())));
        ws.addCell(new Label(3, index + 1, person.getGender()));
        index++;
    }
}
```
这里是根据每个表的数据量来分，大家也可以根据其中一个属性等等来分成各个表格

![](https://upload-images.jianshu.io/upload_images/8807674-5a92ae9f28e59e5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/8807674-1e5ec1f1d2eb73c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 提供文件下载接口
该方法利用文件流来写入文件，方法类型为 void，不需要 return，除此之外，接口参数中需要添加上 HttpServletResponse 
```
    @RequestMapping(value = "/getExcel", method = RequestMethod.GET)
    public void createBoxListExcel(HttpServletResponse response) throws Exception {
        String filePath = "人员数据.xls";
        
        /**
         *  这部分是刚刚导入 Excel 文件的代码，省略
         */
        
        String fileName = new String("人员数据.xls".getBytes(), "ISO-8859-1");
        //设置文件名
        response.addHeader("Content-Disposition", "filename=" + fileName);
        OutputStream outputStream = response.getOutputStream();
        FileInputStream fileInputStream = new FileInputStream(filePath);
        byte[] b = new byte[1024];
        int j;
        while ((j = fileInputStream.read(b)) > 0) {
            outputStream.write(b, 0, j);
        }
        fileInputStream.close();
        outputStream.flush();
        outputStream.close();
    }
```
然后，我们直接在地址栏输入```localhost:8080/getExcel```既可立刻下载你的文件

注意： ```String fileName = new String("人员信息.xlsx".getBytes(), "ISO-8859-1");``` 我使用了 ISO-8859-1 编码，原因是 ISO-8859-1编码是单字节编码，向下兼容 ASCII，而 Header 中只支持 ASCII，传输的文件名必须是 ASCII




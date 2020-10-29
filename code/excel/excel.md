#基于poi对excel操作
##依赖：
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>4.1.2</version>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>4.1.2</version>
</dependency>
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml-schemas</artifactId>
    <version>4.1.2</version>
</dependency>
<dependency>
    <groupId>cn.afterturn</groupId>
    <artifactId>easypoi-spring-boot-starter</artifactId>
    <version>4.0.0</version>
</dependency>

//在工作表中的第2行，第3列创建单元格并赋值123
Sheet.getRow(1).createCell(2).setCellValue("123");
//获取第2行的单元格数量
Sheet.getRow(1).getLastCellNum()

合并完的单元格中的文字，实际上在该合并单元格的第一个，合并完之后的单元格数量并没有变

复制合并单元格
目标位置得在模板行之下，只复制一次模板
1.对应行下移
//移动完之后必须把新行中的每个cell create出来，否者无法赋样式，如果移动完之后直接生成excel是可以根据是shiftRows方法把行高也同步
但是因为要create cell,所以样式就不存在了，先createRow,再getRow
2.创建新产生行的单元格

3.对这些单元格进行行高设置
4.新生成的行循环createCell并根据对应模板行赋值赋样式 .setCellValue  .setCellStyle
5.根据模板行的单元格合并对应的单元格 （先合并还是先赋样式不影响，但是要注意合并单元格的空）
//复制合并单元格的位置参数
CellRangeAddress cellAddresses = s1.getMergedRegion(1).copy();
//对合并区域进行位移
cellAddresses.setFirstRow(8);
cellAddresses.setLastRow(8);
//进行新区域合并，但是单元格样式不会拷贝
s1.addMergedRegion(cellAddresses);

如果模板行是多行合并，要合并的区域范围也应该对应( +y,+y+x,+x)
需要的参数：模板行的行范围（列范围,还是说整行复制？）（如果不是规则形状？）,要生成的复制模板的起始x,y值,模板行中的合并单元格集合（缺点：不知道模板范围中的合并单元格的下标）

合并之后的单元格比如第一个和第二个合并后getCell(1)取出来的是空的，合并单元格的左上角cell有值，其他应该是空的
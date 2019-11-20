# 1. 第三方提供

```shell
# Java操作Excel文件的两种Jar包选择
1. Jxl：Jxl是一个开源的Java Excel API项目，通过Jxl，Java可以很方便的操作微软的Excel文档；
2. POI：POI是Apache软件基金会的开放源码函式库，POI提供API给Java程序对Microsoft Office格式档案读和写的功能。
'区别：Jxl使用方便，POI使用复杂；功能方面Jxl相对POI弱。'
```

# 2. 下载POI

```xml
<!-- 操作Microsoft文件工具 -->
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi</artifactId>
    <version>4.1.1</version>
</dependency>
```

# 3. 包说明

| 包名 | 描述                                             |
| ---- | ------------------------------------------------ |
| HSSF | 提供读写Microsoft Excel XLS格式档案的功能        |
| XSSF | 提供读写Microsoft Excel OOXML XLSX格式档案的功能 |
| HWPF | 提供读写Microsoft Word DOC格式档案的功能         |
| HSLF | 提供读写Microsoft PowerPoint格式档案的功能       |
| HDGF | 提供读Microsoft Visio格式档案的功能              |
| HPBF | 提供读Microsoft Publisher格式档案的功能          |
| HSMF | 提供读Microsoft Outlook格式档案的功能            |

# 4. HSSF常用类

> 每个excel文件都会对应如下多个表单

![1571990859310](image\1571990859310.png)

| **类名**           | **说明**             |
| ------------------ | -------------------- |
| HSSFWorkbook       | Excel的文档对象      |
| HSSFSheet          | Excel的表单          |
| HSSFRow            | Excel的行            |
| HSSFCell           | Excel的格子单元      |
| HSSFFont           | Excel字体            |
| HSSFDataFormat     | 格子单元的日期格式   |
| HSSFHeader         | Excel文档Sheet的页眉 |
| HSSFFooter         | Excel文档Sheet的页脚 |
| HSSFCellStyle      | 格子单元样式         |
| HSSFDateUtil       | 日期                 |
| HSSFPrintSetup     | 打印                 |
| HSSFErrorConstants | 错误信息表           |

# 5. ExcelUtil

> 针对每行数据对应一个实体

```java
package com.bigape.studyExcel.utils;

import com.bigape.studyExcel.dao.entity.DataDictionary;
import org.apache.poi.hssf.usermodel.HSSFSheet;
import org.apache.poi.hssf.usermodel.HSSFWorkbook;
import org.apache.poi.ss.usermodel.Cell;
import org.apache.poi.ss.usermodel.Row;

import java.io.*;
import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.*;

/**
 * @auther zhangHongLu
 * @date 2019/10/25 0025 09:23
 */
public class ExcelUtil {
    /** excel文件后缀 */
    public static final String EXCEL_SUFFIX=".xls";

    /**
     * 创建Excel
     * @param excelCreatePath
     * @param excelName
     */
    public static void createExcelObject(String excelCreatePath,String excelName,String formName)  {
        //1. 验证目录是否存在
        File file = new File(excelCreatePath);
        if (!file.exists()){
            file.mkdirs();
        }
        //2. 创建excel文档对象
        HSSFWorkbook excelFile=new HSSFWorkbook();
        //3. 通过excel对象创建表单
        excelFile.createSheet(formName);
        //4. 获取输出流
        FileOutputStream out=null;
        String filePath=excelCreatePath+File.separator+excelName+EXCEL_SUFFIX;
        try {
             out= new FileOutputStream(filePath);
            //5. 保存Excel文件
             excelFile.write(out);
        }catch (Exception e){
             e.printStackTrace();
        }finally {
            try {
                out.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 自定义异常
     * @param msg
     */
    public static void MyException(String msg){
        try {
            throw new Exception(msg);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 读取Excel，并返回Excel对象,该方法只针对一行属性对应一个实体
     * @param excelPath
     * @return
     */
    public static HSSFWorkbook readExcel(String excelPath){
        //1. 验证路径
        if (!checkExcelPath(excelPath)) return null;
        //2. 创建输入流
        FileInputStream fileInputStream=null;
        try {
            fileInputStream = new FileInputStream(excelPath);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }
        //3. 读取并返回excel对象
        try {
            return new HSSFWorkbook(fileInputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                fileInputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return null;
    }

    /**
     * 解析excel
     * @param excel
     * @param zClass
     * @param <T>
     */
    public static <T> List<T> analysisExcel(HSSFWorkbook excel,Class<T> zClass){
        List<T> list=new ArrayList<>();
        if (excel!=null){
            try {
                //1. 获取表单数量
                int numberOfSheets = excel.getNumberOfSheets();
                //2. 循环获取每一个表单
                for (int i = 0; i < numberOfSheets; i++) {
                    HSSFSheet sheet = excel.getSheetAt(i);
                    //3. 得到表单的所有行：第一行对应数据库的字段
                    Iterator<Row> rowList = sheet.iterator();
                    //4. 拿到第一行与实体类属性对应
                    Row row = rowList.next();
                    Iterator<Cell> attrCell = row.cellIterator();
                    //5. 创建列与属性的映射
                    Map<Integer, String> indexToAttrMap = colAndAttrMapping(zClass, attrCell);
                    //7.遍历剩余的部分
                    col2Entity(zClass, list, rowList, indexToAttrMap);
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
            return list;
        }else {
            return null;
        }

    }

    /**
     * 每行每列的值映射到实体中，并添加到list集合
     * @param zClass 类的类对象
     * @param list    集合
     * @param rowList
     * @param indexToAttrMap
     * @param <T>
     */
    public static <T> void col2Entity(Class<T> zClass, List<T> list, Iterator<Row> rowList, Map<Integer, String> indexToAttrMap) {
        while (rowList.hasNext()){
            Row attrRow = rowList.next();
            // 创建实体对象
            try {
                T obj = zClass.newInstance();
                // 从行中取数据放入实体中
                for (Map.Entry<Integer, String> entry : indexToAttrMap.entrySet()) {
                    Cell cell = attrRow.getCell(entry.getKey());
                    String value = cell.getStringCellValue();
                    Field field=null;
                    try {
                         field= zClass.getDeclaredField(entry.getValue());
                         field.setAccessible(true);
                    } catch (NoSuchFieldException e) {
                        e.printStackTrace();
                    }
                    field.set(obj,value);
                    System.out.println(field.get(obj));
                }
                list.add(obj);
            } catch (InstantiationException e) {
                e.printStackTrace();
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 首列属性映射
     * @param zClass
     * @param attrCell
     * @param <T>
     * @return
     */
    public static <T> Map<Integer, String> colAndAttrMapping(Class<T> zClass, Iterator<Cell> attrCell) {
        Map<Integer,String> indexToAttrMap=new HashMap<>();
        while (attrCell.hasNext()){
            Cell cell = attrCell.next();
            String attrName = cell.getStringCellValue();
            try {
                //6. 判断实体类是否有这个属性
                if (zClass.getDeclaredField(attrName)!=null){
                    int index = cell.getColumnIndex();
                    indexToAttrMap.put(index,attrName);
                }
            } catch (NoSuchFieldException e) {
                e.printStackTrace();
            }
        }
        return indexToAttrMap;
    }

    /**
     * 检查Excel路径
     * @param excelPath
     * @return
     */
    public static boolean checkExcelPath(String excelPath) {
        if (excelPath==null || excelPath==""){
            return false;
        }
        File file = new File(excelPath);
        if (!excelPath.endsWith(EXCEL_SUFFIX)){
            MyException("文件格式不正确");
            return false;
        }
        if (!file.exists()){
            MyException("文件不存在");
            return false;
        }
        return true;
    }

    public static void main(String[] args) throws SQLException, ClassNotFoundException {
        String file="G:\\学习区\\java导入EXCE\\file\\theApe.xls";
        HSSFWorkbook excel = readExcel(file);
        List<DataDictionary> list = analysisExcel(excel, DataDictionary.class);
        String url="jdbc:mysql://106.52.164.198/xd_data";
        String user="root";
        String pwd="123456";
        Class.forName("com.mysql.jdbc.Driver");
        Connection connection = DriverManager.getConnection(url,user,pwd);
        String sql="insert into data_dictionary (parent_node_code, directory, dictionary_code, dictionary_name, dictionary_sort_no,node_code) values (?,?,?,?,?,?)";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        for (DataDictionary d : list) {
           preparedStatement.setString(1,d.getParentNodeCode());
           preparedStatement.setString(2,d.getDirectory());
           preparedStatement.setString(3,d.getDictionaryCode());
           preparedStatement.setString(4,d.getDictionaryName());
           preparedStatement.setInt(5,1);
           preparedStatement.setString(6,d.getNodeCode());
           int len=preparedStatement.executeUpdate();
        }

    }
}

```


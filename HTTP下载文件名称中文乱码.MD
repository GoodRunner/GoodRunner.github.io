### HTTP协议header中设置文件下载名中文文件名乱码

在做文件下载时，当文件名为中文时，经常会出现乱码现象，大体的原因就是header中只支持ASCII，所以我们传输的文件名必须是ASCII，当文件名为中文时，必须要将该中文转换成ASCII。

> 方法一：

将文件名称进行`ISO-8859-1`再编码

```
  @RequestMapping("/export")
  public void exportStudentStudyData(String classCode, String lessonId, HttpServletResponse response)throws IOException {

    String str = studentService.export(classCode, lessonId);

    response.setHeader("Content-Transfer-Encoding","binary");
    response.setHeader("Cache-Control", "must-revalidate, post-check=0, pre-check=0");
    response.setHeader("Pragma", "public");
    // 设置下载文件头及文件名称
    SimpleDateFormat sdft = new SimpleDateFormat("yyyyMMdd");
    response.setHeader("Content-Disposition", "attachment; filename="+ new String("学生数据".getBytes("UTF-8"),"ISO-8859-1") + sdft.format(new Date()) + ".csv");

    ServletOutputStream outPut = response.getOutputStream();

    outPut.write(str.getBytes());
    outPut.flush();

  }

```

> 方法二

可以对中文文件名使用url编码

```

  @GetMapping("/export")
  public void exportStudyData(Integer schoolId, String deptId, String subject, String grade, String area,
                              String classCode, String startTime, String endTime, HttpServletResponse response)throws Exception {
    String result = classService.exportLessonStudtyData(schoolId, deptId, subject, grade, area, classCode, startTime, endTime);

    response.setHeader("Content-Transfer-Encoding","binary");
    response.setHeader("Cache-Control", "must-revalidate, post-check=0, pre-check=0");
    response.setHeader("Pragma", "public");
    // 设置下载文件头及文件名称
    SimpleDateFormat sdft = new SimpleDateFormat("yyyyMMdd");
    response.setHeader("Content-Disposition", "attachment; filename=" + URLEncoder.encode("课次听课数据","UTF-8") + ".csv");
    ServletOutputStream outPut = response.getOutputStream();
    outPut.write(result.getBytes());
    outPut.flush();

  }

```

---
title: Datatables插件1.10.15版本服务器处理模式ajax获取分页数据实例解析
date: 2017-06-30 14:40:42
tags: [jquery, datatables]
---
## 一、问题描述
 
前端需要使用表格来展示数据，找了一些插件，最后确定使用dataTables组件来做。
 
后端的分页接口已经写好了，不能修改。接口需要传入页码(pageNumber)和页面显示数据条数(pageSize)，显示相应的数据。

<!-- more -->
 
## 二、分析
 
先来分析下分页实现。
 
**一是后端分页： 这种情况，请求的数据，后端返回的数据格式都按着官网来编码，很容易实现，在官网上有示例，不多说明。** 
**二是前端分页： 前端分页也是支持的，不过需要一次把所有数据都获取到才可以。**
 
看到这里，问题来了。由于后端在目前的情况下是更改不了，只能在前端实现。但是，现在 又不满足前端分页的条件 ：
 
一次性获取所有数据（现在后端数据接口只能返回相应页码的数据）。
 
介于目前的情况，获取的数据只有一页，没有所有的页码。 
试试能不能 **伪装一下后端分页的情况，就是开启后端分页，在请求之前，将传入的数据进行重组，在获取到数据后，将返回的数据按照后端分页的数据格式组装一遍。**
 
经过测试，是可以的。
 
## 三、实现
通过DataTables配置参数[ajax项](https://datatables.net/reference/option/ajax) 实现的
目前最新DataTables的版本是1.10.15版本 ，以下是使用此版本。
 
关于ajax详细介绍请看官方说明：[中文](http://www.w3school.com.cn/jquery/ajax_ajax.asp) | [英文](https://api.jquery.com/jquery.ajax/)
 
DataTables参数ajax接收三种类型的参数：
 
 
- *string： 设置获取数据的url
 
 
- *object：和 jQuery.ajax 定义类似
 
 
- *function：自定义获取数据的功能
 
 
直接上代码吧，都有注释。
 
### 前端页面代码：
 
```
 
<!DOCTYPE html>
<html lang="en">
 
<head>
  <meta charset="UTF-8">
  <title>jquery DataTables插件自定义分页ajax实现</title>
  <link href="http://cdn.bootcss.com/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet" media="screen">
  <link href="http://cdn.bootcss.com/datatables/1.10.11/css/dataTables.bootstrap.min.css" rel="stylesheet" media="screen">
  <link href="http://cdn.bootcss.com/datatables/1.10.11/css/jquery.dataTables.min.css" rel="stylesheet" media="screen">
</head>
 
<body>
  <div class="row-fluid">
    <h3>JQuery DataTables插件自定义分页Ajax实现</h3>
    <table id="example" class="display table-striped table-bordered table-hover table-condensed" cellspacing="0" width="100%">
      <thead>
        <tr>
          <th>编号</th>
          <th>姓名</th>
          <th>性别</th>
        </tr>
      </thead>
    </table>
  </div>
  <!-- jQuery -->
  <script type="text/javascript" charset="utf8" src="http://code.jquery.com/jquery-1.10.2.min.js"></script>
  <!-- DataTables -->
  <script type="text/javascript" charset="utf8" src="http://cdn.datatables.net/1.10.15/js/jquery.dataTables.js"></script>
  <script src="http://cdn.bootcss.com/datatables/1.10.11/js/jquery.js"></script>
  <script src="http://cdn.bootcss.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
  <script src="http://cdn.bootcss.com/datatables/1.10.11/js/jquery.dataTables.min.js"></script>
  <script src="http://cdn.bootcss.com/datatables/1.10.11/js/dataTables.bootstrap.min.js"></script>
  <script type="text/javascript">
  $(function() {
    //提示信息
    var lang = {
      "lengthMenu": "每页 _MENU_  项",
      "processing": "处理中...",
      "zeroRecords": "没有匹配结果",
      "info": "当前显示第 _START_ 到 _END_ 项, 共 _TOTAL_ 项",
      "infoEmpty": "暂无数据",
      "infoFiltered": "(由 _MAX_ 项结果过滤)", //当使用搜索功能后，表格主要信息出追加的字符
      "loadingRecords": "加载中...",
      "processing": "处理中...",
      "search": "搜索:",
      "infoPostFix": "", //追加到所有其他主要信息字符串之后
      "url": "",
      "emptyTable": "表中数据为空",
      "infoThousands": ",",
      "paginate": {
        "first": "首页",
        "last": "末页",
        "next": "下页",
        "previous": "上页"
      }
    };
 
 
    //初始化表格
    var table = $("#example").DataTable({
      language: lang, //提示信息
      stateSave: true, //状态保存，再次加载页面时还原表格状态
      autoWidth: false, //禁用自动调整列宽
      stripeClasses: ["odd", "even"], //为奇偶行加上样式，兼容不支持CSS伪类的场合
      processing: true, //隐藏加载提示,自行处理
      serverSide: true, //启用服务器端分页
      searching: false, //禁用原生搜索
      orderMulti: false, //启用多列排序
      order: [], //取消默认排序查询,否则复选框一列会出现小箭头
      deferRender: true, //延迟渲染可以提高Datatables的加载速度
      lengthMenu: [
        [10, 25, 50, 100, 300, -1],
        [10, 25, 50, 100, 300, "所有"]
      ], //每页多少项，第一个数组是表示的值，第二个数组用来显示
      renderer: "bootstrap", //渲染样式：Bootstrap和jquery-ui
      pagingType: "simple_numbers", //分页样式：simple,simple_numbers,full,full_numbers
      scrollY: 300, //表格的固定高
      scrollCollapse: true, //开启滚动条
      pageLength: 10, //首次加载的数据条数
      columnDefs: [{
       targets: 'nosort', //列的样式名
       orderable: false //包含上样式名‘nosort'的禁止排序
      }],
      //对应列表表头字段
      columns: [{
        "data": "Id"
      }, {
        "data": "Name"
      }, {
        "data": "Sex"
      }],
      ajax: function(data, callback, settings) {
        //封装请求参数
        var param = {};
        param.limit = data.length; //页面显示记录条数，在页面显示每页显示多少项的时候
        param.start = data.start; //开始的记录序号
        param.page = (data.start / data.length) + 1; //当前页码
        //console.log(param);
        //ajax请求数据
        $.ajax({
          url: "/hello/list",
          type: "POST",
          cache: false, //禁用缓存
          data: param, //传入组装的参数
          dataType: "json",
          contentType: 'application/json;charset=utf-8',
          beforeSend: function(request) {
            request.setRequestHeader("token", localStorage.token);
          },
          success: function(result) {
            //console.log(result);
            //setTimeout仅为测试延迟效果
            setTimeout(function() {
              //封装返回数据
              var returnData = {};
              returnData.draw = data.draw; //这里直接自行返回了draw计数器,应该由后台返回
              returnData.recordsTotal = result.total; //返回数据全部记录
              returnData.recordsFiltered = result.total; //后台不实现过滤功能，每次查询均视作全部结果
              returnData.data = result.data; //返回的数据列表
              //console.log(returnData);
              //调用DataTables提供的callback方法，代表数据已封装完成并传回DataTables进行渲染
              //此时的数据需确保正确无误，异常判断应在执行此回调前自行处理完毕
              callback(returnData);
            }, 200);
          }
        });
      },
 
    });
 
  });
  </script>
</body>
 
</html>
 
```
### JSON数据格式:
![](http://images2015.cnblogs.com/blog/564792/201706/564792-20170630143559118-923358726.png)

### 效果图:
![](http://images2015.cnblogs.com/blog/564792/201706/564792-20170630143543399-2105630772.png)

## 参考链接
[jQuery DataTables插件自定义Ajax分页实例解析](http://www.jb51.net/article/84751.htm)  
[Datatables插件1.10.15版本服务器处理模式ajax获取分页数据实例解析](http://www.cnblogs.com/weiqinl/p/7098746.html)
 
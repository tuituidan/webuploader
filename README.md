
基于百度`webuploader`封装的一个上传控件，解决了IE下使用`flash`上传一些问题，简化了调用过程。

**使用方式**：  

1. 页面引入css文件`webuploader.css`，js文件`jquery.min.js`、`webuploader.js`、`uploader.js`  
2. 在需要生成上传控件的地方添加html代码：`<div id="uploader" class="uploader"></div>`  
3. 生成控件js代码：`var upload = $("#uploader").createUploader(params);`或`或new UploadFile("upload",params)`

> params为json对象，设置上传控件初始化参数，参数如下（均有默认值，可选填）
>
> `extraData` ： 额外传到服务器的参数，默认{}，示例：{xxx:123,bbb:'ddg'}
> `fileList`：初始添加的文件，默认[]，示例：[{name:'name',ext:'doc',prevurl:'prevurl',filepath:'filepath'},{...}]
> `maxCount`：可添加的上传文件个数，默认9
> `submitName`：提交值的name属性值，默认"fileName"
> `serverUrl`：文件上传的服务地址，默认"/api/v1/upload"
> `acceptExt`：允许上传的文件格式，示例"jpg,jpeg,gif,png,bmp"
>
> > 支持格式包括：png,jpg,jpeg,gif,bmp,flv,swf,mkv,avi,rm,rmvb,mpeg,mpg,ogg,ogv,mov,wmv,mp4,webm,mp3,wav,mid,rar,zip,tar,gz,7z,bz2,cab,iso,doc,docx,xls,xlsx,ppt,pptx,pdf,txt,md,xml
>
> `autoload`：选择文件后是否自动上传，默认false,
>
> > true表示添加文件后自动上传，上传后得到的文件路径为上面设置的name为submitName的值的input标签的值。
> >
> > false表示不自动上传，则需要添加上传按钮进行上传，调用下面的代码可获得上传文件路径
> >
> > ```
> >  upload.uploadFile(function(vals) {
> >     console.log(vals);
> >  }); 
> > ```
> >
> > 
>
> `promptText`：显示在上传控件的下面的一行字，作为提示，提高用户体验，示例"提示：最多可同时上传3个文件，每个文件不超过100M。"

**封装说明**

- 支持设置初始值，如编辑商品的时候，商品图片已经上传了一些，就需要以新增时一样的方式显示出来。

- 根据传入的支持的文件格式自动生成`accept`属性，可在选择文件框中自动过滤掉不支持的格式。

- 自动检测`Uploader.swf`的路径。

  > 因为`Uploader.swf`传入的路径必须是相对于当前html页面的路径，但项目中各个上传页面路径都不一样，所以无法设置统一的值。

- 解决使用flash上传时，添加文件达到最大支持个数，隐藏上传按钮后，再移除文件后上传按钮不显示的问题。

  > 使用flash上传时，上传按钮不能用jq的hide方法（即css的display:none）来隐藏，需要用样式
  >
  > ```
  > .webuploader-element-invisible {
  > 	position: absolute !important;
  > 	clip: rect(1px 1px 1px 1px); 
  >     clip: rect(1px,1px,1px,1px);
  > }
  > ```
  >
  > 来控制

- 添加ocx解决IE下flash被禁用的问题。

  > 在IE低版本使用flash上传时，如果flash安装了只是被禁用了，webuploader源码中的方法也会检测为没安装（实际也是js是无法检测windows组件是否安装的），所以添加`Flash11e.ocx`来强行提示用户开启

- 下调flash支持版本为11.1

  > 源码中要求的flash版本至少为11.4，实际生产环境客户正好用的11.1，下调为11.1测试后发现仍然正常




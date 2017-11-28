---
layout : life
title: ExtJs-Grid数据导出为Excel
category : 前端学习
duoshuo: true
date : 2017-11-28
---

```
	作者 : LZHD
	日期 : 2017-11-28
	版本 : 0.0.1
```

<!-- more -->

### Grid数据导出为Excel（纯前端）

**1. gridToExcel工具函数**

```
    Ext.override(Ext.grid.GridPanel, {
        downloadExcelXml: function(includeHidden, title) {
    
            if (!title) title = this.title;
    
            var vExportContent = this.getExcelXml(includeHidden, title);
    
            var location = 'data:application/vnd.ms-excel;base64,' + Base64.encode(vExportContent);
    
            var isSupportDownload = 'download' in document.createElement('a');//当前浏览器是否支持download属性
    
            if (isSupportDownload) {
                var gridEl = this.getEl();
                var el = Ext.DomHelper.append(gridEl, {
                    id: 'downloadExcel',
                    tag: "a",
                    download: title + "-" + new Date().format('Y-m-d') + '.xls',
                    href: location
                });
                el.click();
                Ext.removeNode(Ext.fly('downloadExcel'));
            } else {
                document.location = location;
            }
    
        },
        getExcelXml: function(includeHidden, title) {
            this.title = title || this.title;
            var worksheet = this.createWorksheet(includeHidden, title);
            var totalWidth = this.getColumnModel().getTotalWidth(includeHidden);
            return '<?xml version="1.0" encoding="utf-8"?>' +
                '<ss:Workbook xmlns:ss="urn:schemas-microsoft-com:office:spreadsheet" xmlns:x="urn:schemas-microsoft-com:office:excel" xmlns:o="urn:schemas-microsoft-com:office:office">' +
                '<o:DocumentProperties><o:Title>' + this.title + '</o:Title></o:DocumentProperties>' +
                '<ss:ExcelWorkbook>' +
                '<ss:WindowHeight>' + worksheet.height + '</ss:WindowHeight>' +
                '<ss:WindowWidth>' + worksheet.width + '</ss:WindowWidth>' +
                '<ss:ProtectStructure>False</ss:ProtectStructure>' +
                '<ss:ProtectWindows>False</ss:ProtectWindows>' +
                '</ss:ExcelWorkbook>' +
                '<ss:Styles>' +
                '<ss:Style ss:ID="Default">' +
                '<ss:Alignment ss:Vertical="Top" ss:WrapText="1" />' +
                '<ss:Font ss:FontName="arial" ss:Size="10" />' +
                '<ss:Borders>' +
                '<ss:Border ss:Color="#e4e4e4" ss:Weight="1" ss:LineStyle="Continuous" ss:Position="Top" />' +
                '<ss:Border ss:Color="#e4e4e4" ss:Weight="1" ss:LineStyle="Continuous" ss:Position="Bottom" />' +
                '<ss:Border ss:Color="#e4e4e4" ss:Weight="1" ss:LineStyle="Continuous" ss:Position="Left" />' +
                '<ss:Border ss:Color="#e4e4e4" ss:Weight="1" ss:LineStyle="Continuous" ss:Position="Right" />' +
                '</ss:Borders>' +
                '<ss:Interior />' +
                '<ss:NumberFormat />' +
                '<ss:Protection />' +
                '</ss:Style>' +
                '<ss:Style ss:ID="title">' +
                '<ss:Borders />' +
                '<ss:Font />' +
                '<ss:Alignment ss:WrapText="1" ss:Vertical="Center" ss:Horizontal="Center" />' +
                '<ss:NumberFormat ss:Format="@" />' +
                '</ss:Style>' +
                '<ss:Style ss:ID="headercell">' +
                '<ss:Font ss:Bold="1" ss:Size="10" />' +
                '<ss:Alignment ss:WrapText="1" ss:Horizontal="Center" />' +
                '<ss:Interior ss:Pattern="Solid" ss:Color="#FFFFFF" />' +
                '</ss:Style>' +
                '<ss:Style ss:ID="even">' +
                '<ss:Interior ss:Pattern="Solid" ss:Color="#FFFFFF" />' +
                '</ss:Style>' +
                '<ss:Style ss:Parent="even" ss:ID="evendate">' +
                '<ss:NumberFormat ss:Format="[ENG][$-409]dd\-mmm\-yyyy;@" />' +
                '</ss:Style>' +
                '<ss:Style ss:Parent="even" ss:ID="evenint">' +
                '<ss:NumberFormat ss:Format="0" />' +
                '</ss:Style>' +
                '<ss:Style ss:Parent="even" ss:ID="evenfloat">' +
                '<ss:NumberFormat ss:Format="0.00" />' +
                '</ss:Style>' +
                '<ss:Style ss:ID="odd">' +
                '<ss:Interior ss:Pattern="Solid" ss:Color="#FFFFFF" />' +
                '</ss:Style>' +
                '<ss:Style ss:Parent="odd" ss:ID="odddate">' +
                '<ss:NumberFormat ss:Format="[ENG][$-409]dd\-mmm\-yyyy;@" />' +
                '</ss:Style>' +
                '<ss:Style ss:Parent="odd" ss:ID="oddint">' +
                '<ss:NumberFormat ss:Format="0" />' +
                '</ss:Style>' +
                '<ss:Style ss:Parent="odd" ss:ID="oddfloat">' +
                '<ss:NumberFormat ss:Format="0.00" />' +
                '</ss:Style>' +
                '</ss:Styles>' +
                worksheet.xml +
                '</ss:Workbook>';
        },
    
        createWorksheet: function(includeHidden, title) {
            // Calculate cell data types and extra class names which affect formatting
            this.title = title || this.title;
            var cellType = [];
            var cellTypeClass = [];
            var cm = this.getColumnModel();
            var totalWidthInPixels = 0;
            var colXml = '';
            var headerXml = '';
            var visibleColumnCountReduction = 0;
            var colCount = cm.getColumnCount();
            for (var i = 0; i < colCount; i++) {
                if ((cm.getDataIndex(i) != '')
                    && (includeHidden || !cm.isHidden(i))) {
                    var w = cm.getColumnWidth(i)
                    totalWidthInPixels += w;
                    if (cm.getColumnHeader(i) === ""){
                    	cellType.push("None");
                    	cellTypeClass.push("");
                    	++visibleColumnCountReduction;
                    }
                    else
                    {
                        colXml += '<ss:Column ss:AutoFitWidth="1" ss:Width="' + w + '" />';
                        headerXml += '<ss:Cell ss:StyleID="headercell">' +
                            '<ss:Data ss:Type="String">' + cm.getColumnHeader(i) + '</ss:Data>' +
                            '<ss:NamedCell ss:Name="Print_Titles" /></ss:Cell>';
                        var fld = this.store.recordType.prototype.fields.get(cm.getDataIndex(i));
                        switch(fld.type) {
                            case "int":
                                cellType.push("Number");
                                cellTypeClass.push("int");
                                break;
                            case "float":
                                cellType.push("Number");
                                cellTypeClass.push("float");
                                break;
                            case "bool":
                            case "boolean":
                                cellType.push("String");
                                cellTypeClass.push("");
                                break;
                            case "date":
                                cellType.push("DateTime");
                                cellTypeClass.push("date");
                                break;
                            default:
                                cellType.push("String");
                                cellTypeClass.push("");
                                break;
                        }
                    }
                }
            }
            var visibleColumnCount = cellType.length - visibleColumnCountReduction;
    
            var result = {
                height: 9000,
                width: Math.floor(totalWidthInPixels * 30) + 50
            };
    
            // Generate worksheet header details.
            var t = '<ss:Worksheet ss:Name="' + this.title + '">' +
                '<ss:Names>' +
                '<ss:NamedRange ss:Name="Print_Titles" ss:RefersTo="=\'' + this.title + '\'!R1:R2" />' +
                '</ss:Names>' +
                '<ss:Table x:FullRows="1" x:FullColumns="1"' +
                ' ss:ExpandedColumnCount="' + visibleColumnCount +
                '" ss:ExpandedRowCount="' + (this.store.getCount() + 2) + '">' +
                colXml +
                '<ss:Row ss:Height="38">' +
                '<ss:Cell ss:StyleID="title" ss:MergeAcross="' + (visibleColumnCount - 1) + '">' +
                '<ss:Data xmlns:html="http://www.w3.org/TR/REC-html40" ss:Type="String">' +
                '<html:B><html:U><html:Font html:Size="15">' + this.title +
                '</html:Font></html:U></html:B></ss:Data><ss:NamedCell ss:Name="Print_Titles" />' +
                '</ss:Cell>' +
                '</ss:Row>' +
                '<ss:Row ss:AutoFitHeight="1">' +
                headerXml +
                '</ss:Row>';
    
            // Generate the data rows from the data in the Store
            for (var i = 0, it = this.store.data.items, l = it.length; i < l; i++) {
                t += '<ss:Row>';
                var cellClass = (i & 1) ? 'odd' : 'even';
                r = it[i].data;
                var k = 0;
                for (var j = 0; j < colCount; j++) {
                    if ((cm.getDataIndex(j) != '')
                        && (includeHidden || !cm.isHidden(j))) {
                        var v = r[cm.getDataIndex(j)];
                        if (cellType[k] !== "None") {
                            t += '<ss:Cell ss:StyleID="' + cellClass + cellTypeClass[k] + '"><ss:Data ss:Type="' + cellType[k] + '">';
                            if (cellType[k] == 'DateTime') {
                                t += v.format('Y-m-d');
                            } else {
                                t += v;
                            }
                            t +='</ss:Data></ss:Cell>';
                        }
                        k++;
                    }
                }
                t += '</ss:Row>';
            }
    
            result.xml = t + '</ss:Table>' +
                '<x:WorksheetOptions>' +
                '<x:PageSetup>' +
                '<x:Layout x:CenterHorizontal="1" x:Orientation="Landscape" />' +
                '<x:Footer x:Data="Page &amp;P of &amp;N" x:Margin="0.5" />' +
                '<x:PageMargins x:Top="0.5" x:Right="0.5" x:Left="0.5" x:Bottom="0.8" />' +
                '</x:PageSetup>' +
                '<x:FitToPage />' +
                '<x:Print>' +
                '<x:PrintErrors>Blank</x:PrintErrors>' +
                '<x:FitWidth>1</x:FitWidth>' +
                '<x:FitHeight>32767</x:FitHeight>' +
                '<x:ValidPrinterInfo />' +
                '<x:VerticalResolution>600</x:VerticalResolution>' +
                '</x:Print>' +
                '<x:Selected />' +
                '<x:DoNotDisplayGridlines />' +
                '<x:ProtectObjects>False</x:ProtectObjects>' +
                '<x:ProtectScenarios>False</x:ProtectScenarios>' +
                '</x:WorksheetOptions>' +
                '</ss:Worksheet>';
            return result;
        }
    });
    
```
**2. Base64 encode/decode转换**
    
```
    /**
    *
    *  Base64 encode / decode
    *  http://www.webtoolkit.info/
    *
    **/
    var Base64 = {
        // private property
        _keyStr : "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=",
        // public method for encoding
        encode : function (input) {
            var output = "";
            var chr1, chr2, chr3, enc1, enc2, enc3, enc4;
            var i = 0;
            input = Base64._utf8_encode(input);
            while (i < input.length) {
                chr1 = input.charCodeAt(i++);
                chr2 = input.charCodeAt(i++);
                chr3 = input.charCodeAt(i++);
                enc1 = chr1 >> 2;
                enc2 = ((chr1 & 3) << 4) | (chr2 >> 4);
                enc3 = ((chr2 & 15) << 2) | (chr3 >> 6);
                enc4 = chr3 & 63;
                if (isNaN(chr2)) {
                    enc3 = enc4 = 64;
                } else if (isNaN(chr3)) {
                    enc4 = 64;
                }
                output = output +
                this._keyStr.charAt(enc1) + this._keyStr.charAt(enc2) +
                this._keyStr.charAt(enc3) + this._keyStr.charAt(enc4);
            }
            return output;
        },
        // public method for decoding
        decode : function (input) {
            var output = "";
            var chr1, chr2, chr3;
            var enc1, enc2, enc3, enc4;
            var i = 0;
            input = input.replace(/[^A-Za-z0-9\+\/\=]/g, "");
            while (i < input.length) {
                enc1 = this._keyStr.indexOf(input.charAt(i++));
                enc2 = this._keyStr.indexOf(input.charAt(i++));
                enc3 = this._keyStr.indexOf(input.charAt(i++));
                enc4 = this._keyStr.indexOf(input.charAt(i++));
                chr1 = (enc1 << 2) | (enc2 >> 4);
                chr2 = ((enc2 & 15) << 4) | (enc3 >> 2);
                chr3 = ((enc3 & 3) << 6) | enc4;
                output = output + String.fromCharCode(chr1);
                if (enc3 != 64) {
                    output = output + String.fromCharCode(chr2);
                }
                if (enc4 != 64) {
                    output = output + String.fromCharCode(chr3);
                }
            }
            output = Base64._utf8_decode(output);
            return output;
        },
        // private method for UTF-8 encoding
        _utf8_encode : function (string) {
            string = string.replace(/\r\n/g,"\n");
            var utftext = "";
            for (var n = 0; n < string.length; n++) {
                var c = string.charCodeAt(n);
                if (c < 128) {
                    utftext += String.fromCharCode(c);
                }
                else if((c > 127) && (c < 2048)) {
                    utftext += String.fromCharCode((c >> 6) | 192);
                    utftext += String.fromCharCode((c & 63) | 128);
                }
                else {
                    utftext += String.fromCharCode((c >> 12) | 224);
                    utftext += String.fromCharCode(((c >> 6) & 63) | 128);
                    utftext += String.fromCharCode((c & 63) | 128);
                }
            }
            return utftext;
        },
        // private method for UTF-8 decoding
        _utf8_decode : function (utftext) {
            var string = "";
            var i = 0;
            var c = c1 = c2 = 0;
            while ( i < utftext.length ) {
                c = utftext.charCodeAt(i);
                if (c < 128) {
                    string += String.fromCharCode(c);
                    i++;
                }
                else if((c > 191) && (c < 224)) {
                    c2 = utftext.charCodeAt(i+1);
                    string += String.fromCharCode(((c & 31) << 6) | (c2 & 63));
                    i += 2;
                }
                else {
                    c2 = utftext.charCodeAt(i+1);
                    c3 = utftext.charCodeAt(i+2);
                    string += String.fromCharCode(((c & 15) << 12) | ((c2 & 63) << 6) | (c3 & 63));
                    i += 3;
                }
            }
            return string;
        }
    }
```

**3. 使用方法**

实例
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <link rel="stylesheet" type="text/css" href="./js/extjs/css/ext-all.css"></link>
    <script type="text/javascript" src="./js/extjs/js/adapter/ext/ext-base.js"></script>
    <script type="text/javascript" src="./js/extjs/js/ext-all.js"></script>
    <script type="text/javascript" src="./js/extjs/js/ext-lang-zh_CN.js"></script>
    <script type="text/javascript" src="./js/gridToExcel.js"></script>
    <script type="text/javascript" src="./js/base64.js"></script>
</head>

<body>
    <div id="grid-example"></div>
    <script>
        Ext.onReady(function () {

            //array with data - dummy data
            var myData = [
                ['Meyers, Quyn R.', '(943) 570-5141', 'Proin@nullamagna.ca', '05/13/1990'],
                ['Whitney, Tad T.', '(547) 743-0343', 'vulputate@acurnaUt.org', '05/10/1987'],
                ['Lawrence, Flavia J.', '(404) 826-4553', 'dapibus.id@accumsan.ca', '01/05/1988'],
                ['Morales, Susan I.', '(276) 707-8084', 'tristique@seacmetus.com', '03/09/1992'],
                ['Merrill, Desiree Q.', '(911) 546-0559', 'dictum.cursus@vel.ca', '01/07/1981'],
                ['Hampton, Willa Y.', '(729) 562-8303', 'nascetur@stellus.ca', '06/17/1991'],
                ['Brewer, Brynne F.', '(818) 302-4393', 'ligula@ullamcorper.org', '04/20/1989'],
                ['Marsh, Drew D.', '(645) 671-2779', 'et.euismod.et@eget.ca', '02/13/1990']
            ];
            //data store - description of fields
            var store = new Ext.data.SimpleStore({
                fields: [
                    'name',
                    'phone',
                    'email',
                    {
                        name: 'birthday',
                        type: 'date',
                        dateFormat: 'd/m/Y'
                    }
                ]
            });

            store.loadData(myData);

            // create the Grid
            var grid = new Ext.grid.GridPanel({
                id: 'static-grid',
                store: store,
                columns: [{
                        header: 'NAME',
                        width: 170,
                        sortable: true,
                        dataIndex: 'name'
                    },
                    {
                        header: 'PHONE #',
                        width: 150,
                        sortable: true,
                        dataIndex: 'phone'
                    },
                    {
                        header: 'BIRTHDAY',
                        width: 100,
                        sortable: true,
                        dataIndex: 'birthday',
                        renderer: Ext.util.Format.dateRenderer('d/m/Y')
                    },
                    {
                        header: 'EMAIL',
                        width: 160,
                        sortable: true,
                        dataIndex: 'email'
                    }
                ],
                stripeRows: true,
                autoHeight: true,
                width: 580,
                title: 'My Contacts'
            });

            grid.render('grid-example');
            grid.downloadExcelXml(false, '文件标题');
        });
    </script>
</body>

</html>

```

**4. 注意事项**

* 1. 仅在ExtJs3.4.1下测试
* 2. 由于href长度限制，只支持少量数据导出，大量数据的导出需要借助后台完成。

**5. 参考链接**

综合类 | 地址
:----:|:----:
Extjs-Grid数据导出成Excel|http://www.cnblogs.com/sb19871023/p/3894452.html
ExtJs Grid导出到Excel(修正版)|http://extjs.org.cn/node/324
Generate an Excel File from an Ext JS 4 Grid or Store!|https://druckit.wordpress.com/2013/10/26/generate-an-excel-file-from-an-ext-js-4-grid/
# EasyUI使用技巧
    
总结日常EasyUI使用的小技巧

## select标签在EasyUI中的使用

    <select id="elementId" class="easyui-combobox" data-options="onChange:initSelect,editable:false"></select>

给ID为elementId动态赋值

* 方式一：

        function initSelect(){
            $("#elementId").combobox('clear');
            var options = $("#elementId").combobox('options');
            options.textField= "pch";
            options.valueField="pch";
            $("#elementId").combobox('loadData',data);
        }

* 方式二：

        function initSelect(){
            $("#elementId").combobox('clear');		 
            if(data.length>0){
                $("#elementId").combobox({
                    valueField:"pch",
                    textField:"pch",
                    value:data[0].pch,
                }).combobox("loadData",data);
            }
        }
        

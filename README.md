# Rtl Bootstrap TreeView
نمودار درختی پیشرفته ای برای استفاده در حالت های مختلف

    - امکان فعال کردن انتخاب برای همه ی موارد 
    - امکان فعال کردن انختاب تنها مواردی که فرزند هستند و زیر شاخه ندارند
    - غیر فعال کردن موارد دلخواه
    - انتخاب موارد دلخواه در لحظه ساخته شدن
    - چند سطحی
    - چند انتخابی یا تک انتخابی
    - راست چین و چپ چین
    - قراردادن چک باکس در راست و چپ
    - نمایش تعداد فرزندان
  
![samples](https://user-images.githubusercontent.com/53290330/236744138-6d37591b-7e4c-4a31-8395-c706d19559a0.png)



## نحوه استفاده:
## 1-این فایل ها را اضافه کنید:

```html
<link rel="stylesheet" href="css/bootstrap.css">
<script src="js/jquery-3.6.4.js"></script>
<script src="js/optional-treeview.js"></script>
```
- برای دریافت آیکون ها این لینک را اضافه کنید
```html
<script src="https://kit.fontawesome.com/aec3832ad8.js" crossorigin="anonymous"></script>
```
## 2-افزودن یکDIV برای نمایش نمودار:
```html
<a>All Items Checkable</a>
<div class="form-group">
    <div id="tree_view_div_id"></div>
</div>

```
## 3-افزودن تابع ایجاد ارایه مناسب برای نمودار:

```javascript
function createTreeArray(data, selected_items, disable_items){
        let treeData=[];
        for(let i=0;i<data.length;i++){
            if(data[i]["master_id"]===null){
                let item={
                    id: data[i]["id"],
                    text: data[i]["step_name"],
                    state:{
                        checked:false,
                        disabled:false
                    }
                };
                //set selected items
                for(let t=0;t<selected_items.length;t++)
                {
                    if(selected_items[t]===item["id"]){
                        item.state.checked=true;
                        break;
                    }
                }
                //set disable items
                for(let t=0;t<disable_items.length;t++)
                {
                    if(disable_items[t]===item["id"]){
                        item.state.disabled=true;
                        break;
                    }
                }
                if(loop(item)!== false){
                    item.nodes=loop(item)
                }
                treeData.push(item);
            }
        }
        function loop(input){
            let array_of_items=[];
            let item={}
            let exists=false;
            for(let b=0;b<data.length;b++){
                if(data[b]["master_id"]===input["id"]){
                    exists=true;
                    item={
                        id: data[b]["id"],
                        text: data[b]["step_name"],
                        state:{
                            checked:false
                        }
                    };

                    //set selected items
                    for(let t=0;t<selected_items.length;t++)
                    {
                        if(selected_items[t]===item["id"]){
                            item.state.checked=true;
                            break;
                        }
                    }
                    //set disable items
                    for(let t=0;t<disable_items.length;t++)
                    {
                        if(disable_items[t]===item["id"]){
                            item.state.disabled=true;
                            break;
                        }
                    }
                    if(loop(item)!== false){
                        item.nodes=loop(item)
                    }
                    array_of_items.push(item);
                }
            }
            if(exists===false)
                return false;
            return array_of_items;
        }
        return treeData;
    }
```

این تابع سه ورودی دارد
<br />
- data
 (ارایه ای از آبجکت ها مانند:
[{"id":1,"step_name":"one","master_id":null},{"id":4,"step_name":"one_chiled","master_id":1}])
- selected_items (لیستی از "id" برای انتخاب پیشفرض مانند: [1,3])
- disable_items (لیستی از "id" برای غیر فعال کردن مانند: [2,3,5])

## 4-دریافت اطلاعات
```javascript
let allSteps=
[{"id":1,"step_name":"one","master_id":null},{"id":4,"step_name":"one_chiled","master_id":1},{"id":5,"step_name":"one_chiled_chiled","master_id":4},
            {"id":2,"step_name":"two","master_id":null},
            {"id":3,"step_name":"three","master_id":null}];
```
### دیتای ورودی
دیتای ورودی آرایه ای از آبجکت هاست که هر آبجکت شامل سه مقدار است:
<br />
- "id": unique field for any item
- "step_name": text for items
- "master_id": father id for item

## 5-تبدیل دیتای ورودی به دیتای قابل استفاده در نمودار
```javascript
let treeD=createTreeArray(allSteps,[],[]);
```

## 6-تعریف و تنظیم نمودار

```javascript
$('#tree_view_div_id').treeview({
            data: treeD,
            showIcon: true,
            showCheckboxForMasters: true,
            showCheckbox: true,
            CheckboxLeft: false,
            multiSelect: true,
            onNodeChecked: function(event, node) {

            },
            onNodeUnchecked: function (event, node) {
            }
    });
```
### تنظیمات
| option name  | value | description |
| ------------- | ------------- | ------------- |
| data  | array like: [{"id":1,"step_name":"one","master_id":null},{"id":4,"step_name":"one_chiled","master_id":1},{"id":5,"step_name":"one_chiled_chiled","master_id":4}]  | this is an array of objects with three field  |
| showCheckboxForMasters  | -true -flase| this is for show check box for items that have child  |
| showCheckbox  | -true -flase| this is for show check box for all items |
| CheckboxLeft  | -true -flase| this is for show check box befor item text or opposite side of tex |
| multiSelect  | -true -flase| this is for enable multi choice or single choice |
| ShowChildCount  | -true -flase| this is for show chiled count |



#treeView#bootstrap treeview#laravel#html#css#rtl treeview#rtl bootstrap treeview

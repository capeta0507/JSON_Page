# page分頁製作
###### tags: `HTML5` `JavaScript` `JSON`
![](https://i.imgur.com/GkAFq55.png)
一、先預設各種變數
```javascript=
var xURL = "./data/CPMP.json"; // 資料來源
        
var myDataTable = '';  // show tBody table (Tr/td)
var myData = [];  // 接收 JSON資料讀取的陣列
//紀錄筆數、頁數、頁次
const pCount = 10; //單頁筆數
var recTotal = 0 //記錄總筆數
var pNoTotal = 0 //總頁數
var pNo = 1; //頁號碼
var min = 0; // 該頁 第一筆 紀錄的陣列 index
var max = 9; // 該頁 最後一筆 紀錄的陣列 index

// 頁號碼的群組
var myPageLi = ''; // show myPageNo UL -> LI
var pGroupTotal = 1;
var pGroupNo = 1; //每十頁為1Group
var pGroupMin = 1; // 該頁 第一筆 紀錄的陣列 index
var pGroupMax = 10; // 該頁 最後一筆 紀錄的陣列 index
```
二、讀取資料
```javascript=
//讀取資料的函式
myReadJOSNData();
```
```javascript=
function myReadJOSNData(){
    $.getJSON(xURL,
        function(data){
            myData = data
            // console.log(data)
            // 總筆數
            recTotal = data.length
            pageInfo(); // page計算
            showPage(1); // 秀頁群
            showTable(1); // 秀資料
            groupBtnCheck(); // 檢視按鈕
        }
    )
}
```
三、計算單頁 第一筆與最後一筆 紀錄的陣列 index
```javascript=
pNo = myPageNo; //單頁
min = (pNo-1)*pCount;
max = pNo*pCount-1;
// 小心別超出陣列限制
if (max > (recTotal -1)){
    max = recTotal - 1;
}
```
四、頁號碼與頁群組的計算
```javascript=
// 計算頁數
if((recTotal%pCount) === 0 ){
    pNoTotal = recTotal/pCount
}else{
    pNoTotal = parseInt((recTotal/pCount))+1
}
$("#totalPage").text(pNoTotal);
$("#myCurrentPage").text(pNo);

// 計算頁群組
if((pNoTotal%10) === 0) {
    pGroupTotal = pNoTotal / 10
}else{
    pGroupTotal = parseInt(pNoTotal / 10) + 1;
}
$("#myPageGroup").text(pGroupTotal);
```
五、鎖定翻頁動作
```javascript=
if(pNo == pGroupMin){
    // console.log("鎖定前一頁")
    $("#prevBtn").attr("disabled",true);
}else {
    $("#prevBtn").attr("disabled",false);
}
if(pNo == pGroupMax){
    // console.log("鎖定前一頁")
    $("#nextBtn").attr("disabled",true);
}else {
    $("#nextBtn").attr("disabled",false);
}
```
六、處理 Page按鈕的 Active 狀態
```javascript=
var myPageBtn = document.getElementsByClassName("myPageItem");
for(var x = 0;x < myPageBtn.length;x++){
    // console.log(myPageBtn[x].innerText);
    if(pNo === parseInt(myPageBtn[x].innerText)){
        myPageBtn[x].classList.add("active");
    }else{
        myPageBtn[x].classList.remove("active");
    }
}
```
七、翻頁
```javascript=
// 資料翻頁(上一頁)
function prevPage(){
    pNo--;
    showTable(pNo);
}
// 資料翻頁(下一頁)
function nextPage(){
    pNo++;
    showTable(pNo);
}
// 資料翻頁(上10頁)
function prevGroupPage(){
    pGroupNo--;
    showPage(pGroupNo);
    showTable(pGroupMin);
    groupBtnCheck();
}
// 資料翻頁(下10頁)
function nextGroupPage(){
    pGroupNo++;
    showPage(pGroupNo);
    showTable(pGroupMin);
    groupBtnCheck();
}
```
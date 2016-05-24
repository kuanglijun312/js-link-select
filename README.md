# js-link-select
js link select 联动


![img](link-select-img.png)

数据格式(按需求改相应的代码，如省市区): 
[{'name':'省份','city':[{'name':'城市','shop':['name':'店面',code:'aaaa']}]}]


```
// begin core function
var province = document.getElementById('js_province');
var city = document.getElementById('js_city');
var shop = document.getElementById('js_shop');

function initLinkData() {
	shopList.unshift({"name":"请选择省份","city":[]})
	for(var p = 0,pl = shopList.length; p < pl; p++) {
		shopList[p]['city'].unshift({"name":"请选择城市","shop":[]});
		for(var c=0,cl = shopList[p]['city'].length; c < cl; c++) {
			shopList[p]['city'][c]['shop'].unshift({"name":"请选择店面","code":''});
		}
	}
}

function renderArea(list,selectedIndex,el) {
	var html = [];
	for(var i =0,len=list.length; i < len;i++) {
		var d = list[i];
		html.push('<option value="'+(d.code || d.name)+'" '+(selectedIndex == i ? 'selected="selected"' : '')+'>'+d.name+'</option>');
	}
	el.innerHTML = html.join('');
}

//pass last select value
function setLinkValue(code) {
	//first find all index;
	var index = 0,cityindex = 0,shopindex =0,isfind=false;
	if(code) {
		for(var p = 0,pl = shopList.length; p < pl; p++) {
			for(var c=0,cl = shopList[p]['city'].length; c < cl; c++) {
				for(var s=0,sl=shopList[p]['city'][c]['shop'].length; s<sl;s++) {
					if(shopList[p]['city'][c]['shop'][s].code == code) {
						index = p;
						cityindex = c;
						shopindex = s;
						isfind = true;
						break;
					}
				}
				if(isfind) break;
			}
			if(isfind) break;
		}
	}
	renderArea(shopList,index,province);
	renderArea(shopList[index]['city'],cityindex,city);
	renderArea(shopList[index]['city'][cityindex]['shop'],shopindex,shop);
}

province.onchange = function(e) {
	var index = province.selectedIndex;
	renderArea(shopList[index]['city'],0,city);
	renderArea(shopList[index]['city'][0]['shop'],0,shop);
}
city.onchange = function(e) {
	var index = province.selectedIndex;
	var cityindex = city.selectedIndex;
	renderArea(shopList[index]['city'][cityindex]['shop'],0,shop);
}

initLinkData();
setLinkValue(); //pass none set default; pass existed value ,set value;
//end core function
```

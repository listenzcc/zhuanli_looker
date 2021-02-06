/**
 * 使用方法：
 *
 *        在页面<div data-width="100" data-height="30" id="test" data-border="true" data-zindex="1" data-handler="handler">
 *                  <div value="123">123</div>
 *                  <div value="456">456</div>
 *                  <div value="789" checked="checked">789</div>
 *              </div>
 *         data-width为下拉框宽度（必填），data-height为下拉框高度（必填），checked为默认选中,data-border是否需要边框 默认为false
 *		   data-zindex为图层层级	（主要解决I7下控件互相遮住的问题）
 *         在js里$(选择器).jqSelect();初始化控件
 *
 *
 *         @author zhengchj
 *         @mail zhengchj@neusoft.com
 *
 *         update log     v1.0.0     2016.3.17      完成基本功能
 *
 *                        v1.1.0     2016.3.17      修改BUG
 *
 * 						  v1.2.0     2016.3.17      新增一个参数data-border来控制是否加入边框
 *
 *
 *					      v1.2.1     2016.3.17      修改IE7下点击空白页面后下拉框不收回的问题
 *
 *                        v1.2.2     2016.3.17      修改样式，解决在IE7下自动换行的问题
 *
 *                        v1.2.3     2016.3.18      修改子节点字体大小，修改图层问题
 *
 *					      v1.3.0     2016.3.19      增加data-zindex参数配置以及相关逻辑
 * 							
 * 						  v1.3.1     2016.4.8       修改箭头图标
 * 		
 * 						  v1.4.0     2016.4.18      1.新增data-handler参数（点击下拉选项的时候触发）
 * 													2.屏蔽样式冲突风险
 * 
 * 
 * 						  v1.5.0     2016.4.20      新增setSelectValue API，可以动态设置select组件值
 * 
 * 											        example:$(选择器).setSelectValue(赋值内容);
 *						  
 * 					      v1.5.1     2016.4.27  新增隐藏域
 *
 */



(function($){

	/**
	 * 设置select组件值
	 * @param {Object} value   赋值内容
	 * @author zhengchj
	 * @author zhengchj@neusoft.com
	 */
	$.fn.setSelectValue=function(value){
		var me=$(this);
		if(!me.hasClass("jq-select-container"))
			return;
		var item=me.find("a>ul>li>a[value='"+value+"']");
		if(!item.length>0)
			return;
		item.parent().siblings().find("a").removeClass("active");
		item.addClass("active");
		me.attr("value",value);
		me.find("a>span").html(value);
	}
	
	
    /**
     * 扩展Jquery原型链
     */
    $.fn.jqSelect=function(){
        $(this).each(function(){
            var target=$(this);
            target.hide();
            var selectItem=target.find("[checked='checked']").length!=0?target.find("[checked='checked']"):target.children().eq(0);
            var menuField=$("<span>"+selectItem.html()+"</span>").css({
                "line-height":target.attr("data-height")+"px"
            });
            target.attr("value",selectItem.attr("value"));
            var menu=$("<a></a>").append(menuField).addClass("jq-select-menu").append("<i class='arrow'></i>");
            var hiddenInput;
            if(target.attr("name")){
            	hiddenInput=$("<input type='hidden'>").attr("name",target.attr("name")).val(selectItem.attr("value"));
            	target.removeAttr("name");
            }
            //if($.browser.msie&&$.browser.version<=8){
                menu.css("z-index",target.attr("data-zindex"));
            //}
            if(target.attr("data-border")=="true"){
                menu.addClass("border");
            }
            var list=$("<ul></ul>");
            /**
             * 基于原始dom树构建下拉菜单项
             * @author zhengchj
             * @mail zhengchj@neusoft.com
             */
            target.children().each(function(){
                var me=$(this);
                var item=$("<li></li>");
                var itemContent=$("<a>"+me.html()+"</a>").attr("value",me.attr("value"));
                itemContent.click(function(){
                    itemContent.parent().siblings().find("a").removeClass("active");
                    itemContent.addClass("active");
                    menuField.html(itemContent.html());
                    target.attr("value",itemContent.attr("value"));
                    if(hiddenInput){
                    	hiddenInput.val(itemContent.attr("value"));
                    }
                    if(target.attr("data-handler")){
                    	eval(target.attr("data-handler")+"()");
                    }
                })
                item.append(itemContent);
                list.append(item);
                list.find("li").eq(selectItem.index()).find("a").addClass("active");

            })
            target.width(target.attr("data-width")).height(target.attr("data-height"));
            target.empty();
            menu.append(list);
            target.append(menu);
            if(hiddenInput){
            	target.append(hiddenInput);
            }
            target.addClass("jq-select-container");
            list.data("extend",false);
            list.data("can-click",true);
            /**
             * 绑定事件
             * @author zhengchj
             * @mail zhengchj@neusoft.com
             */
            menu.bind({
                click:function(e){
                    if(!list.data("can-click")){
                        return;
                    }
                    menu.addClass("active");
                    if(!list.data("extend")){
                        list.data("can-click",false);
                    	list.slideDown(function(){
                    		list.data("extend",true);
                    		list.data("can-click",true);
                    	});
                    }else{
                        list.data("can-click",false);
                        list.slideUp(function(){
                        	list.data("extend",false);
                        	list.data("can-click",true);
                        })
                    }
                    e.stopPropagation();
                }
            });
            $(window.document).bind({
                click:function(){
                    if(!list.data("can-click")){
                        return;
                    }
                    menu.removeClass("active");
                    if(list.data("extend")){
                    	list.data("can-click",false);
                    	list.slideUp(function(){
                    		list.data("extend",false);
                            list.data("can-click",true);
                    	})
                    }
                }
            })

        })
    }


})(jQuery);
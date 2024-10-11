


```
 java.io.IOException: JSPException including path '/WEB-INF/views/orders/orders.jsp'.
		at org.apache.tiles.request.servlet.ServletUtil.wrapServletException(ServletUtil.java:61)
		at org.apache.tiles.request.jsp.JspRequest.doInclude(JspRequest.java:125)
		at org.apache.tiles.request.AbstractViewRequest.dispatch(AbstractViewRequest.java:47)
		at org.apache.tiles.request.render.DispatchRenderer.render(DispatchRenderer.java:47)
		at org.apache.tiles.request.render.ChainedDelegateRenderer.render(ChainedDelegateRenderer.java:68)
		at org.apache.tiles.impl.BasicTilesContainer.render(BasicTilesContainer.java:259)
		at org.apache.tiles.template.InsertAttributeModel.renderAttribute(InsertAttributeModel.java:188)
		at org.apache.tiles.template.InsertAttributeModel.execute(InsertAttributeModel.java:132)
		at org.apache.tiles.jsp.taglib.InsertAttributeTag.doTag(InsertAttributeTag.java:299)
		at org.apache.jsp.WEB_002dINF.views.theme.index_jsp._jspx_meth_tiles_005finsertAttribute_005f1(index_jsp.java:761)
		at org.apache.jsp.WEB_002dINF.views.theme.index_jsp._jspService(index_jsp.java:488)
		at org.apache.jasper.runtime.HttpJspBase.service(HttpJspBase.java:70)
		at javax.servlet.http.HttpServlet.service(HttpServlet.java:623)
		at org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:466)
		... 96 more
```

이 에러뜨면

```js
$.ajax({  
    url: '/customers/customers',  
    method: 'GET',  
    data: { id: customerId },  
    dataType: 'json',  
    success: function(data) {  
        // 응답받은 데이터로 고객 정보 업데이트  
        const row = $('.custom-Popover__Section');  
        row.find('.customer-name').text(data.name || '이름 없음');  
        row.find('.customer-location').text(data.location || '위치 없음');  
        row.find('.customer-order-count').text(`주문 ${data.orderCount || 0}개`);  
        row.find('.customer-email').text(data.email || '이메일 없음');  
  
        row.find('.customer').closest('a').attr('href', `/customers/${customerId}`); // href 속성 변경  
        row.find('.custom-Popover__Section').toggle(); // 클릭 시 div를 보이거나 숨김  
    },  
    error: function(xhr, status, error) {  
        console.error('AJAX 요청 중 오류 발생:', error);  
        alert('고객 정보를 가져오는 중 오류가 발생했습니다.'); // 사용자에게 오류 알림  
    }  
});
```

여기서 
```
${data.orderCount || 0}
```

얘가 controller에서 보내주는지 확인 필요
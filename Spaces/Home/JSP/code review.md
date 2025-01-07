

부모 id로 묶고 싶은데 for문안에 ui가 있어 ui는 parent_id 갯수만큼 나오게 하고싶을때
```jsp
<div class="categories" data-category-depth="2">  
    <c:set var="processedParents" value="" />  
    <c:forEach items="${categoryList}" var="category">  
       <c:set var="depth" value="false"/>  
       <c:forEach var="item" items="${categoryList}">  
          <c:if test="${item.parent_id == category.id}">  
             <c:set var="depth" value="true"/>  
          </c:if>  
       </c:forEach>  
       <c:if test="${category.category_depth == 2 and not fn:contains(processedParents, category.parent_id)}">  
          <c:set var="processedParents" value="${processedParents},${category.parent_id}" />  
          <ul class="items" data-reference="${category.parent_id}">  
             <c:forEach items="${categoryList}" var="subCategory">  
                <c:if test="${subCategory.parent_id == category.parent_id}">  
                   <li class="sub-item ${!depth ? '' : 'empty-children'}" data-category-value="${subCategory.id}">  
                      <input type="text" value="${subCategory.category_nm}" readonly>  
                      <input type="hidden" value="${subCategory.id}" readonly>  
                      <c:if test="${!depth}">  
                         <p class="add-category">하위 카테고리 추가</p>  
                      </c:if>  
                      <div class="group ${depth ? 'show-edit-group' : ''}">  
                         <div class="sub-category-edit">  
                            <img src="/resources/images/icons/product manage/category-edit.svg">  
                         </div>  
                         <div class="sub-category-delete">  
                            <img src="/resources/images/icons/product manage/category-del.svg">  
                         </div>  
                         <div class="sub-category-save">  
                            <img src="/resources/images/icons/product manage/category-edit-save.svg">  
                         </div>  
                         <div class="sub-category-cancel">  
                            <img src="/resources/images/icons/product manage/category-edit-cancel.svg">  
                         </div>  
                         <div>  
                            <img src="/resources/images/icons/product manage/category-arrow.svg">  
                         </div>  
                      </div>  
                   </li>  
                </c:if>  
             </c:forEach>  
          </ul>  
       </c:if>  
    </c:forEach>  
</div>
```
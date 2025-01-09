

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

for문으로 depth 최대 길이 만큼 ui 늘리기

```jsp
<div class="comm-field fmain-ield-3">  
  
             <div class="rel category-field">  
  
                <p class="manage-label">카테고리</p>  
  
                <input type="text" placeholder="카테고리 선택" id="category-input" class="manage-input-text">  
                <input type="hidden" placeholder="카테고리 id" id="category-id" class="manage-input-text" name="id">  
  
                <div class="dropdown-box comm-dropdown" id="category-dropdown" style="display: none;">  
                   <div class="navi" style="display: none;">  
                      <img src="/resources/images/icons/product manage/prev-navi.svg" class="navi-prev" onclick="goBack()">  
                   </div>  
                   <!-- items -->  
                   <div class="category-items-contain">  
                      <div class="category-wrap">  
                         <!-- 최상위 카테고리를 한개 두고-->  
                         <!-- 카테고리 클릭시마다 내부 요소만 변경 (ajax요청 보내기)-->  
                         <!-- 상기 방법 아니더라도(반복문 돌리기) -->  
                         <c:set var="maxDepth" value="0" />  
                         <div class="categories current-depth" data-category-depth="1">  
                            <ul class="items current-category-page">  
                               <!-- data-category-value 값과 input value 값과 동일하게 -->  
                               <c:forEach items="${categoryList}" var="category">  
                                  <c:set var="depth" value="false"/>  
                                  <c:forEach var="item" items="${categoryList}">  
                                     <c:if test="${item.parent_id == category.id}">  
                                        <c:set var="depth" value="true"/>  
                                     </c:if>  
                                      <c:if test="${category.category_depth > maxDepth}">  
                                        <c:set var="maxDepth" value="${category.category_depth}" />  
                                      </c:if>  
                                  </c:forEach>  
                                  <c:if test="${category.category_depth == 1}">  
                                     <li class="sub-item ${!depth ? '' : 'empty-children'}" data-category-value = "${category.id}">  
                                        <input type="text" value="${category.category_nm}" readonly>  
                                        <input type="hidden" value="${category.id}" readonly name="id">  
                                        <c:if test="${!depth}">  
                                           <p class="add-category">하위 카테고리 추가</p>  
                                        </c:if>  
                                        <div class="group ${depth ? 'show-edit-group' : ''}">  
                                           <div class="sub-category-edit"><img src="/resources/images/icons/product manage/category-edit.svg"></div>  
                                           <div class="sub-category-delete"><img src="/resources/images/icons/product manage/category-del.svg"></div>  
                                           <!-- category edit mode -->  
                                           <div class="sub-category-save"><img src="/resources/images/icons/product manage/category-edit-save.svg"></div>  
                                           <div class="sub-category-cancel"><img src="/resources/images/icons/product manage/category-edit-cancel.svg"></div>  
                                           <div><img src="/resources/images/icons/product manage/category-arrow.svg"></div>  
                                        </div>  
                                     </li>  
                                  </c:if>  
                               </c:forEach>  
                            </ul>  
                         </div>  
                         <!-- 2부터 최대 depth까지 반복문 돌리기 -->  
                         <c:forEach begin="2" end="${maxDepth}" var="depthLevel">  
                            <div class="categories" data-category-depth="${depthLevel}">  
                               <c:set var="processedParents" value="" />  
                               <c:forEach items="${categoryList}" var="category">  
                                  <c:if test="${category.category_depth == depthLevel and not fn:contains(processedParents, category.parent_id)}">  
                                     <c:set var="processedParents" value="${processedParents},${category.parent_id}" />  
                                     <ul class="items" data-reference="${category.parent_id}">  
                                        <c:forEach items="${categoryList}" var="subCategory">  
                                           <c:if test="${subCategory.parent_id == category.parent_id}">  
                                              <li class="sub-item" data-category-value="${subCategory.id}">  
                                                 <input type="text" value="${subCategory.category_nm}" readonly>  
                                                 <input type="hidden" value="${subCategory.id}" readonly>  
  
                                                 <!-- 하위 카테고리 여부 체크 -->  
                                                 <c:set var="hasChildren" value="false" />  
                                                 <c:forEach items="${categoryList}" var="item">  
                                                    <c:if test="${item.parent_id == subCategory.id}">  
                                                       <c:set var="hasChildren" value="true" />  
                                                    </c:if>  
                                                 </c:forEach>  
  
                                                 <!-- 하위 카테고리 추가 버튼 -->  
                                                 <c:if test="${!hasChildren}">  
                                                    <p class="add-category">하위 카테고리 추가</p>  
                                                 </c:if>  
  
                                                 <div class="group ${hasChildren ? 'show-edit-group' : ''}">  
                                                    <div class="sub-category-edit"><img src="/resources/images/icons/product manage/category-edit.svg"></div>  
                                                    <div class="sub-category-delete"><img src="/resources/images/icons/product manage/category-del.svg"></div>  
                                                    <div class="sub-category-save"><img src="/resources/images/icons/product manage/category-edit-save.svg"></div>  
                                                    <div class="sub-category-cancel"><img src="/resources/images/icons/product manage/category-edit-cancel.svg"></div>  
                                                    <div><img src="/resources/images/icons/product manage/category-arrow.svg"></div>  
                                                 </div>  
                                              </li>  
                                           </c:if>  
                                        </c:forEach>  
                                     </ul>  
                                  </c:if>  
                               </c:forEach>  
                            </div>  
                         </c:forEach>  
  
<%--                         <div class="categories" data-category-depth="2">--%>  
<%--                            <c:set var="processedParents" value="" />--%>  
<%--                            <c:forEach items="${categoryList}" var="category">--%>  
<%--                               <c:if test="${category.category_depth == 2 and not fn:contains(processedParents, category.parent_id)}">--%>  
<%--                                  <c:set var="processedParents" value="${processedParents},${category.parent_id}" />--%>  
<%--                                  <ul class="items" data-reference="${category.parent_id}">--%>  
<%--                                     <c:forEach items="${categoryList}" var="subCategory">--%>  
<%--                                        <c:if test="${subCategory.parent_id == category.parent_id}">--%>  
<%--                                           <li class="sub-item ${!depth ? '' : 'empty-children'}" data-category-value="${subCategory.id}">--%>  
<%--                                              <input type="text" value="${subCategory.category_nm}" readonly>--%>  
<%--                                              <input type="hidden" value="${subCategory.id}" readonly>--%>  
<%--                                              <c:set var="depth" value="false"/>--%>  
<%--                                              <c:forEach var="item" items="${categoryList}" varStatus="status">--%>  
<%--                                                 <c:if test="${item.parent_id == subCategory.id}">--%>  
<%--                                                    <c:set var="depth" value="true"/>--%>  
<%--                                                 </c:if>--%>  
<%--                                              </c:forEach>--%>  
<%--                                              <c:if test="${!depth}">--%>  
<%--                                                 <p class="add-category">하위 카테고리 추가</p>--%>  
<%--                                              </c:if>--%>  
<%--                                              <div class="group ${depth ? 'show-edit-group' : ''}">--%>  
<%--                                                 <div class="sub-category-edit">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-edit.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div class="sub-category-delete">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-del.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div class="sub-category-save">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-edit-save.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div class="sub-category-cancel">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-edit-cancel.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div>--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-arrow.svg">--%>  
<%--                                                 </div>--%>  
<%--                                              </div>--%>  
<%--                                           </li>--%>  
<%--                                        </c:if>--%>  
<%--                                     </c:forEach>--%>  
<%--                                  </ul>--%>  
<%--                               </c:if>--%>  
<%--                            </c:forEach>--%>  
<%--                         </div>--%>  
<%--                         <div class="categories" data-category-depth="3">--%>  
<%--                            <c:set var="processedParents" value="" />--%>  
<%--                            <c:forEach items="${categoryList}" var="category">--%>  
<%--                               <c:if test="${category.category_depth == 3 and not fn:contains(processedParents, category.parent_id)}">--%>  
<%--                                  <c:set var="processedParents" value="${processedParents},${category.parent_id}" />--%>  
<%--                                  <ul class="items" data-reference="${category.parent_id}">--%>  
<%--                                     <c:forEach items="${categoryList}" var="subCategory">--%>  
<%--                                        <c:if test="${subCategory.parent_id == category.parent_id}">--%>  
<%--                                           <li class="sub-item ${!depth ? '' : 'empty-children'}" data-category-value="${subCategory.id}">--%>  
<%--                                              <input type="text" value="${subCategory.category_nm}" readonly>--%>  
<%--                                              <input type="hidden" value="${subCategory.id}" readonly>--%>  
<%--                                              <c:set var="depth" value="false"/>--%>  
<%--                                              <c:forEach var="item" items="${categoryList}">--%>  
<%--                                                 <c:if test="${item.parent_id == subCategory.id}">--%>  
<%--                                                    <c:set var="depth" value="true"/>--%>  
<%--                                                 </c:if>--%>  
<%--                                              </c:forEach>--%>  
<%--                                              <c:if test="${!depth}">--%>  
<%--                                                 <p class="add-category">하위 카테고리 추가</p>--%>  
<%--                                              </c:if>--%>  
<%--                                              <div class="group ${depth ? 'show-edit-group' : ''}">--%>  
<%--                                                 <div class="sub-category-edit">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-edit.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div class="sub-category-delete">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-del.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div class="sub-category-save">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-edit-save.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div class="sub-category-cancel">--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-edit-cancel.svg">--%>  
<%--                                                 </div>--%>  
<%--                                                 <div>--%>  
<%--                                                    <img src="/resources/images/icons/product manage/category-arrow.svg">--%>  
<%--                                                 </div>--%>  
<%--                                              </div>--%>  
<%--                                           </li>--%>  
<%--                                        </c:if>--%>  
<%--                                     </c:forEach>--%>  
<%--                                  </ul>--%>  
<%--                               </c:if>--%>  
<%--                            </c:forEach>--%>  
<%--                         </div>--%>  
                      </div>  
                   </div>  
                   <!-- new add -->  
                   <div class="new-add" onclick="openPopup('categoryPopup')">  
                      <p>신규추가</p>  
                      <img src="/resources/images/icons/product manage/add.svg">  
                   </div>  
                </div>  
             </div>  
             <label class="stock rel">  
                <p class="manage-label">미디어</p>  
                <input type="text" placeholder="미디어 선택" id="media-input"  onkeyup="filterDropdown('media')" class="manage-input-text">  
                <div class="category-dropdown dropdown-box comm-dropdown" id="media-dropdown" style="display: none;">  
                   <div class="list">  
                      <ul>  
                         <c:forEach items="${mediaList}" var="media">  
                            <li class="dropdown-item" onclick="selectItem('media', '${media.media_nm}', '${media.id}')">  
                               <div class="content">  
                                  <p class="category-item">${media.media_nm}</p>  
                               </div>  
                            </li>  
                         </c:forEach>  
                      </ul>  
                   </div>  
                   <div class="new-add" onclick="openPopup('mediaPopup')">  
                      <p>신규추가</p>  
                      <img src="/resources/images/icons/product manage/add.svg">  
                   </div>  
                </div>  
             </label>  
          </div>
```
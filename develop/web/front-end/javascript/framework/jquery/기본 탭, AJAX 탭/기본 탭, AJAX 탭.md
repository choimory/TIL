# 탭버튼

- 주로 ul>li 태그를 이용
- 탭버튼(li)에 이벤트를 건다
- 모든 탭버튼의 클래스명을 지운다
- 클릭된 탭버튼(this)에 on, active등의 클래스명을 추가해주어 클릭된 탭에 어울리는 스타일을 입힌다

# 탭버튼에 맞는 항목만 노출

- 모든 항목을 만들어놓은뒤 `<div id="" style="display:none;">`으로 씌워놓는다
- 클릭된 탭버튼(this)에서 data-id에서 id를 얻어 해당 id의 div의 display를 block으로 바꾸고(`$(selector).css("display","block")`), 나머지는 다 none으로 바꾼다(`$(selector).css("display","none")`)

# 탭버튼 항목 AJAX로 구현

1. 탭버튼 항목의 태그를 별도의 JSP파일로 만들어 작성한다
2. 탭버튼 항목이 들어갈 곳을 `<div id="tabbody"></div>`로 위치만 잡아놓는다
3. 탭버튼 클릭 이벤트로 AJAX를 날린다. 이때 받을 dataType은 html로 한다.
4. 성공시 받은 결과물을 2의 div에 append(result) 혹은 html(result)한다 (`$("#tabbody").append(html) 혹은 $("#tabbody).html(html)`)
5. 컨트롤러에서 model에 값을 담아 1의 JSP 경로를 리턴하면, 해당 JSP에서 조립된 HTML태그가 2로 들어간다
1. AJAX 결과를 받아 화면을 그릴 부분의 태그 부분만 별도의 JSP파일로 만들어 작성한다
2. 메인파일에 클릭 이벤트를 작성해 AJAX를 날린다. 이때 AJAX 속성 중 dataType은 html로 한다.
3. 메인의 해당 AJAX에서 성공시 받은 결과물을 그린 화면이 들어갈 div에 append(result) 혹은 html(result)한다 (`$("#ajaxResult").append(html) 혹은 $("#ajaxResult).html(html)`)
4. 컨트롤러에서 model에 값을 담아 1번(Ajax jsp) 파일경로를 리턴하면, 1번(Ajax jsp)에서 조립된 HTML태그가 2번(Main jsp)로 들어간다
5. 이때 Ajax jsp 파일에 js나 css관련 리소스를 임포트시킬 필요 없음. 화면에 붙일 Main jsp파일에 임포트가 되어있기 때문. 이때 Ajax jsp에도 리소스를 임포트시키면 리소스가 두번 불러와지면서 Main jsp에 이미 그려놓은 화면이 망가지는 경우가 생김. Ajax jsp는 순수 코드만 생성하여 넘겨지고 jq나 css등 리소스에 대한 적용은 Main jsp에서 이루어지는것
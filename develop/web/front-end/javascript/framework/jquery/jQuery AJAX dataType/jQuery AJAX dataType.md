# 개요

dataType은 ajax를 날려서 받을 데이터의 타입

json, html, text 등이 있음

# dataType='json'

리턴된 값을 json형태로 받음 [ {key1:value1, key2:value2....},{} ]

# dataType='html'

datatype(리턴타입)을 html로 지정하여 ajax를 보내면

컨트롤러에서 model에 데이터를 담아 jsp경로를 리턴할시

해당 jsp에서 데이터를 조립해서 완성된 해당 jsp파일의 html코드가 ajax success함수로 리턴됨

AJAX 요청 -> 컨트롤러 작업 & JSP 경로 리턴 -> JSP 화면 조립 -> AJAX부른곳에서 조립된 화면의 코드 받아서 append

이때 JSP는 append될 곳에 대한 코드만 작성된 별도의 JSP파일

이때 ajax에서 html로 붙인 파일의 요소 중에 이벤트를 걸때는 ajax시 덧붙일 jsp파일에 스크립트를 써놔야함

이벤트를 메인 파일에 쓰면 처음에는 해당 요소가 없어서 페이지 로드되어도 요소를 못찾으니까
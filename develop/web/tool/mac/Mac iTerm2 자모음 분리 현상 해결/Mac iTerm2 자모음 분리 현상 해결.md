# 개요

![img.png](img.png)

- iTerm2에서 한글로 된 파일명의 자모음이 분리되어 표기되는 경우가 있다

# 해결

![img_1.png](img_1.png)

![img_2.png](img_2.png)

![img_3.png](img_3.png)

1. iTerm2의 Preference 진입 (`cmd + ,`)
2. Profiles - text의 Unicode normalization form을 none에서 NFC 혹은 HFS+로 변경 후 잘 동작하는지 확인
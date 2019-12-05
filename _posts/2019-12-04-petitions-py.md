---
title: "파이썬 청와대 국민청원 스크랩핑(python korea government petitions scraping)"
date: 2019-12-04T04:00:00+09:00
categories:
  - IT
tags:
  - scraping
  - python
  - petition
  - 국민청원
  - 청와대

---
### 파이썬 청와대 국민청원 스크랩핑(python korea government petitions scraping)
---
- Tested Environment : python3, google colaboratory 
- 참고링크 중 "국민청원 데이터셋 CSV"의 내용을 토대로 수정하여 작성
- 1건당 1-2초(google colab에서 더 느림, 실 사용은 로컬을 권장함)

- 참고:
  - [국민청원 데이터셋 CSV](https://newhiwoong.github.io/%EA%B5%AD%EB%AF%BC%EC%B2%AD%EC%9B%90/%EA%B5%AD%EB%AF%BC%EC%B2%AD%EC%9B%90-%EB%8D%B0%EC%9D%B4%ED%84%B0%EC%85%8B)
  - [청와대 국민청원 https://www1.president.go.kr/petitions](https://www1.president.go.kr/petitions)
  - [Using Selenium with Google Colaboratory https://darektidwell.com/using-selenium-with-google-colaboratory/](https://darektidwell.com/using-selenium-with-google-colaboratory/)
  - [https://stackoverflow.com/questions/56829470/selenium-google-colab-error-chromedriver-executable-needs-to-be-in-path](https://stackoverflow.com/questions/56829470/selenium-google-colab-error-chromedriver-executable-needs-to-be-in-path)

### 코드(code)
  - [github](https://github.com/huisung/president_go_kr_petitions_scraping)
  - [구글 코랩에서 실행해보기 Open in Colab](https://colab.research.google.com/github/huisung/president_go_kr_petitions_scraping/blob/master/get_petition.ipynb)

```python
import sys

# install chromium, its driver, and seleniu함
if 'google.colab' in sys.modules:
    !apt-get update
    !apt install chromium-chromedriver
    !cp /usr/lib/chromium-browser/chromedriver /usr/bin
    !pip install selenium

from bs4 import BeautifulSoup
import re

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

options = webdriver.ChromeOptions()
options.add_argument('--headless')
options.add_argument('--no-sandbox')
options.add_argument('--disable-dev-shm-usage')


def get_petition(wd, number):
    base_url='https://www1.president.go.kr/petitions/'
    try:
        wd.get(base_url+str(number))
        element = WebDriverWait(wd, 2).until(
            EC.presence_of_element_located((By.CLASS_NAME, 'petitionsView_title'))
            )
        #bs = BeautifulSoup(wd.page_source, 'html.parser')
        bs = BeautifulSoup(wd.page_source, 'lxml')

        progress = bs.find('div',{'class':'petitionsView_progress'}).get_text().strip()
        title = bs.find('h3',{'class':'petitionsView_title'}).get_text().strip()
        like = bs.find('h2',{'class':'petitionsView_count'}).get_text().strip()
        like = int(''.join(re.findall('\d+', str(like))))
        if progress == '답변완료':
            contents = bs.find_all('div',{'class':'View_write'})[1].get_text().strip()
            answer = bs.find_all('div',{'class':'pr_tk25'})[1].get_text().strip()[4:].strip()
        else:
            contents = bs.find('div',{'class':'View_write'}).get_text().strip()
            answer = ''
        info = bs.find('ul',{'class':'petitionsView_info_list'}).get_text().split('\n')
        category = info[1][4:]
        sday = info[2][4:]
        eday = info[3][4:]
        proponent = info[4][3:].split()[0]
        return {'number':number, 'progress':progress, 'title':title,  'like':like,
                'contents':contents, 'answer':answer, 'category':category,
                'sday':sday, 'eday':eday, 'proponent':proponent}
    except Exception as e:
        print(number, e)
        return {'number':number, 'progress':'error', 'title':'',  'like':0,
                'contents':str(e), 'answer':'', 'category':'',
                'sday':'', 'eday':'', 'proponent':''}


if __name__ == '__main__' or 'google.colab' in sys.modules:
    wd = webdriver.Chrome('chromedriver',options=options)
    print(get_petition(wd, 300))
    print(get_petition(wd, 582349))
    wd.quit()
```

### 결과(result)

```python
{'number': 300, 'progress': '청원종료', 'title': '대기업의 잘못된 인터넷 개인가입자 민원 처리방식과 위약금', 'like': 0, 'contents': '저는 SK인터넷,TV를 가입해서 사용하고 있는 개인 가입자 입니다.8/22일 휴가를 마치고 집으로 오니 인터넷,TV가 안되어서 SK 장애부서인 106번 으로전화하여 수리요청을 했습니다.상담원이 관할지역 기사님을 알아보고는 오늘은 기사님들이 스케줄이 안되어서 장애처리를 내일 해야 된다고 하였습니다.그래서 오늘 아이들 인터넷으로 숙제도해야되고 TV시청도 해야되니 오늘 늦더라도 고쳐달라고 말하니,조금뒤 기술부서 실장님이 전화를 주셔서 어떻게 해도 금일중으로는  기사님을 수배할수 없기 때문에 장애처리를 할수 없고 내일 오전10시즘에 처리 할수 있다고 말했습니다.A/S기사님들은 오전에 그날 A/S처리  스케줄을 다잡고 나가는데 그러면 소비자 들은 장애가 나면 무조건 다음날 A/S처리를 봤아야 되니까? 소비자 잘못이 아니라 SK장비때문에 장애가 났는데 이거는 SK에서 긴급 장애처리 기사를 더고용해서 당일 장애건도 그날A/S처리를 해줘야 되는거 아닙니까.그리고 SK잘못때문에 장애가 났는데 당일 처리를 못해주면 인터넷 해지를 요청하니 수십만원의 위약금을 물어야 된다고 하는데 이것은 잘못된 처방 아닙니까.자기들 장비때문에 장애가나서 사용못하고 그래서 수리요청하니 당일 처리가 힘들고,그러면 해지하려하니 위약금 내라고 하고,완전히 개인가입자는 봉입니까.그리고 처음 인터넷가입시 장애처리 문제에 대해서는 설명도 없었는데 장애가 나고나니 스케줄 잡아서 내일 A/S받든지 아님 나갈 사람이 없어방법이 없다.알아서 해라라는 식으로 답변을 듣기만 해야되나요.장애신고 하는 사람들은 지금 인터넷,TV가 안되어서 전화를 하는 사람들이지 미리 고장날것을 알고 전화해서 예약스케줄을 잡는 사람들이 아닙니다.SK인터넷 장애처리부서는 생각좀 하시고 일합시다.', 'answer': '', 'category': '기타', 'sday': '2017-08-22', 'eday': '2017-09-06', 'proponent': 'kakao'}
{'number': 582349, 'progress': '답변완료', 'title': '나경원 자한당 원내대표의 각종 의혹에 대한 특검 요청!', 'like': 365040, 'contents': '나경원 자한당 원내대표의 각종 의혹ㆍ논란들이\n일파만파 번지고 있습니다.\n\n야권정치인의 실세인만큼 의혹이 말끔히 해소되려면\n야당이 그토록 강조하는 정치적 중립성을 보다 강조하기 위해서는 현정권의 하에 있는 검찰보다 나경원 의원이 좋아하는 특검을 설치하여 모든 의혹을 말끔히 해소하는게 나경원 원내대표도 바라마지 않을것입니다.\n\n이에 특검수사를 요청합니다.', 'answer': '다음은 나경원 자유한국당 원내대표의 각종 의혹에 대해 특검을 요청하신 청원에 대해 답변드리겠습니다.\n본 청원은 8월 28일부터 한 달간 36만 여명의 국민께서 동의해 주셨습니다.\n\n청원의 계기가 된 것은 누리꾼 사이에서 불거진 나경원 의원의 자녀에 대한 의혹입니다. 국적 의혹, 논문의 제1저자 특혜 의혹 등과 관련된 다수의 언론 보도가 있었습니다.\n\n청원인께서는 나경원 자유한국당 원내대표가 야권의 대표 정치인인만큼 정치적 중립성을 보장하기 위해 특별검사를 통해 의혹을 말끔히 해소할 필요가 있다고 주장하셨습니다.\n\n먼저 청원인께서 요구하신 특별검사제도에 대하여 설명드리겠습니다.\n\n특별 검사 제도는, 주로 고위 공직자의 비리 또는 위법 혐의가 발견되었을 때 그 수사와 기소를 정권의 영향을 받을 수 있는 정규 검사가 아닌 ‘독립된 변호사’로 담당하게 하는 제도입니다.\n특별 검사 제도는 특정 사건에 한정하여 검찰 등 행정부와는 독립된 사람에게 수사 및 기소 등 검사 역할을 수행하도록 하는 제도입니다.\n\n우리나라의 특검제는 정치적 사건이나 권력형 범죄와 비리 사건에서 나타난 검찰 수사의 소극성 또는 편향성에 대한 제도적 보완으로 1988년 당시 야당이던 평화민주당에 의해 처음 제기되었습니다. 그 이후 10년 이상의 논의를 거쳐 1999년에 특검제가 도입되었습니다. 또한 2014년에는 상설 특검을 도입하는 내용을 담은 ‘특별검사의 임명 등에 관한 법률’이 제정되었습니다.\n\n특검은 △ 국회가 정치적 중립성과 공정성 등을 이유로 특별검사의 수사가 필요하다고 판단하여 본회의에서 의결하거나 △ 법무부 장관이 정부 고위공직자의 비리를 대상으로 이해관계 충돌이나 공정성 등을 이유로 특검의 수사가 필요하다고 판단할 경우 발동될 수 있습니다. 후자의 경우, 다시 말해 법무부 장관이 특검 여부를 판단하는 경우, 법무부 장관은 반드시 검찰총장의 의견을 들어야 합니다.\n\n본 청원의 경우, 청원인께서는 나경원 자유한국당 원내대표의 자녀관련 의혹을 밝히는 특검을 요구하셨습니다. 그러나 본 건과 관련해 특별검사의 도입 여부는 국회에서 논의하여 결정해야 할 사안입니다. 법무부 장관이 정부와 무관한 사안에 대해 특검을 발동할 수는 없기 때문입니다.\n\n지난 9월 한 시민단체는 나경원 자유한국당 의원의 ‘자녀입시 의혹’과 관련하여 검찰에 이를 고발하였습니다. 그 이후 본 사건은 현재 검찰에서 수사 중입니다. 따라서 검찰의 수사 결과를 지켜봐야 하는 상황입니다.\n\n최근 부모의 특권적 지위를 이용하여 입시제도에서 혜택을 받은 경우에 대해 국민적 우려는 물론, 입시제도에서의 공정성, 사회적 불평등 해소에 대한 국민의 요구가 높습니다. 이러한 국민의 목소리를 반영하여 교육부에서는 학생부종합전형 전면 실태조사를 엄정하게 추진하고 있으며, 나아가 고교 서열화 해소를 위한 방안 등 입시제도 개편안을 준비하고 있습니다.\n\n또한 국회에서도 최근『국회의원 자녀의 대학입학전형과정 조사에 관한 특별법』『고위공직자 자녀 입시비리 조사를 위한 특별법』등 관련 법안이 발의되었습니다. 이 법안은 입시비리에 대한 전수조사 특별위원회를 구성하여 고위공직자의 자녀에 대한 특혜를 조사하고 결과에 따라 고발 및 수사요청, 감사원 감사요구 등을 실시하는 내용 등을 담고 있습니다.\n\n정부는 본 청원을 계기로 국회의원을 비롯한 사회 특권층, 그리고 이들 자녀의 입시 특혜 등 다양한 불공정에 대한 국민적 우려와 공정에 대한 강력한 열망을 다시 한 번 절감하였습니다. 이 점에 대해 청원인과 청원에 참여해 주신 국민께 깊이 감사드립니다.\n\n정부는 우리 사회에 만연한 특권과 반칙, 불공정을 없애고, 나아가서 제도에 내재 된 합법적인 불공정과 특권까지 근본적으로 바꿔낼 수 있도록 더욱 노력하겠습니다.\n\n오늘 답변은 여기서 마치겠습니다. 감사합니다.', 'category': '정치개혁', 'sday': '2019-08-28', 'eday': '2019-09-27', 'proponent': 'naver'}
```


### 사용예(Example)

```python
import pandas as pd


def get_p(num = 583432, cnt = 10):
    rtn = []
    wd = webdriver.Chrome('chromedriver',options=options)
    for idx, i in enumerate(tqdm(range(num-cnt, num))):
        tmp = get_petition(wd, i)
        if idx == 0:
            rtn_c = list(tmp.keys())
        rtn.append(list(tmp.values()))
        # 오래 유지될경우 느려지는 경향이 있어 중간에 초기화
        if idx % 200 == 0:
            wd.quit()
            wd = webdriver.Chrome('chromedriver',options=options)
    return (rtn_c, rtn)


c, rtn = get_p()
wd = webdriver.Chrome('chromedriver',options=options)
rtn.append(list(get_petition(wd, 582349).values()))
rtn.append(list(get_petition(wd, 10).values()))
wd.quit()
df = pd.DataFrame(rtn, columns=c)
df.to_pickle('petition.pkl')
```
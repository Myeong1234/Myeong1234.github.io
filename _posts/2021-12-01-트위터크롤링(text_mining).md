---
title: "트위터 크롤링"
toc: true
toc_sticky: true
excerpt_separator: "<!--more-->"
author_profile: true
sidebar:
  nav: "main"
categories:
  - Post Formats
tags:
   - python
   - 크롤링
---



# 트위터 크롤링 해보기


```python
import tweepy

CONSUMER_KEY ="gMJgmn9Wtxz5o6fNAC3Pn5VCP"
CONSUMER_SECRET = "Jli5H8awshv4gZh0TAuqkVwpjLcJ5L6oChkXMV0PtMrRrRQeRl"
ACCESS_TOKEN_KEY = "1466216464252542982-egn2pnWVE8ANLonFQCMzEvKppGVzrv"
ACCESS_TOKEN_SECRET = "cobtmkXvOBNoEkpn64VAs3m6tSbnO9a2ooG0JwjpPyyBE"

#개인정보 인증을 요청하는 핸들러
auth = tweepy.OAuthHandler(CONSUMER_KEY,CONSUMER_SECRET)

#인증 요청을 수행합니다
auth.set_access_token(ACCESS_TOKEN_KEY,ACCESS_TOKEN_SECRET)
```


```python
api = tweepy.API(auth)

keyword = '손흥민'
tweets = api.search(keyword)
for tweet in tweets:
    print(tweet.entities['user_mentions'])
    print(tweet.entities['hashtags'])
    print(tweet.text)

```

    [{'screen_name': 'HPE', 'name': 'HPE', 'id': 207495308, 'id_str': '207495308', 'indices': [33, 37]}]
    [{'text': 'THFC', 'indices': [39, 44]}, {'text': 'COYS', 'indices': [45, 50]}, {'text': '토트넘', 'indices': [51, 55]}, {'text': '손흥민', 'indices': [56, 60]}]
    [구단특집] 손흥민 선수의 브렌트포드전 경기후 소감!!
    🤳 @HPE
    
    #THFC #COYS #토트넘 #손흥민 https://t.co/HgG4OCXGsP
    []
    []
    다른 감독들도 손흥민다른자리에써보기시기 다 거쳐갔듯이 콘버지도 경기 하다 보면 롤은 제대로 자리잡을 것 같은데…그 전까지 부상 뜨지 않을까 걱정이다
    [{'screen_name': 'classs_77', 'name': '𝙎𝘾𝙇𝘼𝙎𝙎', 'id': 1032923568256966657, 'id_str': '1032923568256966657', 'indices': [3, 13]}]
    []
    RT @classs_77: 손흥민 골 ㅠㅠㅠㅠㅠㅠ 🤍 https://t.co/TwOsQwWGfx
    [{'screen_name': 'sonny_cut', 'name': '쏘니컷🎬𝙎𝙤𝙣𝙣𝙮_𝙘𝙪𝙩', 'id': 418732818, 'id_str': '418732818', 'indices': [3, 13]}]
    []
    RT @sonny_cut: 셀브하는 손흥민 뒤로 흘러나오는 손흥민,, 짜릿해💙 https://t.co/0kAQIk70EM
    [{'screen_name': 'today_football', 'name': '오늘의 해외축구', 'id': 444525754, 'id_str': '444525754', 'indices': [3, 18]}, {'screen_name': 'SpursOfficial', 'name': 'Tottenham Hotspur', 'id': 121402638, 'id_str': '121402638', 'indices': [101, 115]}]
    [{'text': '오늘의해외축구', 'indices': [117, 125]}, {'text': '오해툰', 'indices': [126, 130]}]
    RT @today_football: 토트넘이 브렌트포드와의 PL 14라운드에서 손흥민의 1골 1자책 유도로 2-0 완승을 거뒀다. 손흥민은 KOTM(킹 오브 더 매치)에 선정됐다.
    @SpursOfficial
    
    #오늘의해외축구 #오해툰 https://…
    [{'screen_name': 'classs_77', 'name': '𝙎𝘾𝙇𝘼𝙎𝙎', 'id': 1032923568256966657, 'id_str': '1032923568256966657', 'indices': [3, 13]}]
    []
    RT @classs_77: 손흥민 어시 벤뎁 골 🤍 벤뎁 무슨일이야 👀 https://t.co/fXvEUHaqOj
    [{'screen_name': 'Spurs_KR', 'name': 'Tottenham Hotspur 🇰🇷', 'id': 1341708258818584576, 'id_str': '1341708258818584576', 'indices': [3, 12]}]
    [{'text': 'THFC', 'indices': [49, 54]}, {'text': 'COYS', 'indices': [55, 60]}, {'text': '토트넘', 'indices': [61, 65]}, {'text': '손흥민', 'indices': [66, 70]}]
    RT @Spurs_KR: 손흥민 21/22시즌 프리미어리그 5호골 (vs 브렌트포드)
    
    #THFC #COYS #토트넘 #손흥민 https://t.co/n39nDjexvn
    [{'screen_name': 'Spurs_KR', 'name': 'Tottenham Hotspur 🇰🇷', 'id': 1341708258818584576, 'id_str': '1341708258818584576', 'indices': [3, 12]}]
    [{'text': 'THFC', 'indices': [49, 54]}, {'text': 'COYS', 'indices': [55, 60]}, {'text': '토트넘', 'indices': [61, 65]}, {'text': '손흥민', 'indices': [66, 70]}]
    RT @Spurs_KR: 손흥민 21/22시즌 프리미어리그 5호골 (vs 브렌트포드)
    
    #THFC #COYS #토트넘 #손흥민 https://t.co/n39nDjexvn
    [{'screen_name': 'OptaJoon', 'name': 'OptaJoon', 'id': 4859599821, 'id_str': '4859599821', 'indices': [3, 12]}]
    []
    RT @OptaJoon: 1 - 손흥민은 토트넘과 브렌트포드의 맞대결에서 경기 최다 득점(1), 유효슈팅(2), 기대득점(0.74), 크로스(9), 스프린트(38)를 모두 기록했다. 에이스. https://t.co/8gXOVHkcQO
    []
    [{'text': '손흥민', 'indices': [12, 16]}, {'text': 'SPORTSTIME', 'indices': [30, 41]}]
    '내가 토트넘의 왕' #손흥민, 원맨쇼로 경기 끝냈다 #SPORTSTIME https://t.co/Bq8oHLzxtK https://t.co/R74voVedIA
    [{'screen_name': 'classs_77', 'name': '𝙎𝘾𝙇𝘼𝙎𝙎', 'id': 1032923568256966657, 'id_str': '1032923568256966657', 'indices': [3, 13]}]
    []
    RT @classs_77: 손흥민 골 ㅠㅠㅠㅠㅠㅠ 🤍 https://t.co/TwOsQwWGfx
    []
    [{'text': '출장업소', 'indices': [0, 5]}]
    #출장업소
    가슴 경주 대구 마사 마산 부산 소통 야동 인천 일탈 최신영화 손흥민 만남앱 소개팅 
     81H93E28R3V
    []
    [{'text': '출장업소', 'indices': [0, 5]}]
    #출장업소
    가슴 경주 대구 마사 마산 부산 소통 야동 인천 일탈 최신영화 손흥민 만남앱 소개팅 
     TB10T999SX9UQ
    []
    []
    톰 홀랜드가 토트넘팬+손흥민팬이고 삼촌이 브렌트포드팬이라 둘이 챔결가는거 보고싶다고 인터뷰한게 퍼지고 그거 누가 알려줬는지 저 세리모니함 ㅋㅋ 저 세리모니 보고 그게 다시 퍼져서 지금 스파이더맨 공계랑 인터… https://t.co/xGNjsOqN91
    [{'screen_name': 'Spurs_KR', 'name': 'Tottenham Hotspur 🇰🇷', 'id': 1341708258818584576, 'id_str': '1341708258818584576', 'indices': [3, 12]}]
    [{'text': 'THFC', 'indices': [53, 58]}, {'text': 'COYS', 'indices': [59, 64]}, {'text': '토트넘', 'indices': [65, 69]}, {'text': '손흥민', 'indices': [70, 74]}]
    RT @Spurs_KR: [하이라이트] 21/22 PL 14R 토트넘 홋스퍼 vs 브렌트포드
    
    #THFC #COYS #토트넘 #손흥민 https://t.co/wj66nyUUPa



```python
%matplotlib inline

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# import tweepy

# CONSUMER_KEY ="gMJgmn9Wtxz5o6fNAC3Pn5VCP"
# CONSUMER_SECRET = "Jli5H8awshv4gZh0TAuqkVwpjLcJ5L6oChkXMV0PtMrRrRQeRl"
# ACCESS_TOKEN_KEY = "1466216464252542982-egn2pnWVE8ANLonFQCMzEvKppGVzrv"
# ACCESS_TOKEN_SECRET = "cobtmkXvOBNoEkpn64VAs3m6tSbnO9a2ooG0JwjpPyyBE"

# #개인정보 인증을 요청하는 핸들러
# auth = tweepy.OAuthHandler(CONSUMER_KEY,CONSUMER_SECRET)

# #인증 요청을 수행합니다
# auth.set_access_token(ACCESS_TOKEN_KEY,ACCESS_TOKEN_SECRET)

```


```python
keyword = '손흥민'
columns = ['created', 'tweet_text']
df = pd.DataFrame(columns=columns)
```


```python
#트위터 api를 사용하여 손흥민이 포함된 100페이지의 트윗들을 크롤링한 뒤, text created_at 정보를 데이터 프레임으로 저장합니다.
for i in range(1,100):
    print("Get data",str(i/500*100),"% complete...")
    tweets = api.search(keyword)
    for tweet in tweets:
        tweet_text = tweet.text
        created = tweet.created_at
        row = [created, tweet_text]
        series = pd.Series(row, index = df.columns)
        df = df.append(series, ignore_index=True)

print('Get data 100 % complete...')
df.head()
```

    Get data 0.2 % complete...
    Get data 0.4 % complete...
    Get data 0.6 % complete...
    Get data 0.8 % complete...
    Get data 1.0 % complete...
    Get data 1.2 % complete...
    Get data 1.4000000000000001 % complete...
    Get data 1.6 % complete...
    Get data 1.7999999999999998 % complete...
    Get data 2.0 % complete...
    Get data 2.1999999999999997 % complete...
    Get data 2.4 % complete...
    Get data 2.6 % complete...
    Get data 2.8000000000000003 % complete...
    Get data 3.0 % complete...
    Get data 3.2 % complete...
    Get data 3.4000000000000004 % complete...
    Get data 3.5999999999999996 % complete...
    Get data 3.8 % complete...
    Get data 4.0 % complete...
    Get data 4.2 % complete...
    Get data 4.3999999999999995 % complete...
    Get data 4.6 % complete...
    Get data 4.8 % complete...
    Get data 5.0 % complete...
    Get data 5.2 % complete...
    Get data 5.4 % complete...
    Get data 5.6000000000000005 % complete...
    Get data 5.800000000000001 % complete...
    Get data 6.0 % complete...
    Get data 6.2 % complete...
    Get data 6.4 % complete...
    Get data 6.6000000000000005 % complete...
    Get data 6.800000000000001 % complete...
    Get data 7.000000000000001 % complete...
    Get data 7.199999999999999 % complete...
    Get data 7.3999999999999995 % complete...
    Get data 7.6 % complete...
    Get data 7.8 % complete...
    Get data 8.0 % complete...
    Get data 8.200000000000001 % complete...
    Get data 8.4 % complete...
    Get data 8.6 % complete...
    Get data 8.799999999999999 % complete...
    Get data 9.0 % complete...
    Get data 9.2 % complete...
    Get data 9.4 % complete...
    Get data 9.6 % complete...
    Get data 9.8 % complete...
    Get data 10.0 % complete...
    Get data 10.2 % complete...
    Get data 10.4 % complete...
    Get data 10.6 % complete...
    Get data 10.8 % complete...
    Get data 11.0 % complete...
    Get data 11.200000000000001 % complete...
    Get data 11.4 % complete...
    Get data 11.600000000000001 % complete...
    Get data 11.799999999999999 % complete...
    Get data 12.0 % complete...
    Get data 12.2 % complete...
    Get data 12.4 % complete...
    Get data 12.6 % complete...
    Get data 12.8 % complete...
    Get data 13.0 % complete...
    Get data 13.200000000000001 % complete...
    Get data 13.4 % complete...
    Get data 13.600000000000001 % complete...
    Get data 13.8 % complete...
    Get data 14.000000000000002 % complete...
    Get data 14.2 % complete...
    Get data 14.399999999999999 % complete...
    Get data 14.6 % complete...
    Get data 14.799999999999999 % complete...
    Get data 15.0 % complete...
    Get data 15.2 % complete...
    Get data 15.4 % complete...
    Get data 15.6 % complete...
    Get data 15.8 % complete...
    Get data 16.0 % complete...
    Get data 16.2 % complete...
    Get data 16.400000000000002 % complete...
    Get data 16.6 % complete...
    Get data 16.8 % complete...
    Get data 17.0 % complete...
    Get data 17.2 % complete...
    Get data 17.4 % complete...
    Get data 17.599999999999998 % complete...
    Get data 17.8 % complete...
    Get data 18.0 % complete...
    Get data 18.2 % complete...
    Get data 18.4 % complete...
    Get data 18.6 % complete...
    Get data 18.8 % complete...
    Get data 19.0 % complete...
    Get data 19.2 % complete...
    Get data 19.400000000000002 % complete...
    Get data 19.6 % complete...
    Get data 19.8 % complete...
    Get data 100 % complete...





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>created</th>
      <th>tweet_text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-12-03 04:00:00</td>
      <td>[구단특집] 손흥민 선수의 브렌트포드전 경기후 소감!!\n🤳 @HPE\n\n#THF...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-12-03 03:55:50</td>
      <td>다른 감독들도 손흥민다른자리에써보기시기 다 거쳐갔듯이 콘버지도 경기 하다 보면 롤은...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-12-03 03:54:10</td>
      <td>RT @classs_77: 손흥민 골 ㅠㅠㅠㅠㅠㅠ 🤍 https://t.co/TwO...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-12-03 03:53:34</td>
      <td>RT @sonny_cut: 셀브하는 손흥민 뒤로 흘러나오는 손흥민,, 짜릿해💙 ht...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-12-03 03:53:14</td>
      <td>RT @today_football: 토트넘이 브렌트포드와의 PL 14라운드에서 손흥...</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>created</th>
      <th>tweet_text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-12-03 02:29:47</td>
      <td>RT @Spurs_KR: 여러분의 오늘경기 #MOTM 픽은 누구인가요? 🤔\n댓글로...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-12-03 02:29:31</td>
      <td>RT @Spurs_KR: #MondayMotivation \n\n여러분 모두 이번 ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-12-03 02:28:12</td>
      <td>RT @Spurs_KR: 손흥민 21/22시즌 프리미어리그 5호골 (vs 브렌트포드...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-12-03 02:27:42</td>
      <td>RT @Spurs_KR: 오늘의 영웅: '손'파이더맨!! 🕷️🕸️@TomHollan...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-12-03 02:26:27</td>
      <td>RT @classs_77: 손흥민 어시 벤뎁 골 🤍 벤뎁 무슨일이야 👀 https:...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021-12-03 02:24:41</td>
      <td>RT @Spurs_KR: 오늘의 영웅: '손'파이더맨!! 🕷️🕸️@TomHollan...</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2021-12-03 02:24:22</td>
      <td>BBC "손흥민의 눈부신 역습..토트넘 6위로 도약" | 다음 스포츠 https:/...</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2021-12-03 02:20:55</td>
      <td>골은 손흥민이 넣었지만 오늘 브렌트포드전 일등 공신은 올리버 스킵과 벤 데이비스라 ...</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2021-12-03 02:18:08</td>
      <td>RT @Spurs_KR: 손흥민 21/22시즌 프리미어리그 5호골 (vs 브렌트포드...</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2021-12-03 02:18:01</td>
      <td>RT @Spurs_KR: 손흥민 21/22시즌 프리미어리그 5호골 (vs 브렌트포드...</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2021-12-03 02:17:53</td>
      <td>BBC "손흥민의 눈부신 역습..토트넘 6위로 도약" | 다음 스포츠 https:/...</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2021-12-03 02:17:48</td>
      <td>RT @So_Lucky_7: #축친소 #축구트친소 #축구_트친소 #토트넘트친소 #토...</td>
    </tr>
    <tr>
      <th>12</th>
      <td>2021-12-03 02:17:31</td>
      <td>RT @Spurs_KR: HM7!!!\n\n⚪️ 2-0 🐝 (65’)\n\n#THF...</td>
    </tr>
    <tr>
      <th>13</th>
      <td>2021-12-03 02:12:22</td>
      <td>콘테가 손흥민을 "새로운 유형의" 10번으로 창조 해 줬으면 좋겠다. \n대한민국 ...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>2021-12-03 02:12:14</td>
      <td>RT @Spurs_KR: 손흥민 21/22시즌 프리미어리그 5호골 (vs 브렌트포드...</td>
    </tr>
    <tr>
      <th>15</th>
      <td>2021-12-03 02:29:47</td>
      <td>RT @Spurs_KR: 여러분의 오늘경기 #MOTM 픽은 누구인가요? 🤔\n댓글로...</td>
    </tr>
    <tr>
      <th>16</th>
      <td>2021-12-03 02:29:31</td>
      <td>RT @Spurs_KR: #MondayMotivation \n\n여러분 모두 이번 ...</td>
    </tr>
    <tr>
      <th>17</th>
      <td>2021-12-03 02:28:12</td>
      <td>RT @Spurs_KR: 손흥민 21/22시즌 프리미어리그 5호골 (vs 브렌트포드...</td>
    </tr>
    <tr>
      <th>18</th>
      <td>2021-12-03 02:27:42</td>
      <td>RT @Spurs_KR: 오늘의 영웅: '손'파이더맨!! 🕷️🕸️@TomHollan...</td>
    </tr>
    <tr>
      <th>19</th>
      <td>2021-12-03 02:26:27</td>
      <td>RT @classs_77: 손흥민 어시 벤뎁 골 🤍 벤뎁 무슨일이야 👀 https:...</td>
    </tr>
  </tbody>
</table>
</div>




```python
import re

def text_cleaning(text):
    hangle = re.compile('[^ ㄱ-ㅣ 가-힣]+')
    result = hangle.sub('',text)
    return result
    
# ‘tweet_text’ 피처에 이를 적용합니다.
df['ko_text'] = df['tweet_text'].apply(lambda x: text_cleaning(x))
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>created</th>
      <th>tweet_text</th>
      <th>ko_text</th>
      <th>nouns</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-12-02 07:53:59</td>
      <td>RT @wpfkem0606: 장꾸대장 손흥민이 나가신다 길을 비켜주세요 🐥 http...</td>
      <td>장꾸대장 손흥민이 나가신다 길을 비켜주세요</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-12-02 07:51:48</td>
      <td>이게 축구로 따지면 손흥민이 아스날 유니폼 입은거랑 똑같은거 아닌가? https:/...</td>
      <td>이게 축구로 따지면 손흥민이 아스날 유니폼 입은거랑 똑같은거 아닌가</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-12-02 07:50:15</td>
      <td>RT @wpfkem0606: 사진찍는 귀여운 민재킴🐤\n그냥 귀여운 손흥민🐥 htt...</td>
      <td>사진찍는 귀여운 민재킴그냥 귀여운 손흥민</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-12-02 07:49:47</td>
      <td>손흥민의 아자디 원정 골, KFA 올해의 골 후보에 | 다음뉴스 https://t....</td>
      <td>손흥민의 아자디 원정 골  올해의 골 후보에  다음뉴스</td>
      <td>[]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-12-02 07:49:15</td>
      <td>RT @apcpark: 손흥민경기를 보니까  골결정력은  역대 한국공격수 최용수,황...</td>
      <td>손흥민경기를 보니까  골결정력은  역대 한국공격수 최용수황선홍최순호는 물론이고 ...</td>
      <td>[]</td>
    </tr>
  </tbody>
</table>
</div>




```python
from konlpy.tag import Okt
from collections import Counter

#한국어 약식 불용어 사전 
korean_stopwords_path = "/content/drive/MyDrive/muchin Learning/korean_stopwords.txt"
with open(korean_stopwords_path, encoding='utf8') as f:
    stopwords = f.readlines()
stopwords = [x.strip() for x in stopwords]

def get_nouns(x):
    nouns_tagger = Okt()
    nouns = nouns_tagger.nouns(x)
    
    # 한글자 키워드를 제거합니다.
    nouns = [noun for noun in nouns if len(noun) > 1]
    
    # 불용어를 제거합니다.
    nouns = [noun for noun in nouns if noun not in stopwords]

    return nouns
    
#ko_text 피처에 이를 적용합니다.
df['nouns'] = df['ko_text'].apply(lambda x: get_nouns(x))
print(df.shape)
df.head(10)
```

    (1485, 4)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>created</th>
      <th>tweet_text</th>
      <th>ko_text</th>
      <th>nouns</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2021-12-02 07:53:59</td>
      <td>RT @wpfkem0606: 장꾸대장 손흥민이 나가신다 길을 비켜주세요 🐥 http...</td>
      <td>장꾸대장 손흥민이 나가신다 길을 비켜주세요</td>
      <td>[대장, 손흥민]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021-12-02 07:51:48</td>
      <td>이게 축구로 따지면 손흥민이 아스날 유니폼 입은거랑 똑같은거 아닌가? https:/...</td>
      <td>이게 축구로 따지면 손흥민이 아스날 유니폼 입은거랑 똑같은거 아닌가</td>
      <td>[축구, 손흥민, 아스날, 유니폼]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2021-12-02 07:50:15</td>
      <td>RT @wpfkem0606: 사진찍는 귀여운 민재킴🐤\n그냥 귀여운 손흥민🐥 htt...</td>
      <td>사진찍는 귀여운 민재킴그냥 귀여운 손흥민</td>
      <td>[사진, 민재, 그냥, 손흥민]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2021-12-02 07:49:47</td>
      <td>손흥민의 아자디 원정 골, KFA 올해의 골 후보에 | 다음뉴스 https://t....</td>
      <td>손흥민의 아자디 원정 골  올해의 골 후보에  다음뉴스</td>
      <td>[손흥민, 원정, 올해, 후보, 뉴스]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2021-12-02 07:49:15</td>
      <td>RT @apcpark: 손흥민경기를 보니까  골결정력은  역대 한국공격수 최용수,황...</td>
      <td>손흥민경기를 보니까  골결정력은  역대 한국공격수 최용수황선홍최순호는 물론이고 ...</td>
      <td>[손흥민, 경기, 결정, 역대, 한국, 공격수, 최용수, 황선홍, 최순호, 차범근,...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2021-12-02 07:49:03</td>
      <td>RT @wpfkem0606: 다시 한번 해줬으면 하는 손흥민  끄덕끄덕 골 세레모니...</td>
      <td>다시 한번 해줬으면 하는 손흥민  끄덕끄덕 골 세레모니</td>
      <td>[다시, 한번, 손흥민, 끄덕, 끄덕, 세레모니]</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2021-12-02 07:49:00</td>
      <td>RT @wpfkem0606: 장꾸대장 손흥민이 나가신다 길을 비켜주세요 🐥 http...</td>
      <td>장꾸대장 손흥민이 나가신다 길을 비켜주세요</td>
      <td>[대장, 손흥민]</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2021-12-02 07:48:54</td>
      <td>RT @wpfkem0606: 내맘대로 오늘 뽑은 손흥민 잘생긴 모먼트🐥 https:...</td>
      <td>내맘대로 오늘 뽑은 손흥민 잘생긴 모먼트</td>
      <td>[오늘, 손흥민, 모먼트]</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2021-12-02 07:48:50</td>
      <td>RT @wpfkem0606: 1일 1아스날전 원더골 보기☝🏻\n나의 손흥민 입덕 골...</td>
      <td>일 아스날전 원더골 보기나의 손흥민 입덕 골장면</td>
      <td>[아스날, 원더, 손흥민, 입덕, 장면]</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2021-12-02 07:48:23</td>
      <td>RT @GulliverPicture: 손흥민-은돔벨레, 토트넘의 개그 콤비?..훈련...</td>
      <td>손흥민은돔벨레 토트넘의 개그 콤비훈련장 분위기 화제</td>
      <td>[손흥민, 벨레, 토트넘, 개그, 콤비, 훈련, 분위기, 화제]</td>
    </tr>
  </tbody>
</table>
</div>




```python
print(df['nouns'])
```

    0                                               [대장, 손흥민]
    1                                     [축구, 손흥민, 아스날, 유니폼]
    2                                       [사진, 민재, 그냥, 손흥민]
    3                                   [손흥민, 원정, 올해, 후보, 뉴스]
    4       [손흥민, 경기, 결정, 역대, 한국, 공격수, 최용수, 황선홍, 최순호, 차범근,...
                                  ...                        
    1480    [역시, 손흥민, 세계, 최고, 포워드, 선정, 네이마르, 아래, 해외, 축구, 스포츠]
    1481                                   [손흥민, 세계, 포워드, 순위]
    1482    [폭설, 휴식, 취한, 손흥민, 승격, 상대로, 득점, 시동, 리그, 경기, 침묵,...
    1483                           [손흥민, 토트넘, 올해, 선수, 후보, 항상]
    1484                        [타의, 추종, 불허, 해외, 매체, 극찬, 손흥민]
    Name: nouns, Length: 1485, dtype: object



```python
!pip install apyori
```

    Collecting apyori
      Downloading apyori-1.1.2.tar.gz (8.6 kB)
    Building wheels for collected packages: apyori
      Building wheel for apyori (setup.py) ... [?25l[?25hdone
      Created wheel for apyori: filename=apyori-1.1.2-py3-none-any.whl size=5974 sha256=504db5feb1e0fde3096c0ea27288753dbc838eb075498b71b6e9956e4524d048
      Stored in directory: /root/.cache/pip/wheels/cb/f6/e1/57973c631d27efd1a2f375bd6a83b2a616c4021f24aab84080
    Successfully built apyori
    Installing collected packages: apyori
    Successfully installed apyori-1.1.2



```python
from apyori import apriori

# 장바구니 형태의 데이터(트랜잭션 데이터)를 생성합니다.
transactions = [
    ['손흥민', '시소코'],
    ['손흥민', '케인'],
    ['손흥민', '케인', '포체티노']
]

# 연관 분석을 수행합니다.
results = list(apriori(transactions))
for result in results:
    print(result)
```

    RelationRecord(items=frozenset({'손흥민'}), support=1.0, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'손흥민'}), confidence=1.0, lift=1.0)])
    RelationRecord(items=frozenset({'시소코'}), support=0.3333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'시소코'}), confidence=0.3333333333333333, lift=1.0)])
    RelationRecord(items=frozenset({'케인'}), support=0.6666666666666666, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'케인'}), confidence=0.6666666666666666, lift=1.0)])
    RelationRecord(items=frozenset({'포체티노'}), support=0.3333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'포체티노'}), confidence=0.3333333333333333, lift=1.0)])
    RelationRecord(items=frozenset({'손흥민', '시소코'}), support=0.3333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'손흥민', '시소코'}), confidence=0.3333333333333333, lift=1.0), OrderedStatistic(items_base=frozenset({'손흥민'}), items_add=frozenset({'시소코'}), confidence=0.3333333333333333, lift=1.0), OrderedStatistic(items_base=frozenset({'시소코'}), items_add=frozenset({'손흥민'}), confidence=1.0, lift=1.0)])
    RelationRecord(items=frozenset({'손흥민', '케인'}), support=0.6666666666666666, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'손흥민', '케인'}), confidence=0.6666666666666666, lift=1.0), OrderedStatistic(items_base=frozenset({'손흥민'}), items_add=frozenset({'케인'}), confidence=0.6666666666666666, lift=1.0), OrderedStatistic(items_base=frozenset({'케인'}), items_add=frozenset({'손흥민'}), confidence=1.0, lift=1.0)])
    RelationRecord(items=frozenset({'손흥민', '포체티노'}), support=0.3333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'손흥민', '포체티노'}), confidence=0.3333333333333333, lift=1.0), OrderedStatistic(items_base=frozenset({'손흥민'}), items_add=frozenset({'포체티노'}), confidence=0.3333333333333333, lift=1.0), OrderedStatistic(items_base=frozenset({'포체티노'}), items_add=frozenset({'손흥민'}), confidence=1.0, lift=1.0)])
    RelationRecord(items=frozenset({'포체티노', '케인'}), support=0.3333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'포체티노', '케인'}), confidence=0.3333333333333333, lift=1.0), OrderedStatistic(items_base=frozenset({'케인'}), items_add=frozenset({'포체티노'}), confidence=0.5, lift=1.5), OrderedStatistic(items_base=frozenset({'포체티노'}), items_add=frozenset({'케인'}), confidence=1.0, lift=1.5)])
    RelationRecord(items=frozenset({'손흥민', '포체티노', '케인'}), support=0.3333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'손흥민', '포체티노', '케인'}), confidence=0.3333333333333333, lift=1.0), OrderedStatistic(items_base=frozenset({'손흥민'}), items_add=frozenset({'포체티노', '케인'}), confidence=0.3333333333333333, lift=1.0), OrderedStatistic(items_base=frozenset({'케인'}), items_add=frozenset({'손흥민', '포체티노'}), confidence=0.5, lift=1.5), OrderedStatistic(items_base=frozenset({'포체티노'}), items_add=frozenset({'손흥민', '케인'}), confidence=1.0, lift=1.5), OrderedStatistic(items_base=frozenset({'손흥민', '케인'}), items_add=frozenset({'포체티노'}), confidence=0.5, lift=1.5), OrderedStatistic(items_base=frozenset({'손흥민', '포체티노'}), items_add=frozenset({'케인'}), confidence=1.0, lift=1.5), OrderedStatistic(items_base=frozenset({'포체티노', '케인'}), items_add=frozenset({'손흥민'}), confidence=1.0, lift=1.0)])



```python
# 지지도 0.5, 신뢰도 0.6, 향상도 1.0 이상이면서 (손흥민, 케인) 처럼 규칙의 크기가 2 이하인 규칙을 추출합니다.
list(apriori(transactions,
             min_support=0.5,
             min_confidence=0.6,
             min_lift=1.0,
             max_length=2))
```




    [RelationRecord(items=frozenset({'손흥민'}), support=1.0, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'손흥민'}), confidence=1.0, lift=1.0)]),
     RelationRecord(items=frozenset({'케인'}), support=0.6666666666666666, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'케인'}), confidence=0.6666666666666666, lift=1.0)]),
     RelationRecord(items=frozenset({'손흥민', '케인'}), support=0.6666666666666666, ordered_statistics=[OrderedStatistic(items_base=frozenset(), items_add=frozenset({'손흥민', '케인'}), confidence=0.6666666666666666, lift=1.0), OrderedStatistic(items_base=frozenset({'손흥민'}), items_add=frozenset({'케인'}), confidence=0.6666666666666666, lift=1.0), OrderedStatistic(items_base=frozenset({'케인'}), items_add=frozenset({'손흥민'}), confidence=1.0, lift=1.0)])]




```python
# 트랜잭션 데이터를 추출합니다.
transactions = df['nouns'].tolist()
transactions = [transaction for transaction in transactions if transaction] # 공백 문자열을 방지합니다.
print(transactions)
```

    [['대장', '손흥민'], ['축구', '손흥민', '아스날', '유니폼'], ['사진', '민재', '그냥', '손흥민'], 
    ...
    ['손흥민', '토트넘', '올해', '선수', '후보', '항상'], ['타의', '추종', '불허', '해외', '매체', '극찬', '손흥민']]



```python
# 연관 분석을 수행합니다.
results = list(apriori(transactions,
                       min_support=0.05,
                       min_confidence=0.1,
                       min_lift=5,
                       max_length=2))
print(results)
```

    [RelationRecord(items=frozenset({'포워드', '세계'}), support=0.13333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset({'세계'}), items_add=frozenset({'포워드'}), confidence=1.0, lift=7.5), OrderedStatistic(items_base=frozenset({'포워드'}), items_add=frozenset({'세계'}), confidence=1.0, lift=7.5)]), RelationRecord(items=frozenset({'후보', '올해'}), support=0.13333333333333333, ordered_statistics=[OrderedStatistic(items_base=frozenset({'올해'}), items_add=frozenset({'후보'}), confidence=1.0, lift=7.5), OrderedStatistic(items_base=frozenset({'후보'}), items_add=frozenset({'올해'}), confidence=1.0, lift=7.5)])]



```python
#데이터 프레임 형태로 정리합니다.
columns =['source','target','support']
network_df = pd.DataFrame(columns=columns)

#규칙의 조건절을 source 결과절을 target 지지도를 support라는 데이터 프레임의 피처로 변환합니다.
for result in results:
    if len(result.items) ==2:
        items = [x for x in result.items]
        row = [items[0],items[1],result.support]
        series = pd.Series(row, index = network_df.columns)
        network_df = network_df.append(series, ignore_index=True)

network_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>source</th>
      <th>target</th>
      <th>support</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>포워드</td>
      <td>세계</td>
      <td>0.133333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>후보</td>
      <td>올해</td>
      <td>0.133333</td>
    </tr>
  </tbody>
</table>
</div>




```python

```

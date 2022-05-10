---
title: "Github 블로그 만들기"
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
  - github
---



# **Github 블로그 만들기**

**1. 새로운 Repository를 만든다.** 

![생성1](\github_img\생성1.png)

new 버튼을 눌러준다~



 **2. Repository 이름을 설정해준다.**

> 반드시 **username.github.io** 이런식으로 만들어주세요.

![생성2](\github_img\생성2.png)

저는 UserName이 Myeong1234라서 **Myeong1234.github.io**로 만들어 주었습니다.

공식문서에서 저장소의 저 username부분이

 **사용자 이름과 정확히 일치하지 않으면 작동하지 않을 수 있다**고 하니 일치하도록 만들어 줍니다

 **Add a README file**은 체크 해도 되고 안해도 됩니다.



 **3. clone하기**

![생성3](\github_img\생성3.png)

저는 만들어 놓은 블로그가 있어서 항목들이 있는데 없는게 정상입니다

주소를 복사해 주시고 터미널창(명령 프롬프트)을 열어줍니다.

우선 블로그 파일을 저장할 위치를 C에 apps로 하겠습니다.

터미널창에서 `cd /` 입력해주시고 `mkdir apps&cd apps` 해서 폴더 생성 후 이동을 해줍니다.

(mkdir은 폴더를 생성해주는 명령어, cd는 해당폴더로 이동하는 명령어 입니다.)

`git colne 복사한주소` 입력해 줍니다.

저는 `git colne https://github.com/Myeong1234/Myeong1234.github.io.git `이 되겠네요

![폴더](\github_img\폴더.png)

그림처럼 폴더하나가 생기고 파일이 안에 파일들이 생성 되었습니다.



**5. clone한 폴더로 이동한다음 파일 생성**

```
cd username.github.io  // username 은 본인 아이디
echo "Hello World" > index.html
```

입력해 줍니다.  해당폴더에 index.html이 만들어졌으면 된겁니다.



**6. Push**(컴퓨터에서 작업한 내용을 github에 복사)

```
git add --all
git commit -m "Initial commit"
git push -u origin main
```

github사이트로 와서

![기본](\github_img\기본.png)

위 페이지에서 index.html ,(README.md) 가 있으면 잘 된겁니다.

잘 만들어 졌는지 확인을 위해서 `username.github.io`를 브라우저 주소창에 입력해 봅니다.

저같은경우 `Myeong1234.github.io`가 되겠네요

빈페이지에 username.github.io만 적혀있는 사이트가 떴다면 생성 완료입니다~



# **Jekyll 사용해 블로그 형식으로 만들어보기**

간단한 jekyll 설치 및 간단한 화면을 보겠습니다.

(jekyll 내용은 https://jekyllrb-ko.github.io/ 사이트에서 확인 하실 수 있습니다.)

**1. 로컬에 Jekyll을 설치** 

좀 전에 사용했던 터미널창을 열어주시고

```
gem install jekyll bundler
```

입력 해줍니다. (jekll설치)

**2. index.html제거**

username.github.io 폴더로 이동 후

 잘 생성되었는지 확인을 위해 만들었던 index.html을 삭제 해 줍니다.

터미널 창에서 앞부분이 `C:\apps\Myeong1234.github.io>`로 되어 있는지 확인해주고 아래코드를 입력합니다.

(만약 안되어있다면 `cd /&cd apps&cd Myeong1234.github.io`입력 해주시면 되겠습니다.)

```
jekyll new ./
```

 **4. bundle install**

```
bundle install
```

터미널에서 입력해주세요~



**5. Jekyll을 로컬서버에 띄우기**

```
bundle exec jekyll serve
```

입력해주면 `Server address: http://127.0.0.1:4000/`라는 부분이 보이실껀데 주소를 복사해서 

브라우저창에 입력을 해주면 

![제키](\github_img\제키.png)

요런 창이 나오면 성공 입니다.



**6. github에 push**

푸시하지 않고 브라우저창에 username.github.io를 하게되면 hello world만 있는 창을 볼 수 있을겁니다.

(아직 PC에서 작업한 내용을 github에 올려주지 않아서 그렇습니다.)

```
git add .
git commit -m "본인의 커밋 메세지"
git push
```

3가지 명령어를 입력해 주게되면 github에 올라가게 되고 브라우저창에 username.github.io를 하게되면 

위에 있는 그림 파일과 동일한 화면이 보이게 되겠습니다.





# **Github 블로그 꾸미기**

테마를 복사해와서 좀더 이쁘게 블로그를 꾸미는 방법으로 진행해 보겠습니다.

#### **1.테마 사이트**

​	**[jamstackthemes.dev](https://jamstackthemes.dev/ssg/jekyll/)**

​	**[jekyllthemes.org](http://jekyllthemes.org/)**

​	**[jekyllthemes.io](https://jekyllthemes.io/)**

​	**[jekyll-themes.com](https://jekyll-themes.com/)**

위 사이트에서 하나를 선택해줍니다.



#### **2.다운로드**

[github.com/thelehhman/plainwhite-jekyll](github.com/thelehhman/plainwhite-jekyll)로 선택해서 진행 해보겠습니다.

![테마적용](\github_img\테마적용.png)

선택하신 테마의 깃허브로 가서 

1. code를 눌러주시고 
2. Download Zip을 눌러서 다운로드 해줍니다. 
3. 다운받은 파일을 압축을 풀어줍니다.
4. 압축 받은 파일들을 전체 복사해 줍니다 

![파일복사](\github_img\파일복사.png)

5. 복사했던 파일을 위에서 만들었던 username.github.io 폴더에 붙여넣기 해줍니다. 
6. 터미널 창으로 가서 bundle install 과 bundle exec jekyll serve을 입력해 줍니다.

```
bundle install
bundle exec jekyll serve
```

7. 입력 후 브라우저창으로 가서 http://127.0.0.1:4000 주소를 입력해주면 아까 복사해 온 테마가 아래 그림처럼적용 되었을 겁니다.

![테마적용이미지](\github_img\github_img\테마적용이미지.png)



#### **3. 블로그 설정**

github.io폴더에 있는 _congif.yml을 열어줍니다

![코드변경](\github_img\코드변경.png)

위 그림처럼 열릴텐데 title, email,name, tagline 등을 자신의 정보로 변환해 주고 저장을 해줍니다.

이미지를 바꾸고 싶다면 github.io/assets에 원하는 이미지 파일을 넣어주고 

_congif.yml에 portfolio_image 항목에 파일이름을 변경해줍니다.

변경을 완료했다면 터미널창에 bundle exec jekyll serve을 입력해줍니다.(보통 자동으로 리로드 되어서 안해줘도됨)



#### **4. Post추가**

github.io/_posts 폴더에 원하는 내용을 markdown형식으로 작성 후 저장해주면 되겠습니다.

(참고 markdown을 사용할때 최 상단에 yam front matter 입력방식을 넣어주면 좋다.)

![yam파일](\github_img\yam파일.png)

#### **5. github에 push**

변경할 것들을 완료했다면 깃허브에 올려주도록 하겠습니다. 

터미널 창에 아래 코드들을 입력해 줍니다.

```
git add .
git commit -m "본인의 커밋 메세지" // 메세지는 아무거나 입력하셔도 됩니다.
git push
```

push가 완료 되었다면 브라우저창에 username.github.io 입력해주고 잘 되었는지 확인해 줍니다.



### 이상으로 깃허브 블로그 만들기를 마치겠습니다.


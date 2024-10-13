---
layout: single
title:  "깃 허브 블로그 만들기"
typora-root-url: ../

---

# 깃 허브 블로그

미루고 미루던 깃허브 블로그를 만들어 보았다. 여러 블로그를 참고해보았지만, A to Z 까지의 과정이 없는 느낌이라 이곳 저곳을 돌아다닌 후 '테디노트'님의 '깃헙(GitHub) 블로그 10분안에 완성하기' 영상을 참고하였다. [깃헙(GitHub) 블로그 10분안에 완성하기](https://youtu.be/ACzFIAOsfpM?si=2FlYV7IyHBXz2S5f)


1. 먼저 [jekyll 테마 리스트](https://github.com/topics/jekyll-theme) 여기서 마음에 드는 테마를 찾은 후 자신의 레포지토리로 Fork 한다. (Star 옆에 있음)

  ![jekyll-theme](/images/2024-10-13-github_blog/jekyll-theme-8817056.png)


2. 레포지토리 이름을 자신의 유저네임.github.io 로 설정한다. ex) myeonguklee.github.io

   ![Fork-name](/images/2024-10-13-github_blog/Fork-name-8817201.png)

   * 잘못 적었어도 괜찮다. settings에 가서 rename을 해주면 된다. 나도 t 빼먹었다.

     ![Rename](/images/2024-10-13-github_blog/Rename.png)


3. _config.yml 파일에서 25번째 줄의 url을 수정 (레포지토리 메인에 있는 _config.yml, 오른쪽 상단에 Raw 오른쪽에 있는 연필 모양 클릭! 수정 후 commit chages... 클릭)

  ![config-url_edit](/images/2024-10-13-github_blog/config-url_edit.png)


4. 새로운 글 작성

   ![create](/images/2024-10-13-github_blog/create.png)

  * Name your file.. 에 _posts/{연도-월-일-파일이름} 적기
  *  layout: single <br>title: "포스팅 제목" 
  * 위와 같이 적어주고 본문 적기
    ![post](/images/2024-10-13-github_blog/post.png)
  * (초록색의) Commit changes... 누르기

5. {username}.github.io 주소 입력 후 첫 포스팅 확인하기 (업로드 되는데 짧게는 30초 정도 걸리니 조금 기다리기)
   ![github-blog](/images/2024-10-13-github_blog/github-blog.png)


마크다운 문서 편집기 - [typora 에디터](https://typora.io/)
  예전(베타 버전)에는 무료였으니 지금은 15일 무료 후 1회성 결제 필요(15 달러), 아마 좀 더 사용해보고 결제하지 않을까 싶다.



참고하면 좋을 사이트

[jekyll docs](https://jekyllrb.com/docs/posts/)

[jekyll 테마 리스트](https://github.com/topics/jekyll-theme)

[제일 유명한 테마의 docs](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)

[테디노트 님의 마크다운 문법](https://teddylee777.github.io/jekyll/Jekyll-%EC%82%AC%EC%9A%A9%EC%9D%84-%EC%9C%84%ED%95%9C-markdown-%EB%AC%B8%EB%B2%95/#google_vignette) - 마크다운 문법은 매번 까먹어 ㅜ

[Typora 이미지 업로드 방법](https://peterica.tistory.com/545)



출처 - 깃헙 블로그 만들때 너무 많은 도움이 되었다. 바로 좋구 박아드렸다.

[테디노트님의 깃헙(GitHub)블로그 10분안에 완성하기 영상](https://youtu.be/ACzFIAOsfpM?si=jpIlmTTMh-7RJ8XJ)

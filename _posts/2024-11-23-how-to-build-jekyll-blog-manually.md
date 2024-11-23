github blog를 수동으로 빌드하기. (github actions로 빌드가 되지 않을 때)
==
나의 삽질 과정
--------
일반적인 경우에는 github에서 chirpy theme을 fork한 후에, repo name을 username.github.io으로 설정하면 자동으로 빌드가 된다.   
따라서 위의 과정만 수행하면, 해당 url에서 다음과 같은 chirpy의 메인화면이 출력되는 것을 볼 수 있다.
![s18331711232024](https://a.okmd.dev/md/6741a15fc8c3b.png)
그러나 나는 index.html의 내용인 "layout: home # Index page"라는 텍스트 밖에 뜨지 않았다.   
길고 긴 구글링을 통해 jekyll을 통한 빌드가 제대로 되지 않으면 위의 상황이 발생한다는 것을 알 수 있었다.   
그래서 구글링을 하던 도중 다음과 같은 해결책을 발견했다.
1. Github Setting - Pages - Bulid and development 에서 Source를 Github Actions 로 변경 이후, jekyll.yml 커밋.
2. 1번 과정에서 build 오류가 날 시, 로컬에서 다음의 명령어를 입력한 후 chirpy source를 커밋 & 1번 과정 재시도. 

        $ bundle lock --add-platform x86_64-linux
그러나 위의 해결책에도 불구하고 나는 jekyll.yml을 커밋하는 과정에서 빌드오류가 발생했다.   
결국 나는 github actions에서 제공해주는 빌드 기능을 사용하지 않고, 수동으로 local에서 빌드해서 커밋하기로 했다. ~~정말 기분이 더러웠다.~~

수동으로 jekyll package 빌드하기
-------
제목에서는 github blog라는 다소 애매한 용어을 사용했지만, 사실 대부분의 github 블로그 테마는 jekyll package이다.   
jekyll package를 수동으로 빌드해서 github에 업로드하려면, gh-pages라는 브랜치를 생성 후, 이 브랜치에 **_site폴더만** 커밋해야 한다.   
_site폴더는 로컬에서 빌드하면 생성되는 폴더인데, c언어의 실행 파일과 같은, jekyll package의 빌드 후 생성된 정적 결과물이라고 볼 수 있다.   
1. 패키지 빌드   
로컬의 패키지 루트 폴더에서 다음과 같이 패키지를 빌드한다.

        $ bundle exec jekyll build
1. _site폴더가 생성되었을 것이다. _site폴더에서 다음의 명령들을 입력해서 gh-pages 브랜치에 _site 폴더의 파일들을 push한다.

        git init
        git remote add origin "ssh repo url"
        git checkout -b gh-pages
        git add .
        git commit -m "deploy"
        git push -u origin gh-pages --force
2. github settings - pages - source 섹션에서 gh-pages 브랜치를 선택한다.   

이제 정상적으로 웹 사이트가 작동하는 것을 확인할 수 있다.

### 출처
<hr>

<https://friendlyvillain.github.io/posts/chirpy-setup/>   
chatgpt

## 로컬 환경 셋팅하기

### 1. 레파지토리 clone
````
$git clone https://github.com/devtutoring/app-publishing.git
$cd app-publishing/
````
### 2. node, gulp 버전 확인
````
$npm init

$node -v
v10.16.0

$gulp -v
CLI version: 2.2.0
Local version: 4.0.1
````
### 3. gulp 설치하기
````
$npm install gulp-cli
$npm install gulp@4.0.1
````
### 4. 플러그인 설치 후 실행
````
$npm install
$npm install postcss autoprefixer cssnano gulp-sourcemaps brower-sync --save-dev

$gulp 또는 
$npm run gulp
````
브라우저에서 localhost:3000 가 새창으로 열리면 성공☺️
  
  
&nbsp;  
## 파일 목록(위치)
### HTML 
````
├── app/                           # app 3.0
│   ├── desktop/                   # app 3.0 데스크탑 버전
├── event-page/                    # 프로모션 페이지
├── global/                        # 튜터링고(영문/중문)
├── mobile/                        # 미확인                      
│   ├── common/modal/           
│   ├── lessonTicket/           
│   ├── publishing/             
│   ├── srcRefactorying/    
├── service-assess/                # 서비스 평가        
├── web/                           # tutoring.co.kr
│   ├── b2bProgram/                # b2bProgram
│   ├── bridge/                    # 브릿지 페이지
````

&nbsp; 
### CSS/SCSS
**CSS 파일은 직접 수정하지 않습니다!!!**  
서비스별 메인 scss 파일은 scss/ 루트 밑에 서비스이름.scss 파일로 저장합니다.  
서비스별 scss 파일은 컴포넌트 단위별로 가능한 짧게 쪼개어 pages/ 폴더 밑에 저장합니다.  
````
├── assets/                           
│   ├── css/
│   ├── scss/
│   │   ├── base/                  
│   │   │   ├── _reset.scss            # 태그 초기화 - 사용을 지양한다
│   │   ├── components/                # 공통 컴포넌트
│   │   │   ├── _app_lading.scss
│   │   │   ├── _login.scss
│   │   │   ├── _modal.scss
│   │   │   ├── _signUp.css
│   │   │   ├── 기타
│   │   ├── fonts/                     # 웹폰트 - 필요한 폰트만 @import 한다
│   │   │   ├── _HGCGothicssi.scss  
│   │   │   ├── _NotoSans.scss
│   │   │   ├── _SpoqaHanSans.scss
│   │   ├── layout/                    # 공통 레이아웃 - web
│   │   │   ├── _common.scss
│   │   │   ├── _footer.scss
│   │   │   ├── _header.scss
│   │   ├── pages/
│   │   │   ├── 서비스이름1/
│   │   │   ├── 서비스이름2/
│   │   │   ├── 서비스이름3/
│   │   ├── utils/                     # 공통 변수, mixin 선언
│   │   │   ├── _mixin.scss
│   │   │   ├── _variables.scss
│   │   ├── vendors/                   # 외부 플러그인 사용시 @import 한다
│   │   │   ├── _slick.scss
│   │   │   ├── _swiper.scss
│   ├── 서비스이름1.scss
│   ├── 서비스이름2.scss
│   ├── 서비스이름3.scss
````

&nbsp; 
## gulpfile.js
###  1.scss 빌드 시 sourcemap이 필요한 경우
1. `.pipe(sourcemaps.write())` 을 활성화  
2. `.pipe(sourcemaps.write('./', {addComment: false}))` 을 주석처리  
3. **origin에 push 하기 전, 꼭 rollback 주세요!!**

````
function style() {
    return gulp
        .src(paths.styles.src)
        .pipe(sourcemaps.init())
        .pipe(sass())
        .on("error", sass.logError)
        .pipe(postcss([autoprefixer(), cssnano()]))
        .pipe(sourcemaps.write())                             // sourcemaps ON
        .pipe(sourcemaps.write('./', {addComment: false}))    // sourcemaps OFF
        .pipe(gulp.dest(paths.styles.dest))
        .pipe(browserSync.stream());
}
````

&nbsp; 
## 브랜치명 사용 규칙 
1. feature 브랜치는 항상 master 브랜치에서 생성합니다.
2. feature 브랜치명은 지라 번호로 생성합니다. (예: WEB-2668)    
3. 지라 번호가 없는 경우 임의의 이름으로 생성하고, 지라가 생성된 후 꼭!!! 변경해 줍니다.
4. feature 브랜치에서는 완료될 때 까지 해당 브랜치에 master 브랜치를 역으로 merge 하지 않습니다.
4. **master 브랜치에 직접 push 하거나 merge 하지 않습니다.**  
5. master 브랜치에 feature 브랜치를 merge할 경우 되도록이면 [Pull requests](https://github.com/devtutoring/app-publishing/pulls) 기능을 사용합니다.  
6. Pull requests 에서 Conflict이 발생한 경우 :
    1. Conflict이 발생한 Pull requests는 closed 처리합니다.
    2. Conflict 해결을 위한 브랜치를 master 브랜치에서 새로 생성합니다. (예: WEB-2668-conflict)
    3. 새로 생성한 브랜치(예: WEB-2668-conflict)에 feature 브랜치(예: WEB-2668)를 Merge 후 수동으로 Conflict를 해결합니다.
    4. 새로 생성한 브랜치(예: WEB-2668-conflict)를 master 브랜치로 Pull requests를 작성하여 Merge 합니다.
7. Conflict를 해결을 위한 브랜치(예: WEB-2668-conflict)는 Merge 후 바로 삭제합니다.
````
// Conflict이 발생한 Pull request를 closed 처리한 후 
$git checkout master
$git pull origin master
$git checkout -b WEB-2668-conflict

// 브랜치가 WEB-2668-conflict에 있는 상태
$git merge WEB-2668
$git commit -m 'Merge branch 'WEB-2668'
$git push origin WEB-2668-conflict

로컬에서 수동 Merge 완료 후,
github에서 WEB-2668-conflict 브랜치를 master 브랜치로 Pull requests 요청을 보냅니다.  

````

&nbsp; 
## 퍼블리싱 가이드
html 클래스명 작성 규칙, CSS 선언 규칙 등은 아래 위키페이지를 참고하세요.  
https://github.com/devtutoring/app-publishing/wiki

&nbsp;  
## 기타
- st 서버 : http://st.tutoring.co.kr/app-publishing/
- dev 서버 : http://dev-app.tutoring.co.kr/
- joyte 서버 : https://joyte-app.tutoring.co.kr/
- real 서버 : https://app.tutoring.co.kr/

````
// URL 예시
- real > 나의 코스 : https://app.tutoring.co.kr/app/home/lms/course 
- joyte > 케이크 프로모션 : https://joyte-app.tutoring.co.kr/home/plan/courses/cake
- dev > b2b 프로그램 : http://dev-app.tutoring.co.kr/home/b2bProgram
````

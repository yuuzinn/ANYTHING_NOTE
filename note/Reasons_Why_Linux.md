## Linux(리눅스)를 사용하는 이유는 무엇일까? (기본 명령어  추가)

이번 팀 프로젝트에서도 그렇고, 대다수의 기업들은 OS로 Linux를 많이 사용한다. 

실제로 채용 과정에서도 채용 공고에 Linux를 사용할 줄 아는 자는 우대 사항에 적혀 있을 정도로 많이 사용하는 편이다.
나는 리눅스를 거의 사용해본 적이 없어 잘 모르지만, GUI가 아닌 CLI기반이라 명령어를 알아야만 사용할 수 있기에, 명령어를 모르는 나는 
사용하기엔 아직 버겁다.

그리고 가장 중요한, 리눅스를 사용하는 이유를 잘 모르기에 해당 글을 작성한다.

여러 구글링을 통해 아래와 같은 이유들을 알 수 있었다. 아래는 [13 Reasons why Linux is better than Windows](https://medium.com/swlh/13-reasons-why-linux-is-better-than-windows-6fa304454ae)를 통해 적는다.

- 많은 개발자가 오픈 소스로 된 코드에 참여하기 때문에 버그가 금방 고쳐지며, 안정적이고 안전한 시스템으로 유지된다.
  - 오픈소스로 구성되어 있어, 바이러스와 같은 공격이 많기에 위험하다 생각할 수 있지만 오히려 안전하다. 그만큼 많은 바이러스 공격에 
  대비하여 운영해왔기 때문. 위와 같은 이유도 된다.
  - 게다가 무료이다.
- 개발자 친화적이다. Linux에 SoftWare를 설치하는 것은 Windows보다 쉽다. 예를 들어, Ubuntu의 apt 명령어 같은 것들
- 강력한 도구들이 사전에 설치되어 있다. SSH에 대한 기본 자원이 함께 제공된다.
- System Update를 제어할 수 있다. Windows는 Update를 비교적 강제로 하는 편이다.
- Terminal에 익숙해진다면 작업 자동화로 효율적인 환경을 만들 수 있다.
- 높은 자유도를 가진 커스터마이징 UI 만들기 가능
- 낮은 수준의 하드웨어에서도 운영이 가능하다.
- 오류의 원인을 정확하게 알려주는 로그를 제공한다. 이 로그를 통해 사용자는 검색과 해결이 가능하다.

---
## 리눅스 기본 명령어 
아래는 리눅스의 기본 명령어 모음이다. 

### pwd

`pwd`는 print working directory의 약자로, 현재 위치하고 있는 경로를 나타낸다. 리눅스에서 경로는 디렉토리를 의미하고, `pwd`를 입력하면
해당 명령어를 입력한 디렉토리를 출력한다.

### mkdir

`mkdir`은 make directory의 약자로, 현재 위치에서 새로운 디렉토리를 만들 때 사용한다. `mkdir 디렉토리_이름` 형식으로 사용한다.

여러 개의 디렉토리를 만들 때에는 만들어질 디렉토리의 이름을 공백으로 잘 구분하여 나열한다.

`mkdir directory1 directory2 directory3` 이런 식이다.

### rmdir

`rmdir`은 remove directory의 약자로, 특정 디렉토리를 삭제할 때 사용한다. 사용법은 `mkdir`과 같다.

참고로 `rmdir`은 내용물이 비어있는 디렉토리만 삭제할 수 있다. 내용물이 존재하는 디렉토리를 사용할 때에는 이후에 있는 `rm` 명령어를 사용한다.

### ls

`ls`는 list의 약자로, 현재 위치한 디렉토리 내에 존재하는 모든 파일 및 하위 디렉토리의 목록을 출력한다.

`ls`에는 다양한 옵션들을 붙여 사용할 수 있다. 대표적으로 `-l`과 `-a`를 많이 사용한다. `ls -l`를 입력하면 각 파일 및 디렉토리의 세부 정보까지 출력할 수 있다. 
이 때 출력되는 세부 정보는 권한, 소유자, 그룹, 용량, 생성 시각 등의 정보를 포함한다.

또한 `ls -a`를 입력하면 숨김처리된 디렉토리들까지 목록 상에 조회할 수 있다. 리눅스에서 파일명 앞에 `.`를 붙이면 해당 파일을 숨길 수 있다.

여러 개의 옵션을 조합해 사용할 수 있다. 옵션을 조합할 때에는 `-`옆에 사용하고자 하는 옵션을 의미하는 알파벳을 나열해준다.

`ls -a`와 `ls -al`의 결과를 보면, `.`과 `..`를 확인할 수 있는데, `.`는 현재 디렉토리, `..`는 현재 위치한 디렉토리의 상위 디렉토리를 의미한다.

### cd

`cd`는 change directory의 약자로, 다른 디렉토리로 이동할 때 사용한다. `cd directory1`는 사용 예시이다.

다시 이전의 디렉토리로 돌아가려면 `cd ..`를 입력하면 된다.

경로를 이동함에 있어 절대 경로와 상대 경로가 있다. 

`절대 경로`는 리눅스의 최상위 디렉토리인 루트 디렉토리(`/`)를 기준으로 특정 디렉토리의 경로를 지칭하는 개념이다. `pwd`를 입력할 때 나타나는 경로가 절대 경로이다.
반면, 상대 경로는 현재 위치를 기준으로 타겟 디렉토리의 경로를 지칭한다.

예를 들어서 directory1에 위치한 상태에 directroy2로 이동하려면 아래와 같이 입력한다.

`cd /Users/사용자/Desktop/linux/directory2`

반면에 아래와 같이 현재 위치를 기준으로 경로를 입력하면 상대 경로가 된다.

`cd ../directory2`

### touch

`touch`는 파일의 생성 날짜 및 시각을 수정할 때에 사용하는 명령어이지만, 내용물이 비어 있는 파일을 생성할 때에도 사용한다.

`touch helloWorld.txt` 를 입력하면, 해당 이름과 확장자를 가진 파일을 만들 수 있다. 여기서 `pwd`의 실행 결과를 helloWorld.txt 파일에 저장하고자 한다.

어떤 명령어의 실행 결과를 파일 내에 저장할 때에는 `>`를 사용한다. 리눅스에서 명령어를 입력했을 때 아무 것도 출력되지 않았다면 이는 성공했음을 의미한다.

`pwd > helloWorld.txt`

### cat

`cat`은 concatenate의 약자로, 여러 파일의 내용을 연결하여 출력한다. 특정 파일의 내용을 간단하게 확인해보고자 할 때 많이 사용한다.

아래는 helloWorld.txt의 내용을 출력하는 명령어이다.

`cat helloWorld.txt`

### mv

`mv`는 move의 약자로, 파일을 이동시킬 때와 파일의 이름을 변경할 때 사용한다. 먼저 파일을 이동시킬 때에는 `mv 이동_대상_파일 이동시키고자_하는_디렉토리`를 입력한다.

`mv helloWorld.txt direcotry2`

파일 이름을 변경하고자 할 때에는 `mv 변경_대상_파일_이름 변경될_파일_이름`을 입력한다.

`mv helloWorld.txt hi.txt`

### cp 

`cp`는 copy의 약자로, 파일을 다른 위치에 복사하고자 할 때에 사용한다. 현재 directory2에 위치해 있는 hi.txt를 directory1로 이동시킬 때 아래와 같은 명령어를 입력한다.

`cp`는 파일을 복제한 새로운 파일을 생성하는 것이므로, 원본 파일을 삭제하지 않는다. 즉, directory1과 directory2에 동일한 내용을 가진 파일이 존재하게 되는 것이다.

`cp hi.txt ../directory1`

### rm

`rm`은 `remove`의 약자로, 파일을 삭제할 때에 사용한다. hi.txt를 삭제하려면 아래와 같이 입력한다.

`rm hi.txt`

`rm`에는 `-rf`옵션을 사용할 수 있다. 여기서 `-r`옵션은 recursive의 약자로, `rm`의 동작을 재귀적으로 수행하라는 의미이다. 
`-f`는 forced의 약자로 삭제 확인 과정을 거치지 않음을 의미한다. 즉, `rm -rf directory1`과 같이 입력하면 directory1을 포함하여 그 안에 있는 내용물들까지
확인 없이 바로 모두 삭제하라는 의미가 된다. 따라서, 내용물이 존재하는 디렉토리를 삭제할 때에는 `rm -rf`를 사용하면 된다.

`rm -rf`는 휴지통으로 거치지 않고 시스템 상으로 바로 삭제가 되기 때문에 주의해서 사용해야 한다.


### man

`man`은 manual의 약자로, 리눅스에서 사용할 수 있는 모든 명령어의 사용법을 출력시킬 때 사용한다.

예를 들어, `ls` 명령어에 어떤 옵션을 붙일 수 있는지 알아보거나, `cp` 명령어의 자세한 사용법을 알고 싶은 경우 사용한다.

`man 명령어_이름`의 형태로 사용하며, 명령어를 입력하면 매뉴얼이 열리게 된다.

`man ls` 

필요한 내용을 확인한 후에 `q`를 눌러서 매뉴얼 창을 닫을 수 있다.


### getopt 명령어

* 배경

    잘 작성된 프로그램은 환경이나 설정 등을 가정하지 않고 사용자의 옵션(option)과 실행 인수(argument)에 따라 유연하게 다른 동작을 하도록 구현하고 있다. 프로그램에서 사용자의 옵션을 처리를 지원하기 위해 Unix 에는 전통적으로 getopt 라는 라이브러리가 있었고 개발자는 이 라이브러리를 사용하여 사용자의 입력을 처리할 수 있었다.

    전통적인 유닉스의 getopt 함수는 사용자의 옵션 입력을 파싱하기 위해 옵션은 대쉬 문자(-)로 시작하고 뒤에 한 개의 옵션 문자가 오며 그 후에 실행 인수를 받을 수 있도록 설계하였다.예로 프로그램 사용을 위한 도움말을 출력하는 경우 -h 또는 -? 옵션이 입력되면 도움말을 출력하고 종료되게 구현한 경우가 많으며 버전을 확인하기 위한 옵션은 -v(또는 대문자 -V) 를 사용하고 있다.

    GNU 재단은 이를 확장하여 GNU getopt 라는 라이브러리를 만들어서 GNU 의 표준 라이브러리(glibc 패키지)에 포함시켰다. GNU getopt 는 기존의 getopt 처럼 한 개의 - 로 시작하는 짧은 옵션을 처리할 수 있었고(short option 이라고 한다) 두 개의 - 로 시작하고 명령어 전체를 입력하는 긴 옵션(long option) 기능도 추가하였다. 긴 옵션을 지원하는 프로그램의 경우 도움말을 보는 옵션인 -h 또는 --help 두 가지 방식의 옵션을 모두 사용할 수가 있다.

* 설명

    getopt 명령은 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석합니다. 플래그는 단일 ASCII 문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 합니다. 인수에는 복수 바이트를 포함시킬 수 있지만 플래그 문자로는 포함시킬 수 없습니다.

    getopt 명령은 모든 토큰을 읽은 후 또는 특수 토큰 -(더블 하이픈)이 발생하는 경우 처리를 완료합니다. 그러면 getopt 명령은 처리된 플래그, -(더블 하이픈) 및 남아 있는 토큰을 출력합니다.

    토큰이 플래그와 일치하는 데 실패하는 경우 getopt 명령은 메시지를 표준 오류에 기록합니다.


* 구문

  **getopt** optstring parameters

  **getopt** [options] [--] optstring parameters

  **getopt** [options] -o|--options optstring [options] [--] parameters


* 옵션

    |Tag|Description|
    |----|----|
    |-a, --alternative|단일 '-'로 시작하는 긴 옵션을 허용한다.|
    |-h, --help|도움말 텍스트를 표시하고 종료한다. 다른 출력은 생성되지 않는다.|
    |-l, --longoptions *longopts*|긴(다중 문자) 옵션. 쉼표로 이름을 구분하여 두 개 이상의 옵션 이름을 동시에 지정할 수 있다.|
    |-n, --name *progname*|오류를 보고할 때 getopt(3) 루틴에서 사용할 이름. getopt(1)의 오류는 여전히 getopt에서 발생하는 것으로 보고된다.|
    |-o, --options *shortopts*|짧은(한 문자) 옵션. 이 옵션을 찾을 수 없으면 '-'로 시작하지 않는 getopt의 첫 번째 매개 변수가 짧은 옵션 문자열로 사용된다. shortopt의 각 짧은 옵션 문자는 필수 인수를 나타내기 위해 하나의 콜론이 뒤에 오고 선택적 인수를 나타내기 위해 두 개의 콜론이 뒤에 올 수 있다.|
    |-q, --quiet|getopt(3)에 의한 오류 보고를 비활성한다.|
    |-Q, --quiet-output|정상 출력을 생성하지 않는다. -q 를 사용하지 않는 한 오류는 getopt(3)에 의해 보고된다.|
    |-s, -shell *shell*|셸의 따옴표 규칙을 설정한다. -s 을 사용하지 않으면 BASH 규칙이 사용된다.|
    |-T, --test|getopt(1)가 이 enhanced version인지 테스트한다. 출력이 생성되지 않으며 오류 상태를 4로 설정한다.|
    |-V, --version|버전 정보를 표시하고 종료한다. 다른 출력은 생성되지 않는다.|


* 예제

    ```sh
     #!/usr/bin/bsh
     # parse command line into arguments
     set -- `getopt a:bc $*`
     # check result of parsing
     if [ $? != 0 ]
     then
             exit 1
     fi
     while [ $1 != -- ]
     do
             case $1 in
             -a)     # set up the -a flag
                     AFLG=1
                     AARG=$2
                     shift;;
             -b)     # set up the -b flag
                     BFLG=1;;
             -c)     # set up the -c flag
                     CFLG=1;;
             esac
             shift   # next flag
     done
     shift   # skip --
     # now do the work
    ```
---




### getopts 명령어
![getopts](https://user-images.githubusercontent.com/94420917/142726714-39ac6bf2-6ead-4339-9ecb-652ca7447792.png)

쉘 에서 명령을 실행할 때 주로 옵션을 사용한다. 스크립트 파일이나 함수를 실행할 때도 동일하게 옵션을 사용할 수 있다. 사용된 옵션은 다른 인수들과 마찬가지로 $1, $2, ... positional parameters 형태로 전달되므로 스크립트 내에서 직접 옵션을 해석해서 사용해야 된다. 이때 옵션 해석 작업을 쉽게 도와주는 명령이 getopts 이다. 옵션에는 short 옵션과 long 옵션이 있는데 getopts 명령은 short 옵션을 처리한다.

---




### sed 명령어

---




### awk 명령어

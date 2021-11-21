# Open-source-SW
Open-source-SW
### 과제1

1) getopt 명령어
2) getopts 명령어로의 발전
3) sed 명령어
4) awk 명령어
---

![science-g71c499ed0_1920](https://user-images.githubusercontent.com/86939460/142568721-c38e99ee-e605-4074-8112-eb313f434945.jpg)
###### (본 글에 있는 이미지들은 전부 저작권이 없는 무료 이미지를 사용하였습니다.)

### getopt 명령어
(getopts와 헷갈리지 말 것!)
> 예상되는 플래그와 인수를 지정하는 형식을 사용하여 토큰 리스트를 구문 분석한다. 플래그는 단일 ASCII 문자이며 뒤에 :(콜론)이 올 경우 하나 이상의 탭 또는 공백으로 분리하거나 분리할 수 없는 인수가 있어야 한다.
###### 인용문 출처: https://www.ibm.com/docs/ko/aix/7.2?topic=g-getopt-command

* long 옵션의 처리(--가 붙는 옵션, --posix, --wrning level 등)

1) option은 그냥 나열하면된다.
2) argument를 가지는 옵션은 :를 뒤에 붙인다.
```c
getopt -o a:bc // getopts와 똑같다. -a, -b, -c 를 옵션으로 가지고 -a는 argument가 있다는 뜻.
```
3) long option은 -l을 사용하고 ','로 구분한다.
```c
getopt -l help,path:,name:
```

4) 마지막에 -- "$@"를 붙인다.(short, long 둘다 해당)
5) invalid option이나 argument가 빠진 경우 오류메시지를 출력한다.

***

### getopts 명령어

> getopts 명령은 매개변수 리스트에서 옵션 및 옵션 인수를 검색하는 Korn/POSIX 쉘 내장 명령입니다. 옵션은 +(더하기 부호) 또는 -(빼기 부호)로 시작하고 그 뒤에 문자가 옵니다. + 또는 -로 시작하지 않는 옵션은 OptionString을 종료합니다.
###### 인용문 출처: https://www.ibm.com/docs/ko/aix/7.2?topic=g-getopts-command
 ```c
 /**********************************************************************
 * exmple source – using option                                        *
**********************************************************************/
 
#include <stdio.h>
#include <unistd.h>
 
extern char *optarg;
 
void AddProc();
void ListProc();
void Usage(const char *pname);
int main(int argc, char **argv)
{
    int option=0;
 
    while((option = getopt(argc, argv, "a:l"))!=EOF)
    {
        switch(option)
        {
            case 'a': AddProc(); break;
            case 'l': ListProc(); break;
            default: Usage(argv[0]); break;
        }
    }
    printf("good bye...\n");
    return 0;
}
 
void AddProc()
{
    printf("=== Add Proc ===\n");
    printf("Add parameter is %s\n", optarg);
}
 
void ListProc()
{
    printf("=== List Proc ===\n");
}
 
void Usage(const char *pname)
{
    printf("Usage: %s [options]\n",pname);
    printf("option: [a] | [l] \n");
    printf("a need Add parameter\n");
    printf("ex) %s -a Hello\n", pname);
    printf("ex) %s -l\n", pname);
}
 ```
###### (getopt 함수는 이번에 발견한 옵션의 아스키코드 값을 반환하며 옵션 뒤에 인자를 사용하고자 한다면 이미 선언한 optarg를 이용합니다. 그리고 더 이상 옵션을 발견하지 못하면 EOF를 반환.)

![화면 캡처 2021-11-19 153201](https://user-images.githubusercontent.com/86939460/142576179-3c3f2635-cfb8-47f8-b128-63b35b4a4856.png)

<결과값>

###### 코드 참고 링크
<https://ehpub.co.kr/%EB%A6%AC%EB%88%85%EC%8A%A4-%EC%8B%9C%EC%8A%A4%ED%85%9C-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-7-4-%EB%AA%85%EB%A0%B9%ED%96%89-%EC%9D%B8%EC%9E%90-%ED%99%9C%EC%9A%A9%ED%95%98%EA%B8%B0-getopt/>


***

### sed 명령어
> SED는 Stream Editor의 약자로 sed라는 명령어로 원본 텍스트 파일을 편집하는 명령어

리눅스의 에디터중에서는 vi 편집기가 대표적이다. vi편집기로 편집할때는 vi filename의 명령을 이용해서 파일을 열고 난 이후에 각종 vi 명령어를 통해서 추가, 삭제, 변경 등의 편집을 하게 된다. 그리고 작업이 다 끝나게 되면 저장 후 나가게 되는데 이때 원래의 파일을 변경하여 저장하기 때문에 원본파일이 변경된다. vi는 실시간 저장을 할 수가 있는데(대화형) 이러한 점이 sed와 vi 편집기와의 차이점 입니다.

 * SED와 vi편집기의 차이점 정리
1. sed는 명령어 형태로 편집이 되며 vi처럼 실시간 편집이 아니다.
2. seds는 원본을 건드리지 않고 편집하기 때문에 작업이 완료되었어도 기본적으로 원본파일에는 전혀 영향이 없다.(단, 여러분이 sed옵션에서 -i 옵션을 지정한다면 원본을 바꾸게 됨.)

sed명령어는 동작시 내부적으로 특수한 저장 공간인 두개의 버퍼를 사용한다. 이 버퍼는 __패턴 버퍼(= 패턴 스페이스)__ 와 __홀드 버퍼(= 홀드 스페이스)__ 이다.
설명을 덧붙이자면 sed는 InputStream으로 파일의 내용을 가져오고 후에 패턴 버퍼에 그 내용을 담고 있으며 데이터의 변형과 추가를 위해 다시 임시 버퍼를 사용하게 되는데, 그게 홀드 버퍼이다. 그리고 작업이 전부 완료되었다면 패턴 버퍼에 그 내용이 담기는데, 그 내용을 OutputStream으로 보내주게 되면 비로소 우리가 원하는 결과가 출력된다.(기본적으로 OutputStream은 콜솔화면인 stdout)

***

### awk 명령어
대부분의 리눅스 명령어들이 그 명령의 이름으로 대략적인 기능이 예상이 된다.(ex : rm -remove) 하지만 awk 명령어는 최초에 awk 기능을 개발한 사람들의 이니셜을 조합하여 만든 이름이기 때문에 그 기능을 이래 짐작하기는 쉽지 않다.
__A__ ho + __W__ einberger + __K__ ernighan. 
간단한 연산자를 명령라인에서 사용할 수 있으며, awk는 데이터를 조작할 수 있기 때문에 쉘 스크립트에서 사용되는 필수 툴이며, 작은 데이터베이스를 관리하기 위해서도 필수입니다.

#### 사용법
awk 명령어를 입력한 다음, 작은따옴표로 둘러싸인 패턴이나 액션을 입력하고 마지막엔 입력 파일 이름. 파일 이름을 지정하지 않으면 키보드 입력에 의한 표준 입력을 받고 awk는 입력된 라인들의 데이터들을 공백 또는 탭을 기준으로 분리해 $1부터 시작하는 각각의 필드 변수로 분리해 인식한다. 


>> awk의 기본 사용법은 패턴(pattern)과 액션(action)을 정의하여 입력으로 주어진 파일의 데이터를 가공하여 출력합니다. 패턴과 액션 중 하나만 사용하여도 무관.
>> awk에서는 레코드가 $0을 나타내며 $1, ..., $N 은 열(필드)을 나타냅니다.

```
awk 'pattern' filename
awk '{action}' filename
awk 'pattern {action}' filename
```
> ex

* $ desc testfile  

|이름|학번|분반|
|------|---|---|
|이라라|20180000|03|
|김나나|20190000|02|

__ awk '/이라라/' testfile__  //'이라라'이라는 문자를 포함하는 열을 출력
출력값: 이라라	20185014	03

```C
awk '{ if ( $5 >= 80 ) print ($0) }' ./파일이름 //5번째 필드 숫자가 80보다 크거나 같으면 출력
                                               //액션에는 if문이 존재
```


![children-ga2dbb1d43_1920](https://user-images.githubusercontent.com/86939460/142583293-2869ddde-0ae7-4777-96fb-31ab6640197f.jpg)

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
> 명령행 인자를 효과적으로 활용하기 위해 제공하는 getopt 함수

getopt 함수에 첫 번째와 두 번째 인자는 main 함수의 argc와 arg를 그대로 전달하고 세 번째 option에 제공하고자 하는 옵션을 전달한다. 만약 ‘a’, ‘l’ 옵션을 전달하고자 한다면 “al”이라고 전달하고 옵션 뒤에 인자를 사용해야 한다면 ‘:’을 추가한다. 만약 ‘a’에는 옵션 뒤에 인자를 사용하고 ‘l’에는 옵션 뒤에 인자를 사용하지 않는다면 “a:l”로 전달한다.
(프로그램 실행에 의해 main()함수에서 넘겨진 argc와argv는 인자의 수와 배열을 나타낸다.)

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

### getopts 명령어

***

### sed 명령어
> SED는 Stream Editor의 약자로 sed라는 명령어로 원본 텍스트 파일을 편집하는 명령어

리눅스의 에디터중에서는 vi 편집기가 대표적이다. vi편집기로 편집할때는 vi filename의 명령을 이용해서 파일을 열고 난 이후에 각종 vi 명령어를 통해서 추가, 삭제, 변경 등의 편집을 하게 된다. 그리고 작업이 다 끝나게 되면 저장 후 나가게 되는데 이때 원래의 파일을 변경하여 저장하기 때문에 원본파일이 변경된다. vi는 실시간 저장을 할 수가 있는데(대화형) 이러한 점이 sed와 vi 편집기와의 차이점 입니다.

 * SED와 vi편집기의 차이점 정리
1. sed는 명령어 형태로 편집이 되며 vi처럼 실시간 편집이 아니다.
2. seds는 원본을 건드리지 않고 편집하기 때문에 작업이 완료되었어도 기본적으로 원본파일에는 전혀 영향이 없다.(단, 여러분이 sed옵션에서 -i 옵션을 지정한다면 원본을 바꾸게 됨.)

sed명령어는 동작시 내부적으로 특수한 저장 공간인 두개의 버퍼를 사용한다. 이 버퍼는 __패턴 버퍼(= 패턴 스페이스)__ 와 __홀드 버퍼(= 홀드 스페이스)__ 이다.
설명을 덧붙이자면 sed는 InputStream으로 파일의 내용을 가져오고 후에 패턴 버퍼에 그 내용을 담고 있으며 데이터의 변형과 추가를 위해 다시 임시 버퍼를 사용하게 되는데, 그게 홀드 버퍼이다. 그리고 작업이 전부 완료되었다면 패턴 버퍼에 그 내용이 담기는데, 그 내용을 OutputStream으로 보내주게 되면 비로소 우리가 원하는 결과가 출력된다.(기본적으로 OutputStream은 콜솔화면인 stdout)


![children-ga2dbb1d43_1920](https://user-images.githubusercontent.com/86939460/142583293-2869ddde-0ae7-4777-96fb-31ab6640197f.jpg)

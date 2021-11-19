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

__옵션을 분석할 수 있게 제공하는 시스템 호출이 getopt 함수__ 이다.

getopt 함수에 첫 번째와 두 번째 인자는 main 함수의 argc와 arg를 그대로 전달하고 세 번째 option에 제공하고자 하는 옵션을 전달한다.

만약 ‘a’, ‘l’ 옵션을 전달하고자 한다면 “al”이라고 전달한다. 그리고 옵션 뒤에 인자를 사용해야 한다면 ‘:’을 추가한다.

만약 ‘a’에는 옵션 뒤에 인자를 사용하고 ‘l’에는 옵션 뒤에 인자를 사용하지 않는다면 “a:l”로 전달한다.

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

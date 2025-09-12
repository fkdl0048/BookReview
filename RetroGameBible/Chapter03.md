# Chapter03: 기초 다지기

기초 다지기에선 C언어로 프로그래밍하기 위한 기초 예제와 라이브러리 사용법을 다룬다.

- HelloWorld
- 콘솔모드 HelloWorld
- 타이머
- 외부 라이브러리 활용하기
- 배경음악
- 효과음

ubox msx 라이브러리를 사용하여 개발한 게임의 타켓 플랫폼은 MSX1이다. 따라서 MSX1의 상위 머신에서도 실행 가능하다. 용량은 32KB로 한정된다. 매우 작은 용량이지만 그 시절 게임들은 다 이정도 용량이었으니 충분히 게임을 만들 수 있다.

용량의 제약으로 픽셀기반이 아닌 타일 기반의 스크롤을 사용한다.

- ubox MSX 라이브러리 구성 요소
  - ubox 라이브러리: 게임 제작을 위한 기본 프레임워크
  - mplayer 라이브러리: 배경음악 및 사운드 처리
  - spman 라이브러리: 스프라이트 처리

## HelloWorld Example

```c
#include "ubox.h" // ubox MSX 라이브러리 헤더파일

#define LOCAL
#include "tiles.h"

#define WHITESPACE_TILE 129

void put_text(uint8_t x, uint8_t y, const uint8_t *text)
{
    while (*text)
        ubox_put_tile(x++, y, *text++ + 128 - 31);
}

void main()
{
    ubox_init_isr(2); // 초당 프레임을 설정한다. (30fps)

    ubox_set_mode(2); // 화면 스크린 모드 2로 설정
  
    ubox_set_colors(1, 1, 1);

    ubox_disable_screen();

    ubox_set_tiles(tiles);
    ubox_set_tiles_colors(tiles_colors);
  
    ubox_fill_screen(WHITESPACE_TILE);
 put_text(11, 11, "HELLO WORLD!!");
  
    ubox_enable_screen();


    while (1)
    { 
        ubox_wait();
    }
}
```

문자 이미지를 포함한 타일 이미지는 files.h에 정의되어 있다. 실제 png파일로부터 생성되었으며 이를 바이트 배열로 변환하여 사용한다. *여담이지만 영어가 확실히 철자가 적기에 한글표현보단 효율적인 것 같다.*

바이오스 문자 출력도 가능하지만 화면에 이미지를 표시하는 화면모드 2이기 때문에 png파일을 타일화하여 사용한 것 (타일 하나의 크기는 8x8픽셀로 가로 32개 세로 8개로 구성된다. 즉 타일을 표현하려면 2048바이트가 필요하다.)

`ubox_init_isr`이 함수는 인터럽트 핸들러를 설치하고 초기화하는 함수이다. 기초적인 인터럽트 용어 개념과 동일하며 첫 번째 인자인 `wait_ticks`는 함수가 대기할 틱 카운트 값이다.

이 개념은 유니티 Update, 언리얼 Tick과 유사한 개념으로 일정 시간마다 호출되게 하며 환경에 따라 다르지 않도록 해준다.

### ctr0.z80

일반적으로 win32이든 응용프로그램이든 main함수로 진입하기 전에 응용프로그램을 초기화하는 과정이 필요하다. 이를 `스타트업 코드`라고 부르며 보통 ctr0라는 이름의 파일에 정의한다.

즉, 초기화를 진행한 후에 실제 main함수가 실행하게 되는 것이다. ctr0.z80코드는 ubox MSX 라이브러리를 사용해서 롬을 제작할 때 공통적으로 사용하는 파일이다.

```assembly
.module crt0
// 외부의 main함수를 참조한다는 것을 명시
.globl _main

...

// 롬 헤더
 ; ROM header
 .str "AB" // 실제 롬 파일 시그니처로 "AB"를 사용한다.
                //텍스트 파일로 열면 해당 문자열이 보인다.
 .dw _main_init
 .db 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0

_main_init::
// 스택 초기화
 ; init the stack
 di
 ld sp, #0xf380 //sp는 스택 포인터의 약자이며 0xf380번지를 스택 베이스 주소로 지정한다.
 ei

// C 메인 엔트리 호출
 call gsinit
 call _main
    // _main을 쓴 이유는 C언어에서 main함수를 _main으로 변환하기 때문이다. (링커 옵션에 따라 다를 수 있다. 네임 맹글링) 

```

make 파일 부분은 코드나 파일간의 종속성과 우선순위를 지정하는 부분이다. 과거에 C언어를 공부할 때 make파일을 작성하는 법을 배웠던 기억이 난다.

## 콘솔모드 HelloWorld

콘솔모드에서 문자열 출력을 위해 `printf`함수를 사용한다. 해당 함수는 어셈블리 언어로 작성되어 있다.

```assembly
.globl _printf

CHPUT = 0x00a2

_printf::
 di
Loop: 
 ld a,(hl)
 and a
 jr z,Done
 call CHPUT
 inc hl
 jr Loop
Done:
 ei
 ret
```

마찬가지로 네임 맹글링으로 인해 전역 심볼로 `_printf`를 사용한다. CHPUT는 바이오스 입출력 서비스를 호출한다는 의미로 레지스터에 담겨있는 문자를 화면에 출력하라는 명령이다.

이와 같이 C언어로 C함수를 출력하지 않고 어셈블리로 작성된 함수를 사용하면 된다. *앞서 말한 C언어의 문제를 해결한다.*

## 타이머

타이머는 인터럽트를 사용해서 구현한다. 앞서 구현한 게임에서 일정 프레임 레이트를 유지하기 위해 활용했다. 다만 현대의 프로그래밍에서 많이 발생하는 경쟁상태에 대해서는 걱정할 필요가 없다.

## 외부 라이브러리 활용하기

매번 어셈블리로 함수를 작성할 수 없기 때문에 외부 라이브러리를 활용하면 편리하다.

```c
void put_text(uint8_t x, uint8_t y, const uint8_t *text)
{
    while (*text)
        ubox_put_tile(x++, y, *text++ + 128 - 31);
}

uint8_t g_x = 0;
void _putchar(char character)
{
 ubox_put_tile(g_x++, 0, character + 128 - 31); 
}
```

`ubox_put_tile`을 한번 래퍼로 감싸서 사용하여 문제를 피한다. *가능하면 실수연산을 피하자 지원하지 않고 이를 피하는 방법이 있지만 비활성화가 더 적합하다.*

## 배경음악

`mplayer`라이브러리를 사용하여 배경음악을 재생한다. 효과음도 비슷하게 사용한다.

## 정리

C문법을 이해하는데 어려움이 있다면 조금은 어려울 수 있을 것 같다. 또한, 리눅스, makefile, 어셈블리 개념들도 마찬가지다.

그외에는 정리가 너무 잘되어 있고 문제없이 잘 따라갈 수 있어서 만족스럽다. 다음 챕터에선 직접 게임을 만들어본다.

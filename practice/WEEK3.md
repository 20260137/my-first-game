1. 배경색 변경 시 게임이 실행되지 않는 문제

어려움
배경을 검은색으로 바꾸기 위해
BLACK = (0,0,0)으로 수정했지만, 게임이 멈추거나 실행되지 않는 것처럼 보이는 문제가 발생함.

원인

screen.fill(WHITE)를 그대로 사용해서 실제 배경이 바뀌지 않음

텍스트 색도 검정으로 설정되어 화면에 안 보이면서 “멈춘 것처럼” 보일 수 있음

 해결 방법

배경을 검정으로 바꾸기 위해

screen.fill(BLACK)

로 수정

텍스트 색상을 흰색으로 변경

font.render("텍스트", True, WHITE)

2.한글 텍스트가 네모(□)로 깨지는 문제

어려움
게임 종료 시 "게임 클리어!" 같은 한글 텍스트가 네모 박스로 표시됨.

원인

pygame.font.SysFont() 기본 폰트는 한글을 지원하지 않음

해결 방법

한글 지원 폰트로 변경

font = pygame.font.SysFont("malgungothic", 36)
end_font = pygame.font.SysFont("malgungothic", 72)

또는 영어로 대체

"GAME CLEAR!"


3.별(장애물)과 플레이어 충돌 처리 설계 문제

어려움
처음에는 별에 부딪히면 게임이 바로 종료되었는데,
두 플레이어 게임에서는 한 명만 사라지도록 바꾸고 싶었음.

원인

기존 구조가 “게임 전체 종료” 기준으로 설계되어 있음

플레이어별 상태 관리가 없음

해결 방법

플레이어 상태 변수 추가

red_active = True
blue_active = True

충돌 시 해당 플레이어만 제거

if red_rect.colliderect(star):
    red_active = False

두 플레이어 모두 사라졌을 때만 게임 종료

if not red_active and not blue_active:
    game_over = True

# 11주차 실습기록

## 1. 오늘 한 것

- PyInstaller 설치 및 빌드
- `resource_path()` 함수 추가
- `--add-data` 옵션으로 에셋 포함
- `.exe` 실행 확인



# 2. 빌드 명령어

## 기본 빌드

```bash
pyinstaller game.py
```

## 하나의 exe 파일로 빌드

```bash
pyinstaller --onefile game.py
```

## 콘솔창 없이 exe 파일 생성

```bash
pyinstaller --onefile --windowed game.py
```

## 에셋 폴더 포함하여 빌드

```bash
pyinstaller --onefile --windowed --add-data "assets;assets" --name=MyGame game.py
```



# 3. AI 활용 내역

## 질문

`"assets;assets"` 없이 에셋 파일을 불러오는 방법은?

## 답변

`--add-data` 옵션을 사용할 때 폴더 전체 대신 파일 규칙(패턴)을 지정하거나 와일드카드(`*`)를 사용할 수 있다.

예시:

```bash
pyinstaller --onefile --windowed --add-data "*.png;." --add-data "*.ogg;." --name=MyGame game.py
```



# 4. resource_path() 를 사용하는 이유

실제 파일이 있는 위치와 프로그램이 인식하는 위치가 빌드 과정에서 달라지기 때문이다.

`.exe` 파일로 빌드하는 과정에서 경로가 꼬이면 에셋 파일을 제대로 불러오지 못하는 문제가 발생할 수 있다.

이를 방지하기 위해 개발 중과 빌드 후 모두 동작하는 경로로 변환해주는 `resource_path()` 헬퍼 함수를 사용한다.

즉, 빌드 과정에서 경로가 변경되더라도 에셋 파일이 사라지지 않고 정상적으로 불러와지도록 도와준다.

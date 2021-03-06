# Devtools

개발시 서버 재가동 없이 변경점 빌드, 테스트 서버 배포 (static 파일 포함)

## 설치

gradle dependencies에 추가

`runtime('org.springframework.boot:spring-boot-devtools')`

## Ide 설정

### IntelliJ IDEA

자동 빌드 설정

- Settings -> Build -> Compiler
- `Build project automatically` 체크

실행중 빌드 설정

- `crtl+shift+A`를 눌러서 기능검색
- `Registry`실행
- `compiler.automake.allow.when.app.running` 체크

### Eclipse

기본 설정이 자동빌드이므로 의존성만 추가해도 작동함

## `application.yml` 설정 (별도 설정 없이도 작동함)

아무것도 설정하지 않으면 기본 값으로 작동함

(템플릿엔진 cache 기본값: false `DevToolsPropertyDefaultsPostProcessor.java`)

```yml
devtools:
  livereload:
    enabled: true # 기본값이 true임
  restart:
    additional-exclude: [static/**, public/**] # 변경되었을때 재시작 하지 않을 파일경로 추가 (static 파일은 기본값에 등록되어 있음)
```

## LiveReload

소스 변화가 있으면 App이 자동으로 브라우저 새로고침을 일으킴, <livereload.com>에서 브라우저용 플러그인을 설치할 수 있음

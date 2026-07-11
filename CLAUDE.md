# 렙블룸 (RepBloom)

플릭 스타일 운동 기록 PWA. 세트·무게·횟수를 탭탭탭으로 빠르게 기록하고, 루틴·휴식타이머·성장 그래프·기기 간 동기화를 제공.

## 기능(확장판)
- 6탭: 오늘(세션 기록)·루틴·기록(캘린더)·통계·몸·설정
- **빠른 기록**: 세트 스테퍼(±2.5kg/±1회), 지난기록 프리필, 세트완료→휴식타이머 자동
- **세트 타입**: 세트번호 탭 → 워밍업(W)/드롭(D)/실패(F) 순환. 워밍업은 볼륨·PR 제외
- **개인기록(PR)**: 세트 완료 시 역대 최고 무게/1RM 경신하면 축하 배너(운동당 세션 1회)
- **운동별 히스토리**: 세션 중 운동 이름 탭 → PR 4종 + 지난 기록 전체
- **통계**: 주간목표 링, 1년 잔디 히트맵, 근육 히트맵(앞/뒤 신체도), PR 목록, 부위별 볼륨, 운동별 성장 그래프(Epley 1RM)
- **몸**: 체중·가슴·허리·팔·허벅지 기록 + 증감 표시 + 체중 추세 그래프

## 스택
- 바닐라 JS + HTML + CSS (빌드 없음), PWA(오프라인)
- 저장: localStorage (기본) → Supabase REST 동기화(선택, `js/config.js`에 키 입력 시 활성화)
- 배포: GitHub Pages (`skdyddns-max.github.io/repbloom` 예정)

## 실행
```bash
bash run.sh          # http://localhost:8035
```

## 구조
- `index.html` — 앱 셸(탭 5개 + 모달 + 휴식 오버레이)
- `js/config.js` — Supabase 키 + 브랜드명(`BRAND`). **이름 바꾸려면 여기만 수정**
- `js/data.js` — 운동 DB(부위 8·기본운동 50여종), state, 저장/불러오기
- `js/app.js` — 세션 기록·세트 스테퍼·휴식타이머·루틴·캘린더·통계
- `js/sync.js` — 기기 간 동기화(코드 6자리, id 합집합 병합)
- `sw.js` — 서비스워커. **배포 시 VERSION 증가 필수**
- `supabase/schema.sql` — 동기화 테이블 DDL

## 데이터 모델
```
state = { profile:{name,unit}, customExercises:[], routines:[{id,name,exIds}],
          sessions:[{id,date,start,end,secs,entries:[{exId,name,part,type,sets:[{w,r,done}]}],note}],
          settings:{restDefault}, account:{code,lastSync} }
```
운동 type: `wr`(무게+횟수) `br`(맨몸횟수) `time`(분) `dist`(km+분)

## 동기화 켜기
1. Supabase 프로젝트 생성 → SQL Editor에 `supabase/schema.sql` 실행
2. Settings→API의 URL·anon key를 `js/config.js`에 입력
3. 앱 설정 탭 → "동기화 코드 만들기" → 다른 기기에서 그 코드로 "참여"

## 다음 후보
- 운동별 메모/난이도, 슈퍼세트, 주간 목표, 신체 부위 히트맵
- 홈 화면 위젯, 데이터 CSV 내보내기

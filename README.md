# 📚 프론트엔드 개발자를 위한 자바스크립트 프로그래밍

## 팀원

- [younho9](https://github.com/younho9)

- [taenykim](https://github.com/taenykim)

## 스터디 규칙

1. 일주일 마다 정해진 분량만큼 학습 후 md 문서로 요약 정리 하기

2. 커밋 후 PR 보내기

3. 일주일 동안 피드백 주고 받기

- 의견이 있는 부분 선택 후 의견 작성

- 피드백 주고 받기 (의견이 없어도 읽고 확인하기)

4. 의견 교환이 끝나면 확인한 사람이 `Squash and Merge` , `Delete branch`

## PR 방법

1. 원본 repository 를 remote 로 추가 (처음에 한번만)

```
git remote add upstream https://github.com/group-studyjs/javascript-for-web-developers.git
```

2. 원본 repository 의 변경 사항 확인

```
git fetch upstream
```

3. 변경사항이 있으면 로컬 master에 merge 하기

```
git merge upstream/master
```

4. 변경사항 업데이트 후 / 또는 변경사항이 없으면 브랜치 만들어서 학습한 내용 md 문서로 작성하기

```
git checkout -b 브랜치-이름
... work ...
git commit
... work ...
git commit
```

5. 작업 내용 푸시하기

```
git push -u origin 브랜치-이름
```

6. PR 작성하기

Github 에서 `New pull request` 사용, 리뷰어 추가

7. 리뷰 후 merge 되면 로컬 master 브랜치와 Fork한 저장소 동기화 하기

```
git checkout master
git merge upstream/master
git push origin master
```

### 주의사항

> ☝️ 로컬 master에서는 직접 변경하지 말고 항상 브랜치에서 작업하기

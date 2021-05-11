---
layout: post 
# title: 
# subtitle:
category: technical
# tags: technical
description: >
  GitHub의 사소하지만 큰 변화, 기본 브랜치 변경을 따라 적용해봅니다.
image: '/assets/img/posts/github_blm.jpg'
related_posts: ''
random_posts: ''
---

# Git의 기본 브랜치 바꾸기

## "Black Lives Matter", 그리고 GitHub의 움직임

조금 된 이야기인데, 평소 로컬에서부터 리포지토리를 생성해오던 나는 어느날 GitHub의 한 리포지토리에서 브랜치 이름이 main인 것을 발견했다. 찾아보니 2020년 조지 플로이드 사망 사건 이후로 활발해진 Black Lives Matter 운동을 따라 GitHub이 10월 1일자로 기본 브랜치 이름을 바꿨다고 한다. ([The default branch for newly-created repositories is now main](https://github.blog/changelog/2020-10-01-the-default-branch-for-newly-created-repositories-is-now-main/){: target="_blank"})

![github_branch_main](/assets/img/posts/github_main.png)

기존의 기본 브랜치는 'master'로, 주인(master)과 노예(slave)를 연상시키기에 이를 'main'으로 대체한 것. (바뀐 지도 모르고 master 계속 쓰고 있었다)

## 그래서 나도 하려는데 어떻게 하면 되나요?

### 1. 기존의 GitHub 리포지토리
각 리포지토리의 Settings - Branches 에서 브랜치 이름을 변경하거나, 계정의 [Settings - Repositories](https://github.com/settings/repositories){: target="_blank"} 에서 일괄 변경할 수 있다.

### 2. 로컬 Git
이미 Git 2.28 에서 `init.defaultBranch` 라는 옵션이 추가되었다. CLI 명령 단 한줄로 손쉽게 바꿀 수 있다. ([Highlights from Git 2.28](https://github.blog/2020-07-27-highlights-from-git-2-28/){: target="_blank"})

```
git config --global init.defaultBranch main
```

혹시 내 로컬 Git의 버전이 이보다 구버전이라면, 윈도우 환경에서는 `git update-git-for-windows` 를 이용해 업데이트를 해보자. (Git Windows에서 `git update` 는 deprecated 되었다고 하며, POSIX 계열은 각 패키지 매니저를 통해 업데이트 하면 될 듯)


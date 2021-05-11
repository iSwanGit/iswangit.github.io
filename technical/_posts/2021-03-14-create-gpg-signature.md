---
layout: post 
# title: 
# subtitle:
category: technical
# tags: technical
description: >
  내 깃헙 커밋에 Verified 표시하여 간지를 내보자.
related_posts: '/technical/change-git-default-branch/'
random_posts: ''
---

# 서명으로 GitHub에서 커밋 인증받기

그렇게 오래 되지 않은 것 같은데, 예전에 GitHub을 돌다가 커밋에 'Verified'가 써있거나, 사용자 자기소개 문서에 'My GPG Key'를 써놓은 걸 발견하고 검색하다 그 정체를 알았다. 딱히 써야 할 필요성을 느껴본 적은 없었으나, 개발자로서 '인증', '서명'을 그냥 넘어갈 수 없지 ㅋㅋ 한번 써봐야겠다는 생각이 들었다.

## GPG(GNU Privacy Guard)란?
> GnuPG is a complete and free implementation of the OpenPGP standard as defined by RFC4880 (also known as PGP).

PGP(Pretty Good Privacy)라는 이메일에 쓰이는 암호화 도구를 기반으로 만든 구현체(소프트웨어)이다. 일반적으로 RSA를 사용하며, 이외에 다양한 알고리즘을 지원한다고 한다.

## GitHub에서 GPG 서명이 존재하는 이유와 작동원리
Git을 설치하고, 커밋 전에 사용자 정보를 반드시 입력하는 과정을 거친다.
```
git config --global user.name
git config --global user.email
```
그런데 이 정보는 입력하는 대로 올라가니까, 다른 사람이 맘만 먹으면 나랑 똑같은 이름이랑 이메일주소로 쓸 수도 있다. (보통은 GitHub같은 경우는 각자 이메일도 등록되어 있고, 굳이 그렇게 할 까 싶긴 한데 일단은 ...)

다른 일반적인 서명, 인증과정과 같이 비대칭 키 한 쌍을 이용해서 검증하는 방법을 쓴다. GitHub에 공개키를 등록해놓고, 로컬에서 비밀키로 커밋들을 서명하면 GitHub에서 공개키로 그 서명을 검증하는 것이다. 조금 찾아보니 GitHub Docs에 해당 내용이 있다. 자세한 내용은 [여기](https://docs.github.com/en/github/authenticating-to-github/managing-commit-signature-verification){: target="_blank"}를 참조해 보는 것으로..

## GPG 설치
기본적으로 [gnupg.org](https://gnupg.org/download/index.html){: target="_blank"}에서 바이너리 세트를 제공한다. 나는 주로 윈도우 환경에서 쓸거라 Gpg4win을 받아 설치했다.  
우분투 환경에서는 `apt install gpg`, 맥(홈브류 설치) 환경에서는 `brew install gpg`로 설치 가능하다. Chocolatey 패키지 매니저를 사용하는 윈도우에서는 `choco install gpg`로 가능할 것 같다.

## GPG 서명하기
### 키 생성
쉘에서 `gpg --gen-key`를 입력하면 Git 사용자 정보 설정하듯이 이름과 이메일을 입력 후, passphrase를 입력해주면 기본값인 RSA 알고리즘을 적용하여 키가 생성된다. 그런데 cli 툴에서 키가 생성되고 나니 `gpg --full-generate-key`로 추가 정보를 입력하여 더 견고한 키를 만들 수 있다고 하는데 이를 권장하는 듯하다. 띠옹

### Git에 키 등록
쉘에서 `gpg --list-keys`를 입력하면 아까 등록한 키의 정보를 볼 수 있다.
pub 란에 있는 문자열을 사용하면 되며, 굳이 다른 이름을 구분해서 사용하고 있지 않으니 `git config --global user.signingkey <my_key>` 로 전역 설정에 등록했다.

### Windows 10 환경에서 no secret key 에러 해결하기
```sh
gpg: skipped "<my_key>": No secret key
gpg: signing failed: No secret key
error: gpg failed to sign the data
fatal: failed to write commit object
```
아니, 왜 안되는 거야. 스택오버플로우를 찾아보니 윈도우 환경에서는 gpg 프로그램 경로를 직접 잡아줘야 제대로 작동했다는 이야기가 있다. ~~킹택오버플로우 감사합니다~~  
`git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"` (또는 PATH등록된 경우 경로 대신 gpg)

### GitHub에 공개키 등록하기
GitHub에서 서명을 검증할 공개키를 등록해야 한다. 먼저 공개키를 알아내야 한다. 쉘에서 `gpg --armor --export <my_key>`를 입력하면 "-----BEGIN PGP PUBLIC KEY BLOCK-----" 로 시작하는 키가 쭈욱 나온다. GitHub에 등록할 때 쓸 키 블럭이니 잘 복사해 두고, GitHub 설정의 [SSH and GPG keys](https://github.com/settings/keys){: target="_blank"}에서 해당 키를 등록하면 된다.

![](/assets/img/posts/gpg_register.png)

짠

### Git 커밋하고 GPG 키를 이용해 서명하기
커밋시 서명을 첨부하려면 Git CLI 기준 `-S` 옵션을 추가하면 된다. 그런데 매번 입력하기도 귀찮을테고, 본인은 SourceTree나 VSCode같은 툴을 주로 사용할 것이므로 기본적으로 서명을 하도록 `git config --global commit.gpgsign true`를 쉘에 입력한다.

커밋에 서명을 포함하게 되면, 처음에 키 생성할 때 입력한 passphrase를 다시 요구한다. 올바르게 입력 후, 커밋이 된 후 push해보면 GitHub에서 다음과 같은 Verified 문구를 볼 수 있다.

![](/assets/img/posts/gpg_thumbnail.png)

!잔짜잔!  
이제 나도 인증받은(?) 커밋을 할 수 있게 되었다.

### 추가: 서명에 여러 이름/이메일 추가하기
GitHub에 메일을 여러 개 등록해 놓아서 추가해보려고 함께 찾아봤다. [StackExchange](https://superuser.com/questions/293184/one-gnupg-pgp-key-pair-two-emails){: target="_blank"}에서 찾아는데, 한 키를 가지고 여러 유저 정보로 서명하고 싶을 때 쓰면 되려나 싶다. (user 정보를 바꾸지 않는 한 의미가 없나...? 일단 원래 쓰던 유저 프로필로 써야지.)

```sh
gpg --edit-key <my_key>
gpg> adduid
  Real Name: <name>
  Email address: <email>
  Comment: <comment or Return to none>
  Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
  Enter passphrase: <password>
  # (이 중간과정 생략하고 바로 save헀는데 기존에 --gen-key로 생성한 것과 똑같은 권한으로 나온다)
gpg> save
```

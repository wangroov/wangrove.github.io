---
title: "한 pc에서 여러 유저의 ssh-key로 github 관리하기"
date: 2019-06-27 08:26:28 -0400
categories: git ssh-key
---
## 문제
jekyll 블로그를 만드려고 안 쓰는 github id에 접속해서 새 repository 생성하였다.  
그리고 현재 pc(os는 윈도우)에서 ssh-key를 생성한 뒤 github에 등록하고 git clone을 시도하였으나..
permission denied(publickey) 에러가 뜨면서 되지 않았다.
여러 글을 검색한 끝에 한 대의 pc에서는 단순히 ssh-key를github에 등록해주는 것만으로는 안된다는 사실을 알게 되었다.  
  
## 원인
사용자 pc에서 ssk-key를 생성하고 난 뒤, public key를 github에 등록하면 매번 로그인하지 않고 편하게 github을 이용할 수 있다.  
github은 default로[user home directory]/.ssh 하위에서id_rsa 파일을 찾아 github에 존재하는 public key와 비교하여 처리하기 때문이다.  
그런데 여러 사용자의 ssh-key가 있다면 나중에 등록한 ssh-key 이름은 id_rsa가 될 수 없기 때문에 
github에서 등록된 public key와 매칭되는 private key를 못 찾아서 에러가 나는 것이다.  
즉 매칭시켜주면 해결된다.  
  
## 해결방법
1. [user home directory]/.ssh 하위에 config 파일을 만든다.  
``` touch [user hoem directory]/.ssh/config```
2. 다음과 같이 작성한다.  
<pre>
	<code>
\# Old github Account
Host github.com
	HostName github.com
	User git
	AddKeysToAgent yes
	UseKeychain yes
	IdentityFile ~/.ssh/id_rsa

\# New GitHub account
Host github.com-new
	HostName github.com
	User git
	AddKeysToAgent yes
	UseKeychain yes
	IdentityFile ~/.ssh/new_id_rsa 
	</code>
</pre>
 3. 블로그 용 github repository로 가서 ssh remote url을 복사합니다.
 4. 여기서 github.com을 새로운 ssh-key의 host로 변경해야 합니다.  
 원래 ssh remote url  
 ```git@github.com:[github nickname]/[repository name].git```  
 수정된 ssh remote url  
 ```git@github.com-new:[github nickname]/[repository name].git```
 5. 변경 사항을 add, commit, 그리고 origin으로 push 했을 때 정상 동작한다면 성공!  

 ---  
 reference :  
 - [Use multiple ssh-keys for different GitHub accounts on the same computer](https://medium.com/@xiaolishen/use-multiple-ssh-keys-for-different-github-accounts-on-the-same-computer-7d7103ca8693"미디엄")
 - [Github 다수 계정을 위한 SSH key 설정 :: 마이구미](https://mygumi.tistory.com/96"마이구미")









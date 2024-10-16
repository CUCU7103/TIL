# git reset 과 git revert



### git reset

- 돌아가려는 커밋으로 리포지토리를 재설정하고 해당 커밋이후의 이력은 사라집니다.
- 과거 커밋으로 돌아간다는 메시지를 남기지 않습니다.
- 돌아간 시점 ~ 현재(돌아가기 전)까지의 기억(commit)을 모두 삭제해버리는 것
- **reset은 가능하면 로컬 저장소에서만 실행**할 것을 권장합니다.

![image](https://github.com/user-attachments/assets/195e8ff9-f2f2-4e49-a7f9-882f40b524e8)




![image](https://github.com/user-attachments/assets/fb3dfb58-1be4-49cf-9725-11c765dabac5)


- 위의 사진에서 second commit을 실행한 시점으로 돌아가고 싶으면  아래 명령어를 수행합니다.

``` bash
$ git reset 되돌리고 싶은 커밋의 코드 --옵션
```

![image](https://github.com/user-attachments/assets/3a103150-97ab-483d-b0aa-b9241b9fc404)


- 그러면 second commit 까지의 모든 커밋 내역이 삭제되면서 second commit으로 돌아옵니다.



```bash
1) git reset . 
	- Working Directory 유지
	- Staging Area 삭제 

2) git reset <commit번호> 
	- <commit번호>버전으로 돌아가기
	- Working Directory는 유지
	- Staging Area 삭제

3) git reset --hard <commit ID> 
	- <commit번호> 버전으로 돌아가기
	- Working Directory는 삭제
	- Staging Area 삭제

4) git reset --soft <commit ID> 
	- <commit번호> 버전으로 돌아가기
	- Working Directory는 유지
	- Staging Area에 돌아간 돌아간 시점 ~ 현재까지의 작업 내용이 추가됨
```





### git revert

- 이전 커밋 내역들은 그대로 두고, 되돌리고 싶은 커밋의 코드만 복원시킵니다.

- **특정 커밋을 되돌리는 작업도 하나의 커밋으로 간주**하여 커밋 내역에 추가합니다.

- 이미 공유된 브랜치에서 커밋을 취소하고자 할 때, **특히 이미 리모트 저장소에 푸시된 상태에서 사용**



``` bash
$ git revert 되돌리고 싶은 commit의 hash
```

- *Commit A -> Commit B -> Commit C*의 순서로 커밋 내역이 쌓이는 것을 생각해보면, 이를 다시 원래대로 돌리기 위해서는 *Commit C -> Commit B -> Commit A* 거꾸로 revert를 실행하면 된다.(“실행취소”기능과 비슷하다)

- 커밋 CC 에서 버그가 발생했다고 가정하고 해당 변경 사항을 되돌리고 싶다면 **git revert**를 사용

```
A -- B -- C -- D (현재 브랜치)

# 커밋 C에서 발생한 변경 사항을 되돌림
git revert C

# 커밋 C를 취소하는 커밋 E가 생성되어집니다.
A -- B -- C -- D -- E (현재 브랜치)

```

- 여러 커밋을 되돌릴 경우에는 `..`으로 범위를 주어 `commit2..commit4` 이런 식으로 입력하면됩니다.

```
$ git revert commit2_hash..commit4_hash
```



## `--no-commit` 옵션 사용하기

하지만 여러 커밋을 되돌리는 경우에는 각 revert마다 커밋 메세지를 작성해야 하는 번거로움이 생긴다. 이때, `--no-commit` 옵션을 이용하면 revert를 위한 커밋을 하나만 생성할 수 있다.

그리고 커밋의 hash가 아닌 `되돌리고 싶은 커밋의 범위`를 인수로 입력해주면 된다.

![git no-commit(1)](https://han-joon-hyeok.github.io/assets/images/2021/2021-01-25-git-reset-revert/git_revert_no_commit(1).png)

```
$ git revert --no-commit HEAD~2..
```

`HEAD~2..`는 최근 2개 커밋을 의미한다. 현재 commit 4를 마친 상태에 있으므로, commit 4와 commit 3을 취소하게 되는 것이다.

`--no-commit`을 사용하면 복수의 revert에 대해서 각각 커밋 메세지를 남기는 대신, 하나의 커밋 메세지만 남기도록 한다. 따라서 `git add`를 완료한 상태로 되돌아가게 되며, 아래와 같이 별도의 커밋 메세지를 남겨야 한다.

![git no-commit(2)](https://han-joon-hyeok.github.io/assets/images/2021/2021-01-25-git-reset-revert/git_revert_no_commit(2).png)

위와 같이 정상적으로 여러 커밋에 대한 revert도 하나의 커밋 메세지로 처리할 수 있게 됩니다.




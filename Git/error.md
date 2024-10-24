
readme 파일이 remote에 있어서
git pull origin main --allow-unrelated-histories
해준 후
git push origin main
해줌.

obsidian 사용 중 깃헙 인증 문제 `fatal could not read userName for ...`
[이슈 탭 참고](https://github.com/Vinzent03/obsidian-git/issues/254)

토큰을 저장 안해둬서 다른 곳에서 필요할 때 인증한다고 토큰 재발급 받을 때마다 기존에 인증했던 옵시디언에서 에러가 나는 것인데 다시 인증해주는 방법으로 했지만 이슈 탭의 내용 중 원격 url을 https가 아닌 ssh로 설정하라는 내용이 있었다. 찾아보니 SSH키를 한 번 설정하면 컴퓨터에 안전하게 저장되고 생성된 공개키를 깃헙에 등록해두기만 하면 된다고 한다. [키 생성](https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key) [https -> ssh 전환](https://docs.github.com/en/get-started/getting-started-with-git/managing-remote-repositories#switching-remote-urls-from-https-to-ssh) 

[키 설정 되었는지 확인](https://docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey#make-sure-you-have-a-key-that-is-being-used)
```shell
$ eval "$(ssh-agent -s)"
> Agent pid 59566
# 아래 긴 문자열이 나와야 키 설정 완료
$ ssh-add -l -E sha256
> 2048 SHA256:274ffWxgaxq/tSINAykStUL7XWyRNcRTlcST1Ei7gBQ /Users/USERNAME/.ssh/id_rsa (RSA)
```
